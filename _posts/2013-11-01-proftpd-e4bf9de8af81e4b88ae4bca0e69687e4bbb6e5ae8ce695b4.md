---
id: 168
title: proftpd 保证上传文件完整
date: 2013-11-01T17:30:40+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/archives/168
permalink: /archives/168
categories:
  - Linux
  - 经典技巧
tags:
  - ftp
  - it
  - 文件完整
  - 文件扫描
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  目前接到一个需求：<br />1.用户通过ftp上传特别大的文件到指定目录，定期会有一个进程会扫描此目录下文件。<br />2.如果发现有文件将会把文件挪走……</p> 
  
  <p>
    期间我们担心如果ftp没有传完但是却被扫描进程扫走……文件将会不完整<br />之前没细研究过ftp……<br />跟几个朋友聊的时候表示这个需求很抑郁<br />后来想到不如直接改ftp服务的代码<br />在读代码的时候意外发现ftp是有这个功能的……只是说法不一样……
  </p>
  
  <p>
    具体细节如下：
  </p>
  
  <p>
    proftpd是linux下的ftp服务软件<br />在他的配置内有一个选项HiddenStores 默认是off<br />如果打开，那么新上传的文件将会以.in.文件名保存，直到收取完成，将会把文件改回原名<br />具体介绍如下连接<br />http://www.proftpd.org/docs/directives/linked/config_ref_HiddenStores.html
  </p>
  
  <p>
    扔这里给各位苦逼的同胞，预祝早日脱离苦海……
  </p>