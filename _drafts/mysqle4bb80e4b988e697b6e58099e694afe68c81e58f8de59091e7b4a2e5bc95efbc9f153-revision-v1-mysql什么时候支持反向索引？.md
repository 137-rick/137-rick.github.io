---
id: 248
title: mysql什么时候支持反向索引？
date: 2018-10-10T14:13:27+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/248
permalink: /archives/248
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  在我们使用limit对含有大量数据的表进行分页的时候，越往后翻页越缓慢<br />主要原因是，mysql会把数据结果生成后排序，然后进行分页<br />这会导致越往后翻页查询越慢，看了一眼其他数据库，可以在创建索引的时候创建倒序索引<br />这样使用这个索引的时候普通查询即可把最新更新的数据很快列出来<br />不知道mysql什么时候支持这类先置索引而不是在查询时倒序使用索引</p>