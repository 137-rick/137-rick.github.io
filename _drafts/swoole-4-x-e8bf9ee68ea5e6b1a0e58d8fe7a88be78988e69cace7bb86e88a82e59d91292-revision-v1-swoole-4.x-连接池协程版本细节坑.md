---
id: 302
title: swoole 4.x 连接池协程版本细节坑
date: 2019-02-14T17:12:57+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/302
permalink: /archives/302
---
使用swoole 4.x协程版本后，系统资源利用率提高很多，与此同时发现很多开发习惯已经不同

自从看了王晶老师（半桶水）在微信公众号php饭米粒 里发表的文章后忍不住手痒跟随教程认真轮了一个php框架 磨刀石项目，旨在深入了解协程，在试玩过程中碰到很多有意思的事情。今天感觉很经典分享一例

设计如下：

<pre class="wp-block-code"><code>&lt;?php

namespace WhetStone\Stone\Driver;

/**
 * 触发式连接池
 * 空余连接数不够才会增长连接数
 * 连接到达上限会阻塞等待指定秒数后抛异常
 */
abstract class Pool
{

    private $_pool = NULL;

    protected $_config = NULL;

    //整体最多连接数
    private $_maxObjCount = 50;

    //正在被调用个数
    private $_invokeObjCount = 0;

    /////////////////////////////////////////////////////
    //获取对象，对象必须自带重连

    /**
     * Fend_Pool constructor.
     * @param int $maxObjCount 最大连接数
     * @param float $waitTimeout 连接池满等待超时时间
     * @param array $config 数据连接配置
     */
    public function __construct($maxObjCount = 50, $waitTimeout = 3.0, $config = array())
    {
        $this->_maxObjCount = $maxObjCount;
        $this->_waitDelay   = $waitTimeout;
        $this->_config      = $config;
        $this->_pool        = new \Swoole\Coroutine\Channel($maxObjCount);

        //一次性创建好所有连接
        //失败一个会异常抛出
        //桶哥建议，好处是请求响应时间稳定
        for ($count = 0; $count &lt; $maxObjCount; $count++) {
            $obj = $this->getDriverObj();
            $this->_pool->push($obj);
        }
    }

    //对象报错时会调用这个函数整理错误

    /**
     * 对象调用操作
     * @param string $name 动作名称
     * @param array $arguments 参数
     * @return mixed
     * @throws
     */
    public function __call($name, $arguments)
    {
        $obj = null;

        try {
            $obj = $this->fetchObj();

            $result = call_user_func_array(array(
                $obj,
                $name
            ), $arguments);

            $this->recycleObj($obj);

            return $result;
        } catch (\Exception $e) {
            $this->onError($obj, $e);
            throw $e;
        }
    }

    /////////////////////////////////////////////////////

    private function fetchObj()
    {
        //pool is empty and have idle space
        if ($this->_pool->isEmpty() && ($this->_invokeObjCount + $this->_pool->length() &lt; $this->_maxObjCount)) {
            
            $obj = $this->getDriverObj(); //这里有坑！！！！！后续讲解
            $this->_invokeObjCount++;
           
            return $obj;
        }

        //channel have obj
        //fetch obj by 3 second wait
        $obj = $this->_pool->pop(3.0);

        if ($obj !== FALSE) {
            //increase count
            $this->_invokeObjCount++;
            return $obj;
        }

        //fetch fail max arrive
        throw new \Exception("fetch pool obj fail... please increase the connection pool size.", -123);
    }
    
    /**
     * 获取数据对象，return对象即可
     * @return mixed
     */
    abstract public function getDriverObj();

    private function recycleObj($obj)
    {
        if (empty($obj)) {
            return;
        }

        //decrease count
        $this->_invokeObjCount--;

        return $this->_pool->push($obj);
    }

    abstract public function onError($obj, $e);


}</code></pre>

从上图可以看到我做了一个父类，给各种驱动继承后回调方式传回不同对象，以此实现一个池管理，当有命令调用的时候直接通过__call方法将请求传递到obj内。

子类如果使用可以用如下方式使用

<pre class="wp-block-code"><code>&lt;?php

namespace WhetStone\Stone\Driver\Redis;

