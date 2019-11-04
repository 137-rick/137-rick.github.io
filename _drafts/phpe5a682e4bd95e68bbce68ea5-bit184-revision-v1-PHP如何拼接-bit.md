---
id: 224
title: PHP如何拼接 bit
date: 2018-10-10T14:07:31+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/224
permalink: /archives/224
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    最近做了个好玩的用php拼类似微博mid一样的串。。。比如用28bit 存储时间 用18bit存储毫秒。
  </div>
  
  <div>
  </div>
  
  <div>
    在php下如何实现？
  </div>
  
  <div>
  </div>
  
  <div>
    我们需要对数值进行按位拼接。
  </div>
  
  <div>
  </div>
  
  <div>
    php提供了很多内置函数做这个事情：
  </div>
  
  <div>
  </div>
  
  <p>
    base_convert() 强大的进制转换函数
  </p>
  
  <div>
    bindec 将&#8221;0000100&#8243;等类似2进制串转成 10进制
  </div>
  
  <div>
    str_pad 若输出长度不够根据需要进行填充
  </div>
  
  <div>
  </div>
  
  <div>
    使用以上功能就可以做到
  </div>
  
  <div>
    特此记录
  </div>
  
  <div>
    </p> 
    
    <div>
      这个方式还是有坑的。。。字节序问题。。。老实用pack去了
    </div>
  </div>