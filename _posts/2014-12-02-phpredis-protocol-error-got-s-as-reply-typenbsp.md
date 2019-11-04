---
id: 181
title: 'phpredis protocol error, got &#039;s&#039; as reply type&amp;nbsp'
date: 2014-12-02T17:48:40+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/181
permalink: /archives/181
categories:
  - PHP
  - 经典技巧
tags:
  - phpredis
  - protocolerror
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    今天发现某台服务器突然疯狂的报 phpredis protocol error, got&#8217;s&#8217; as reply type byte
  </div>
  
  <div>
    类似错误，找了半天……猜测是如下问题
  </div>
  
  <div>
  </div>
  
  <p>
    https://github.com/nicolasff/phpredis/issues/52
  </p>
  
  <div>
  </div>
  
  <div>
    服务器重启后正常
  </div>
  
  <div>
    怀疑是phpredis使用的是长连接某种情况下共用一个socket，特此mark
  </div>