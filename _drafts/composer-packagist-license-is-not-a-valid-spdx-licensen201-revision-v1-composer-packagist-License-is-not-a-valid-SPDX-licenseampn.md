---
id: 209
title: 'composer packagist License is not a valid SPDX license&amp;n'
date: 2018-10-10T14:03:25+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/209
permalink: /archives/209
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <span STYLE="font-family:">License &#8220;Apache&#8221; is not a valid SPDX license identifier, see </span><a HREF="https://spdx.org/licenses/" TARGET="_blank">https://spdx<wbr />.org/license<wbr />s/</a><span STYLE="font-family:"> if you use an open license.</span><br STYLE="font-family:" LUCIDA="" /><span STYLE="font-family:">If the software is closed-source, you may use &#8220;proprietary&#8221; as license.</span></p> 
  
  <div>
    <span STYLE="font-family:"><br /></span>
  </div>
  
  <div>
    <span STYLE="font-family:">究其原因，是开源项目的composer.json的license格式不对了</span>
  </div>
  
  <div>
    <span STYLE="font-family:">刚才摸不到头脑跑去问了问大牛</span>
  </div>
  
  <div>
    跑去<a HREF="https://spdx.org/licenses/" TARGET="_blank">https://spdx<wbr />.org/license</a>
  </div>
  
  <div>
    把apache字段换成apache-2.0即可
  </div>