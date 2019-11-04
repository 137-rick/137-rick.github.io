---
id: 192
title: 'JS报错 Uncaught SyntaxError: Unexpected token ILLEGAL'
date: 2016-01-08T18:21:01+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/192
permalink: /archives/192
categories:
  - 经典技巧
tags:
  - javascript
  - 报错
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  这两天碰到一个大坑！</p> 
  
  <div>
    JS文件内回个车都会报Uncaught SyntaxError: Unexpected tokenILLEGAL
  </div>
  
  <div>
    几次三番找原因，终于找到。。。
  </div>
  
  <div>
    Uncaught SyntaxError: Unexpected token ILLEGAL
  </div>
  
  <div>
    是因为js有未识别的字符
  </div>
  
  <div>
    然后发现nginx 1.8会的content-type:application/javascript;charset=utf-8
  </div>
  
  <div>
    charset=utf-8
  </div>
  
  <div>
    问题是有的文件不是utf-8的，他也给加这个charset
  </div>
  
  <div>
    导致chrome浏览器无法解析
  </div>
  
  <div>
  </div>