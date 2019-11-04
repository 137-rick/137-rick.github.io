---
id: 106
title: linux下的 lighttpd 与框架 thinkphp urlrewrite 配置
date: 2010-06-21T15:18:34+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/106
permalink: /archives/106
categories:
  - 经典技巧
tags:
  - it
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  首先修改lighttpd.conf文件<br />打开mod_rewrite前的#号注释<br />然后配置内加入<br />url.rewrite-once = (<br /> &#8220;^/index.php(.*)$&#8221; => &#8220;/index.php/$1&#8221;,<br /> &#8220;^/Public/(.*)$&#8221; => &#8220;/Public/$1&#8221;,<br /> &#8220;^/Share/(.*)$&#8221; => &#8220;/Share/$1&#8221;,<br /> &#8220;^/(.*)$&#8221; => &#8220;/index.php/$1&#8221;,<br />)<br />这样tp框架就可以打开</p> 
  
  <p>
    如果使用官方的rpm包安装的，需要重新编译lighttpd<br />需要先安装pcre<br />配置两个变量LIB以及CPPFLGS值 根据pcre-config 输出结果配置<br />然后再configure lighttpd的包<br />make<br />make install
  </p>