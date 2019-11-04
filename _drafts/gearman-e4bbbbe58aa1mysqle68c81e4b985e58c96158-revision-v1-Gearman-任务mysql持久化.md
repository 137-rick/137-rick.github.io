---
id: 243
title: Gearman 任务mysql持久化
date: 2018-10-10T14:13:26+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/243
permalink: /archives/243
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  这里只是简单记录下</p> 
  
  <p>
    gearman异步任务下发过多的时候可以对没处理完成的异步任务持久化保存，持久化的方式很多支持mysql psqlmemcache等
  </p>
  
  <p>
    在gearman编译过程中的./configure步骤时，会自动检测系统内已有安装环境，并在configure完成后提示支持哪些持久化扩展，这里我只用了mysql方式
  </p>
  
  <p>
    操作步骤如下
  </p>
  
  <p>
    在mysql创建数据库及对应数据表
  </p>
  
  <p>
    create database gearman<br />create table `gearman_queue` (<br />`unique_key` varchar(64) NOT NULL,<br />`function_name` varchar(255) NOT NULL,<br />`priority` int(11) NOT NULL,<br />`data` LONGBLOB NOT NULL,<br />`when_to_run` INT, PRIMARY KEY (`unique_key`)<br />)
  </p>
  
  <p>
    //在启动gearmand的时候指定扩展类型 扩展对应的端口 用户名 密码 以及数据库<br />gearmand &#8211;queue-type mysql &#8211;mysql-host 127.0.0.1 &#8211;mysql-dbgearman &#8211;mysql-user &#8220;root&#8221; &#8211;mysql-password &#8220;&#8221; -w 2 -d
  </p>
  
  <p>
  </p>