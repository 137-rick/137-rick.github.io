---
id: 229
title: 使用redis的setnx制作排他锁
date: 2018-10-10T14:08:38+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/229
permalink: /archives/229
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  今日发现一个逻辑select count，如果没有数据那么执行insert<br />正常情况下数据库应该有一条数据，但是实际发现出现两条<br />经测试是因为多进程并发发起请求 selectcount提前执行导致的<br />最终讨论后使用排他锁保证事务同样参数时只执行一个</p> 
  
  <p>
    使用redis的setnx对根据参数拼好的key的set进行赋值<br />如果赋值成功，那么继续执行下面操作<br />如果赋值失败，代表之前有进程正在跑<br />排他事务执行完毕后，删除刚才添加的key
  </p>
  
  <p>
    setnx：当redis内存在此key的时候，插入失败并返回false，当redis内不存在此key的时候插入，并返回true
  </p>
  
  <p>
    备注：请记得加expire……
  </p>