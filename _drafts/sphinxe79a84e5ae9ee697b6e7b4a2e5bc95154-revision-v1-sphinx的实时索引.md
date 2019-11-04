---
id: 247
title: sphinx的实时索引
date: 2018-10-10T14:13:27+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/247
permalink: /archives/247
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  sphinx目前出来一个realtime index即实时索引<br />据官方透露，目前已经开始可以在生产环境使用<br />他可以使用sphinxQL用mysql协议进行查询添加更新数据<br />看起来像一个mysql一样，不过他支持全文检索，新更新进去的数据会自动索引达到实时索引的程度<br />但是他也有缺点，比如经常更新会导致内存增长<br />在内存中的数据如果不及时写入硬盘，出现中断会丢失数据<br />只支持部分sql语句，前段时间简单测试了一下<br />如果数据量巨大他会变得缓慢，在内存数据往硬盘写的时候会卡几秒<br />下面是sphinx.conf内的一段关于实时索引的语句<br />index rt<br />{ <br /> # 实时索引类型<br /> type = rt</p> 
  
  <p>
    # 索引保存路径，平时都是保存在内存内，数据量超过内存量的时候会保存在文件内，这里随便存了下没放到data目录下<br /> path = /usr/local/sphinx/var/data/rttest
  </p>
  
  <p>
    # 内存保存大小限制，超过这个就会保存到硬盘中<br /> # optional, default is 32M，默认32m<br /> #<br /> rt_mem_limit = 32M
  </p>
  
  <p>
    # 全文检索字段声明，这里把实时索引的索引字段都声明出来<br /> rt_field = title #全文索引字段<br /> #rt_field = content<br /> rt_attr_uint = gid #其他属性字段，可以用来查询<br /> rt_attr_bigint = guid<br /> #rt_attr_float = gpa<br /> #rt_attr_timestamp = ts_added<br /> #rt_attr_string = author<br />}<br />searchd<br />{<br />#这里配置很多只说关键的地方<br /> listen = 9306:mysql41 #searchd支持mysql协议连接的端口<br />max_matches = 3000 #在mysql协议内查询出来的数据只会返回3000条，即使使用limit语句也是如此<br />}
  </p>
  
  <p>
    启动searchd后<br />shell下命令<br />mysql -P 9306 -h 127.0.0.1<br />连接sphinx
  </p>
  
  <p>
    添加数据<br />insert into rt values(3,&#8217;test&#8217;,1,2);<br />注意，第一个字段必须指定值，因为id是sphinx内指定的唯一id
  </p>
  
  <p>
    我用脚本添加了一批随机数据
  </p>
  
  <p>
    下面可以用sql查询<br />select * from rt order by id desc limit 1;
  </p>
  
  <p>
    +&#8212;&#8212;&#8212;+&#8212;&#8212;&#8211;+&#8212;&#8212;+&#8212;&#8212;-+<br />|id | weight | gid | guid |<br />+&#8212;&#8212;&#8212;+&#8212;&#8212;&#8211;+&#8212;&#8212;+&#8212;&#8212;-+<br />| 2613327| 1 | 179 | 45759 |<br />+&#8212;&#8212;&#8212;+&#8212;&#8212;&#8211;+&#8212;&#8212;+&#8212;&#8212;-+<br />1 row in set (0.57 sec)<br />如果不执行全文检索，那么性能和没有建立索引的mysql效果是一样<br />其中weight是结果匹配权重
  </p>
  
  <p>
    使用matchs在where内可以做全文检索<br />大家可以参考官方文档，注意哦目前只有2.0.6才开始声明realtime index可以用于生产环境了<br />2.0.6官方文档网址：<br />http://sphinxsearch.com/docs/2.0.6/
  </p>