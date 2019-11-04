---
id: 220
title: c++ 11 获取当前天0点的timestamp
date: 2018-10-10T14:06:47+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/220
permalink: /archives/220
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  弄了半天才搞定。。光靠baidu是不靠谱的</p> 
  
  <div>
  </div>
  
  <div>
    <pre STYLE="background-color:#2b2b2b;color:#a9b7c6;font-family:'Menlo';font-size:12.0pt;"><span STYLE="color:#cc7832;font-weight:bold;">struct </span>tm *p<span STYLE="color:#cc7832;">;<br /></span>timestamp += <span STYLE="color:#6897bb;">7 </span>* <span STYLE="color:#6897bb;">60 </span>* <span STYLE="color:#6897bb;">60</span><span STYLE="color:#cc7832;">;<br /></span><span STYLE="color:#cc7832;"><br /></span>p = localtime(&timestamp)<span STYLE="color:#cc7832;">;<br /></span><span STYLE="color:#808080;">//LOG_INFO &lt;&lt; "date:" &lt;&lt; p-&gt;tm_mday &lt;&lt; " " &lt;&lt; p-&gt;tm_hour &lt;&lt; ":" &lt;&lt; p-&gt;tm_min &lt;&lt; ":" &lt;&lt; p-&gt;tm_sec;<br /></span>p-&gt;tm_hour = <span STYLE="color:#6897bb;"></span><span STYLE="color:#cc7832;">;<br /></span>p-&gt;tm_min = <span STYLE="color:#6897bb;"></span><span STYLE="color:#cc7832;">;<br /></span>p-&gt;tm_sec = <span STYLE="color:#6897bb;"></span><span STYLE="color:#cc7832;">;<br /></span><span STYLE="color:#cc7832;"><br /></span>uint32_t num = mktime(p)<span STYLE="color:#cc7832;">;<br /></span>num -= <span STYLE="color:#6897bb;">7 </span>* <span STYLE="color:#6897bb;">60 </span>* <span STYLE="color:#6897bb;">60</span><span STYLE="color:#cc7832;">;</span></pre>
  </div>