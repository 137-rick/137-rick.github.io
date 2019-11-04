---
id: 48
title: PHP memcached持久化长链接的坑
date: 2018-05-28T18:29:15+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/48
permalink: /archives/48
---
最近这个坑确实很坑。

<div>
</div>

<div>
  使用memcached的时候如果在construct的时候传递入持久化id的话，记得addserver只能执行一次。
</div>

<div>
</div>

<div>
  注意：是fpm进程生命周期内只执行一次，否则会导致客户端连接数爆增，直到挂掉。
</div>

<div>
</div>

<div>
  为了防止这个问题需要在construct后执行getServerList确认目前是否已经addServer
</div>

<div>
</div>

<div>
  如果getServerList已经存在数据了，就不需要添加
</div>

<div>
</div>

<div>
  另外，如果config配置更新了，上面的代码因为判断了getServerList那么就不会更新！
</div>

<div>
</div>

<div>
  这个问题是帅哥发现的。。。
</div>

<div>
</div>

<div>
  如果fpm下，需要再判断下config是否有变化，如果有变化需要resetServerList，然后重新addServer
</div>