---
id: 253
title: Gearman Manager调优
date: 2018-10-10T14:13:49+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/253
permalink: /archives/253
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