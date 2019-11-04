---
id: 217
title: 'JS报错 Uncaught SyntaxError: Unexpected token ILLEGAL'
date: 2018-10-10T14:04:56+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/217
permalink: /archives/217
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