---
id: 219
title: c 系列中 localtime多线程下的大坑
date: 2018-10-10T14:06:40+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/219
permalink: /archives/219
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  localtime是有一个大坑的。。。</p> 
  
  <div>
    当我们使用localtime(timestamp)的时候返回是一个指针。
  </div>
  
  <div>
    这个指针的指向是共用的，这时如果有其他线程执行了localtime。。。。会覆盖之前的值！！
  </div>
  
  <div>
    可以考虑localtime_r。。。。但是！！！他也有坑~~~
  </div>