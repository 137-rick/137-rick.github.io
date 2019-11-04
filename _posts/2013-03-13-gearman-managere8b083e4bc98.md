---
id: 160
title: Gearman Manager调优
date: 2013-03-13T16:55:12+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/160
permalink: /archives/160
categories:
  - PHP
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  今天下午调试Gearman Manager不工作原因的时候<br />strace发现有大量的rt_sigprocmask调用</p> 
  
  <p>
    觉得这样很浪费系统资源，百度了一下找到以下网址
  </p>
  
  <p>
    http://hk.tarotme.net/tag/rt_sigprocmask
  </p>
  
  <p>
    根据这个网址介绍修改<br />php/ext/pcntl/pcntl.c
  </p>
  
  <p>
    注释掉其指定函数的pcntl_signal_dispatch函数内的<br />sigfillset(&mask);<br />sigprocmask(SIG_BLOCK, &mask, &old_mask);<br />sigprocmask(SIG_SETMASK, &old_mask, NULL);
  </p>
  
  <p>
    后发现strace这里调用不会刷的那么快了……
  </p>
  
  <p>
    其中要注意重新编译php的pcntl
  </p>