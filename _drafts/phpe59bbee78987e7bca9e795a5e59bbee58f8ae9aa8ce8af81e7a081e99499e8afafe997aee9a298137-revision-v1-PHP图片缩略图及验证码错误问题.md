---
id: 206
title: PHP图片缩略图及验证码错误问题
date: 2018-10-10T14:02:15+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/206
permalink: /archives/206
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  今天碰到一个很奇怪的问题，缩略图和验证码突然都不能正常使用了<br />经过排查发现<br />PHP有一个文件包含BOM头，这个头导致了输出数据不正常……<br />通过工具修改此文件去掉BOM头，这两个功能就可以正常工作了</p>