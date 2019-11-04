---
id: 145
title: 'java  解密碰到的误导人的错误提示 pad block corrupted'
date: 2012-08-24T19:00:45+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/145
permalink: /archives/145
categories:
  - Java
  - 经典技巧
tags:
  - block
  - corrupted
  - it
  - java
  - pad
  - 加密
  - 解密
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  今天碰到一个很窝火的问题<br />使用java进行加密</p> 
  
  <p>
    代码如下：
  </p>
  
  <p>
    Security.addProvider(new com.sun.crypto.provider.SunJCE());<br /> Security.addProvider(neworg.bouncycastle.jce.provider.BouncyCastleProvider());//添加PKCS7Padding支持<br /> Cipher cipher = Cipher.getInstance(&#8220;DESede/ECB/PKCS7Padding&#8221;,&#8221;BC&#8221;);<br /> Key key = CipherManager.getKey(sig.getBytes(&#8220;GBK&#8221;));<br /> cipher.init(Cipher.DECRYPT_MODE, key);<br /> byte[] decBytes =cipher.doFinal(CipherManager.str2ByteArr(encStr));
  </p>
  
  <p>
    结果怎么都是报如下错<br />javax.crypto.BadPaddingException: pad block corrupted<br /> atorg.bouncycastle.jcajce.provider.symmetric.util.BaseBlockCipher.engineDoFinal(UnknownSource)<br /> atjavax.crypto.Cipher.doFinal(Cipher.java:2086)<br /> 略
  </p>
  
  <p>
    各大网站搜索折腾好久，依旧无果<br />最后发现……原来是因为公钥写错了，导致解密失败，翻白中……<br />但是这个错误有很大的误导……发在这里，期望各位在苦海中的亲能解脱早日超生……
  </p>
  
  <p>
  </p>