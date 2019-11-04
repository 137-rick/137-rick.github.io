---
id: 277
title: 'PHP Notice: Redis::set(): send of 36 bytes failed with errno=32 Broken pipe in'
date: 2018-11-06T20:39:42+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/?p=277
permalink: /archives/277
categories:
  - 未分类
---
发现phpredis 如果已经链接，但是底层链接异常的情况下，所有命令都会执行失败，只返回false，不会抛出异常。



如果想修复需要每次执行都ping一下，这样会降低qps



还在和官方沟通。