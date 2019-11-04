---
id: 196
title: 'c++ 11 utf8 substr &amp;&amp; length 中英混合'
date: 2016-03-02T21:19:07+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/196
permalink: /archives/196
categories:
  - C++
tags:
  - c
  - substr
  - utf8
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div>
    改编自：http://www.zedwood.com/article/cpp-utf-8-mb_substr-function
  </div>
  
  <div>
    这个也有问题，我基础这个改了个完善的另外写了个文字个数统计</p> 
    
    <div>
    </div>
  </div>
  
  <div>
  </div>
  
  <div>
    <div>
      uint64_t pinyin::utf8_len(const std::string &str) {
    </div>
    
    <div>
      uint64_t i = 0;
    </div>
    
    <div>
      uint64_t count =0;
    </div>
    
    <div>
      uint64_t c;
    </div>
    
    <div>
    </div>
    
    <div>
      for (i = 0; i <str.length(); i++) {
    </div>
    
    <div>
      count++;
    </div>
    
    <div>
    </div>
    
    <div>
      c = (unsigned char) str[i];
    </div>
    
    <div>
      if (c >= 0 && c <= 127) i +=0;
    </div>
    
    <div>
      else if ((c & 0xE0) == 0xC0) i += 1;
    </div>
    
    <div>
      else if ((c & 0xF0) == 0xE0) i += 2;
    </div>
    
    <div>
      else if ((c & 0xF8) == 0xF0) i += 3;
    </div>
    
    <div>
      //else if(($c & 0xFC) == 0xF8) i+=4; // 111110bb //byte 5, unnecessaryin 4 byte UTF-8
    </div>
    
    <div>
      //else if(($c & 0xFE) == 0xFC) i+=5; // 1111110b //byte 6, unnecessaryin 4 byte UTF-8
    </div>
    
    <div>
      else return 0;//invalid utf8
    </div>
    
    <div>
      }
    </div>
    
    <div>
      return count;
    </div>
    
    <div>
      }
    </div>
    
    <div>
    </div>
    
    <div>
      std::string pinyin::utf8_substr(const std::string &str,uint64_t start, uint64_t leng) {
    </div>
    
    <div>
      if (leng == 0) { return&#8221;&#8221;; }
    </div>
    
    <div>
      uint64_t c, i, ix, q,min = std::string::npos, max = std::string::npos;
    </div>
    
    <div>
      for (q = 0, i = 0, ix =str.length(); i < ix; i++, q++) {
    </div>
    
    <div>
      if (q == start) { min = i; }
    </div>
    
    <div>
      if (q <= start + leng || leng ==std::string::npos) { max = i; }
    </div>
    
    <div>
    </div>
    
    <div>
      c = (unsigned char) str[i];
    </div>
    
    <div>
      if (c >= 0 && c <= 127) i +=0;
    </div>
    
    <div>
      else if ((c & 0xE0) == 0xC0) i += 1;
    </div>
    
    <div>
      else if ((c & 0xF0) == 0xE0) i += 2;
    </div>
    
    <div>
      else if ((c & 0xF8) == 0xF0) i += 3;
    </div>
    
    <div>
      //else if(($c & 0xFC) == 0xF8) i+=4; // 111110bb //byte 5, unnecessaryin 4 byte UTF-8
    </div>
    
    <div>
      //else if(($c & 0xFE) == 0xFC) i+=5; // 1111110b //byte 6, unnecessaryin 4 byte UTF-8
    </div>
    
    <div>
      else return &#8220;&#8221;;//invalid utf8
    </div>
    
    <div>
      }
    </div>
    
    <div>
      if (q <= start + leng|| leng == std::string::npos) { max = i; }
    </div>
    
    <div>
      if (min ==std::string::npos || max == std::string::npos) { return &#8220;&#8221;; }
    </div>
    
    <div>
      return str.substr(min,max &#8211; min);
    </div>
    
    <div>
      }
    </div>
  </div>