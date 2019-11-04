---
id: 241
title: php的curl的坑
date: 2018-10-10T14:13:26+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/241
permalink: /archives/241
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  这个坑是凯胖子发现的……<br />我们在使用curl的post的时候是用如下命令执行的</p> 
  
  <p>
    curl_setopt($ch, CURLOPT_POST, 1);<br />但是如果不设置POST参数<br />curl_setopt($ch, CURLOPT_POSTFIELDS, $params);<br />那么CURL会将POST请求变成了GET
  </p>