/**
 * 基础新版Redis驱动做的
 * 新版本明确服务器不可回应时抛出异常
 *
 * Class Redis
 * @package WhetStone\Stone\Driver\Redis
 */
class Redis
{
    private $config = null;

    private $dbName = "";

    private $redis = null;

    public function __construct($dbname, $config)
    {
        $this->dbName = $dbname;
        $this->config = $config;

        //do connect
        $this->reconnect();
    }

    //if connection is broken reconnect
    public function checkConnection()
    {
        try {
            if ($this->redis->ping() != "+PONG") {
                $this->reconnect();
            }
        } catch (\Exception $e) {
            $this->reconnect();
        }
    }

    public function reconnect()
    {
        $this->redis = new \Redis();
        
        //connect the server
        $ret = $this->redis->connect($this->config["host"], $this->config["port"], $this->config["timeout"] ?? 0);
        if (!$ret) {
            throw new \Exception("connect Redis Server fail:" . $this->redis->getLastError(), -24);
        }

        $this->redis->setOption(\Redis::OPT_SCAN, \Redis::SCAN_RETRY);
        $this->redis->setOption(\Redis::OPT_READ_TIMEOUT, -1);

        //prefix
        if (isset($this->config["prefix"]) && !empty($this->config["prefix"])) {
            $this->redis->setOption(\Redis::OPT_PREFIX, $this->config["prefix"]);
        }

        //auth
        if (isset($this->config["auth"]) && !empty($this->config["auth"])) {
            if ($this->redis->auth($this->config["auth"]) == FALSE) {
                throw new \Exception("Redis auth fail.dbname:" . $this->dbName, -23);
            }
        }

        //db
        if (isset($this->config["db"]) && !empty($this->config["db"])) {
            $this->redis->select($this->config["db"]);
        }


    }

    public function __call($name, $arguments)
    {
        //check is work well
        $this->checkConnection();

        //do the cmd，如果刚检测完还报错，那。。。
        return call_user_func_array(array($this->redis, $name), $arguments);
    }
}</code></pre>

实际运行当中却碰到了一个奇怪的问题，我在pool中规定连接池对象只能有50个，但是使用ab压测-c 1000的时候发现实际会超过我规定的数量，并且请求出现卡死现象。

经过深入及swoole作者之一twose帮助，通过gdb发现代码卡在channel->push上

<pre class="wp-block-code"><code>invoke count:236 pool len:0
invoke count:237 pool len:0
invoke count:238 pool len:0
invoke count:239 pool len:0
invoke count:240 pool len:0
invoke count:241 pool len:0
invoke count:242 pool len:0
invoke count:243 pool len:0
invoke count:244 pool len:0
invoke count:245 pool len:0
invoke count:246 pool len:0
invoke count:247 pool len:0
invoke count:248 pool len:0
</code></pre>

反向推理后发现关键问题在这里：

<pre class="wp-block-code"><code>private function fetchObj()
    {
        //pool is empty and have idle space
        if ($this->_pool->isEmpty() && ($this->_invokeObjCount + $this->_pool->length() &lt; $this->_maxObjCount)) {
            try {
                $this->_invokeObjCount++; //此处是关键，一定要放在getDriverObj前面
                $obj = $this->getDriverObj();
            } catch (\Exception $e) {
                $this->_invokeObjCount--;
                throw $e;
            }

            return $obj;
        }

        //channel have obj
        //fetch obj by 3 second wait
        $obj = $this->_pool->pop(3.0);

        if ($obj !== FALSE) {
            //increase count
            $this->_invokeObjCount++;
            return $obj;
        }

        //fetch fail max arrive
        throw new \Exception("fetch pool obj fail... please increase the connection pool size.", -123);
    }</code></pre>

故障原因为：并发请求1000时，1000个请求会同时调用池获取对象，这时new redis及connect会自动进入协程，由于计数没有先添加，其他并发的请求拿到的已经分配的_invokeObjCount个数没有超过指定数量，所以导致了1000个redis对象被new及connect，最终导致channel超过了之前规定的50&#8230;

虽然swoole4.x的协程底层只有一个线程，但是一定要识别出哪里会自动协程……

这个连接池有很大缺点，刚才讨论后发现还需要改进，如pipline，事务会有问题，稍晚会继续介绍新版本