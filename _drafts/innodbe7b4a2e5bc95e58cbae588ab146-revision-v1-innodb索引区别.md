---
id: 251
title: innodb索引区别
date: 2018-10-10T14:13:27+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/251
permalink: /archives/251
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  主索引保存在数据中<br />从索引保存在外置文件内<br />所以innodb有的时候检索，从索引会快一些<br />如查总共数据个数count(从索引)会快些</p>