---
id: 215
title: 使用swoole制作evenloop文件更新监听
date: 2018-10-10T14:04:37+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/215
permalink: /archives/215
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    使用swoole的evenloop和php的inotify插件可以很方便的监控文件夹下的文件更新，后面也不用写死循环和sleep
  </div>
  
  <div>
    如果使用原生的php是需要写while死循环和sleep。这样效率不是很高
  </div>
  
  <div>
  </div>
  
  <div>
    $handleList = array();
  </div>
  
  <div>
  </div>
  
  <div>
    //init
  </div>
  
  <div>
    foreach ($this->_config[&#8220;log_path&#8221;] as$content) {
  </div>
  
  <div>
    $folder =$content[&#8220;path&#8221;];
  </div>
  
  <div>
  </div>
  
  <div>
    //if thefolder not exist
  </div>
  
  <div>
    if(!is_dir($folder)) {
  </div>
  
  <div>
    mkdir($folder, true);
  </div>
  
  <div>
    }
  </div>
  
  <div>
  </div>
  
  <div>
    //创建一个inotify句柄
  </div>
  
  <div>
    $handleList[$folder][&#8220;fd&#8221;] = inotify_init();
  </div>
  
  <div>
  </div>
  
  <div>
    echo&#8221;Listen the folder:&#8221; . $folder . PHP_EOL;
  </div>
  
  <div>
    //监听文件，仅监听修改操作，如果想要监听所有事件可以使用IN_ALL_EVENTS
  </div>
  
  <div>
    $handleList[$folder][&#8220;desc&#8221;] =inotify_add_watch($handleList[$folder][&#8220;fd&#8221;], $folder,IN_MODIFY);
  </div>
  
  <div>
  </div>
  
  <div>
    //加入到swoole的事件循环中
  </div>
  
  <div>
    swoole_event_add($handleList[$folder][&#8220;fd&#8221;], function ($fd) use($folder) {
  </div>
  
  <div>
    $events =inotify_read($fd);
  </div>
  
  <div>
    if ($events) {
  </div>
  
  <div>
    var_dump($folder,$events);
  </div>
  
  <div>
    foreach ($events as $event) {
  </div>
  
  <div>
    //echo&#8221;inotify Event :&#8221; . var_export($event, 1) . &#8220;\n&#8221;;
  </div>
  
  <div>
    }
  </div>
  
  <div>
    }
  </div>
  
  <div>
    });
  </div>
  
  <div>
  </div>
  
  <div>
    后续记得后面不要调用sleep也无需while死循环
  </div>
  
  <div>
    今天踩坑我在后面加了while(true){sleep(5);}死活不工作……后来问了峰哥……才知道丢人了……
  </div>
  
  <div>
  </div>
  
  <div>
    这个效率很高…dora-rpc的日志收集就这么定了，我会做更多的测试后更新github
  </div>
  
  <div>
  </div>
  
  <div>
    最后声明：这段代码的原始版本来自韩天峰的分享：http://my.oschina.net/matyhtf/blog/343508
  </div>