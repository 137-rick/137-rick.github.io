---
id: 329
title: The HOME or COMPOSER_HOME environment variable must be set for composer to run
date: 2019-10-15T17:45:49+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/?p=329
permalink: /archives/329
categories:
  - PHP
---
在saltstack远程将项目composer 做dump优化的时候碰到如下错误提示

17:17:23 [SSH] executing&#8230;  
17:17:26 ERROR: Minions returned with non-zero exit code  
17:17:26 xxxxxxxxxxxxxxxx:  
17:17:26  
17:17:26  
17:17:26 [RuntimeException]  
17:17:26 The HOME or COMPOSER_HOME environment variable must be set for composer to run correctly  
17:17:26  
17:17:26 [SSH] completed  
17:17:26 [SSH] exit-status: 11  
17:17:26  
17:17:26 Build step &#8216;Execute shell script on remote host using ssh&#8217; marked build as failure

原来命令:

cd /home/xxx/xxxx/ && /usr/local/php/bin/php ./Bin/composer dump-autoload -o -a &#8211;no-dev

经过检测发现是HOME和COMPOSER_HOME没有设置，改为

cd /home/xxxx/xxx/ && HOME=&#8221;/home/xxxx&#8221; COMPOSER_HOME=&#8221;/home/xxxx/.config/composer&#8221; /usr/local/php/bin/php ./Bin/composer dump-autoload -o -a &#8211;no-dev

即可解决，composer需要这个目录写cache而很多环境变量在saltstack这里没有设置，导致此问题