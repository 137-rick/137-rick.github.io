---
id: 226
title: php mysql_connect自动共用链接的坑
date: 2018-10-10T14:08:37+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/226
permalink: /archives/226
---
<div id="sina_keyword_ad_area2" class="articalContent   newfont_family">
  <div STYLE="text-indent: 2em;">
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);">今天在底层开了mysql多服务器链接功能，但是发现偶尔会出现db串台的情况</span></span>
  </div>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);"><br /></span></span>
  </div>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);">比如a库内的cccs表，底层会请求到b库找cccs表，这个问题很奇怪</span></span>
  </div>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="text-indent: 2em; color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);"><br /></span></span>
  </div>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="text-indent: 2em; color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);">后来发现原来虽然底层实现了多实例，一个db链接对象一个链接，但是在mysql_connect这层也会自动做链接重复直接复用即</span></span><span STYLE="text-indent: 2em; font-size: 16px; letter-spacing: -1px; word-spacing: -2px; color: rgb(102, 153, 51);">bool</span><span STYLE="text-indent: 2em; color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"> </span><code STYLE="text-indent: 2em; font-size: 1rem; letter-spacing: -0.0625rem; line-height: 1.5rem; word-spacing: -0.125rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; margin: 0px;">$new_link</code><span STYLE="text-indent: 2em; font-size: 16px; letter-spacing: -1px; word-spacing: -2px; color: rgb(153, 51, 102);"> =false这个选项，打开后一切正常，略坑，特此记录</span>
  </div>
  
  <div>
  </div>
  
  <div>
    底层链接自动复用的规则为，ip，用户名密码同样的时候自动复用
  </div>
  
  <div>
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px;"><span STYLE="text-rendering: optimizelegibility; color: rgb(51, 102, 153);"><br /></span></span></span>
  </div>
  
  <p>
    mysql_connect<span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">([</span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(102, 153, 51);">string</span> <code STYLE="font-size: 1rem; line-height: 1.5rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; letter-spacing: -0.0625rem; word-spacing: -0.125rem; margin: 0px;">$server</code><span STYLE="color: rgb(153, 51, 102);"> =ini_get(&#8220;mysql.default_host&#8221;)</span></span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">[,</span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(102, 153, 51);">string</span> <code STYLE="font-size: 1rem; line-height: 1.5rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; letter-spacing: -0.0625rem; word-spacing: -0.125rem; margin: 0px;">$username</code><span STYLE="color: rgb(153, 51, 102);"> =ini_get(&#8220;mysql.default_user&#8221;)</span></span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">[,</span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(102, 153, 51);">string</span> <code STYLE="font-size: 1rem; line-height: 1.5rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; letter-spacing: -0.0625rem; word-spacing: -0.125rem; margin: 0px;">$password</code><span STYLE="color: rgb(153, 51, 102);"> =ini_get(&#8220;mysql.default_password&#8221;)</span></span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">[,</span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(102, 153, 51);">bool</span> <code STYLE="font-size: 1rem; line-height: 1.5rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; letter-spacing: -0.0625rem; word-spacing: -0.125rem; margin: 0px;">$new_link</code><span STYLE="color: rgb(153, 51, 102);"> =false</span></span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">[,</span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; text-indent: 2em;"><span STYLE="color: rgb(102, 153, 51);">int</span> <code STYLE="font-size: 1rem; line-height: 1.5rem; font-family: 'Fira Mono', 'source Code pro', monospace; word-wrap: break-word; color: rgb(51, 102, 153); cursor: pointer; letter-spacing: -0.0625rem; word-spacing: -0.125rem; margin: 0px;">$client_flags</code><span STYLE="color: rgb(153, 51, 102);"> =0</span></span><span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255); text-indent: 2em;">]]]]])</span>
  </p>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacin
g: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255);"><br /></span>
  </div>
  
  <div STYLE="text-indent: 2em;">
    <span STYLE="color: rgb(115, 115, 115); font-family: 'Fira Mono', 'source Code pro', monospace; font-size: 16.363636016845703px; letter-spacing: -1px; line-height: 24px; word-spacing: -2px; background-color: rgb(255, 255, 255);"><br /></span>
  </div>