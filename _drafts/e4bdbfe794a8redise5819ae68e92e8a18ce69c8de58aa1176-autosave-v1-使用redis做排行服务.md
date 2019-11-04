---
id: 270
title: 使用redis做排行服务
date: 2018-10-10T14:26:37+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/270
permalink: /archives/270
---
细节介绍，做排行建议不做全站的，除非你有很大的redis集群或使用ssdb 

那么就维持前几万用户即可……  
使用redis的zset即可

<pre class="wp-block-preformatted">$this->redis->zAdd($rankType, $score, $key);<br />if ($length > 0) { //限个数<br />$all = $this->redis->zCard($rankType);<br />if ($all > $length) {<br />    $keys = $this->redis->zRange($rankType, 0, $all -$length);<br />    foreach ($keys as $key) {<br />        $this->redis->zDelete($rankType, $key);<br />    }<br />  }<br />}</pre>

此代码来自zphp（基础swoole的实时通讯框架）