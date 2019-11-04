---
id: 237
title: Smarty模板的date_format的坑
date: 2018-10-10T14:11:40+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/237
permalink: /archives/237
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  今天看管理系统的时候发现个有趣的现象，界面列表内的时间竟然都是<b>当前时间</b>0 0b</p> 
  
  <p>
    上数据库一看，记录的时间值为0（unix_time格式保存在数据库的）
  </p>
  
  <p>
    看模板是这么写的 {$list.create_time|date_format:&#8221;%Y-%m-%d %H:%M:%S&#8221;}
  </p>
  
  <p>
    由于create_time是0
  </p>
  
  <p>
    这个竟然自动写成了当前时间……
  </p>
  
  <p>
    留给各位坑中朋友预防下……
  </p>