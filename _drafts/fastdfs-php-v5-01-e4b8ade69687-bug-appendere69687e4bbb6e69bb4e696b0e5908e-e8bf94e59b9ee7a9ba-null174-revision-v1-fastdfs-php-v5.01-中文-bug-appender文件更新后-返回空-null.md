---
id: 230
title: fastdfs php v5.01 中文 bug appender文件更新后 返回空 null
date: 2018-10-10T14:08:38+08:00
author: 徐艺洲
layout: revision
guid: http://www.fireidea.com/archives/230
permalink: /archives/230
---
<div id="sina_keyword_ad_area2" class="articalContent   ">
  首先使用php client</p> 
  
  <p>
    $storage = fastdfs_tracker_query_storage_store($group_name);<br />添加内容有utf8中文<br />fastdfs_storage_upload_appender_by_filebuff(“测试提交 @xcl3721@sd^&*$#@^&(*%!(#dd@s我了个dkjkfd @_fds4444jlk”, $file_ext,$file_meta, $group_name, self:<img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://bbs.chinaunix.net/static/image/smiley/default/shy.gif" ALT="" BORDER="0" TITLE="fastdfs php v5.01 中文 bug appender文件更新后 返回空 null" />tracker, $storage);
  </p>
  
  <p>
    然后更新内容为随意一个英文字符串
  </p>
  
  <p>
    fastdfs_storage_modify_by_filebuff($content, 0, $group_name,$remote_filename, self:<img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://bbs.chinaunix.net/static/image/smiley/default/shy.gif" ALT="" BORDER="0" TITLE="fastdfs php v5.01 中文 bug appender文件更新后 返回空 null" />tracker, $storage);
  </p>
  
  <p>
    然后读取<br />fastdfs_tracker_query_storage_fetch<br />fastdfs_storage_download_file_to_buff($group_name,$remote_filename, $file_offset, $file_range, self:<img src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" real_src ="http://bbs.chinaunix.net/static/image/smiley/default/shy.gif" ALT="" BORDER="0" TITLE="fastdfs php v5.01 中文 bug appender文件更新后 返回空 null" />tracker, $storage);
  </p>
  
  <p>
    结果错误的返回值为null，文件内容丢失<br />查看info以及existe都为正常
  </p>
  
  <p>
    后来解决办法，每次更新的时候使用truncate函数清空，然后再更新后会正常
  </p>