---
id: 236
title: Dora RPC 虚拟机下实测性能
date: 2018-10-10T14:10:27+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/236
permalink: /archives/236
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  经过24小时持续压力测试，目前接口仍旧工作正常</p> 
  
  <div>
  </div>
  
  <div>
    使用的vagrant虚拟进行压测的分配了1G内存和1核CPU（Mac 2.2 GHz Intel Corei7）
  </div>
  
  <div>
    <div>
    </div>
    
    <div>
      压测进程：目前只开了10个php进程疯狂发送请求</p> 
      
      <div>
        并发性能：TPS 2100上下（比直接使用curl快很多）
      </div>
      
      <div>
        响应时间：0.02～0.04s 偶尔出现0.4s
      </div>
      
      <div>
        后端代码为：查询一次数据库后返回结果
      </div>
    </div>
    
    <div>
      CPU使用：10～25%
    </div>
    
    <div>
      内存使用：一个PHP task 16M 目前开了30个进程
    </div>
    
    <div>
      PHP版本：5.4.41
    </div>
    
    <div>
      压测时使用端口个数：10个（长连接）
    </div>
    
    <div>
    </div>
    
    <div>
      测试代码使用的使用客户端示范程序无限循环，服务端直接返回一个数组。
    </div>
    
    <div>
      每次接口会请求一次api接口调用后再下发一个请求内含两个并发任务
    </div>
    
    <div>
    </div>
    
    <div>
      其他资源情况如下：
    </div>
    
    <div>
    </div>
    
    <div>
      <a HREF="http://photo.blog.sina.com.cn/showpic.html#blogid=54ef39890102vkgh&#038;url=http://album.sina.com.cn/pic/001yr0dXzy6T23d7GJJf3" TARGET="_blank"><img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://s4.sinaimg.cn/bmiddle/001yr0dXzy6T23d7GJJf3&690" NAME="image_operate_41771434179817668" ALT="Dora RPC 虚拟机下实测性能" TITLE="Dora RPC 虚拟机下实测性能" /></a></p>
    </div>
    
    <div>
      此开源使用Swoole特性制作
    </div>
    
    <div>
      <ul>
        <li>
          客户端使用长链接，处理请求结束后连接也不会断开，再次使用的时候会自动找回
        </li>
        <li>
          服务端自动管理task及进程通讯
        </li>
        <li>
          通过task处理业务
        </li>
        <li>
          如果使用更高速的序列化函数取代serialize会更快一些
        </li>
        <li>
          支持单api请求，多api并发请求，此功能可取代发展越来越怪的gearman
        </li>
        <li>
          如果有持久化请求需求，可以考虑在此基础上自行封装下（会降性能的哦）
        </li>
      </ul>
    </div>
    
    <div>
    </div>
    
    <div>
      过几天增加个中间件，可以检测后端服务压力状态自动负载均衡～
    </div>
    
    <div>
    </div>
    
    <div>
      github地址
    </div>
    
    <div>
      https://github.com/xcl3721/Dora-RPC
    </div>
    
    <div>
    </div>
  </div>