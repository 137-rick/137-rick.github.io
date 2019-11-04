---
id: 179
title: php preg_match_all 段错误
date: 2014-07-03T11:34:44+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/179
permalink: /archives/179
categories:
  - PHP
  - 经典技巧
tags:
  - php
  - 崩溃
  - 正则
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  昨天碰到个问题php preg_match_all 执行的时候在某条数据上报错<br />怀疑是文字太长导致所以加了pcre的可使用空间，但是故障依旧<br />最后在正则结尾加上U修饰符，取消了greed模式 工作正常了……</p>