---
id: 228
title: php preg_match_all 段错误
date: 2018-10-10T14:08:38+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/228
permalink: /archives/228
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  昨天碰到个问题php preg_match_all 执行的时候在某条数据上报错<br />怀疑是文字太长导致所以加了pcre的可使用空间，但是故障依旧<br />最后在正则结尾加上U修饰符，取消了greed模式 工作正常了……</p>