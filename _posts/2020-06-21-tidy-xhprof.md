---
id: 330
title: 并不是所有文件Append都是原子性的
date: 2020-06-21T14:45:49+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/?p=331
permalink: /archives/330
categories:
  - PHP-Source
---

## xhprof原理分析
今日一早看到有人star tidyway的 xhprof扩展，好奇性能分析在PHP中是怎么做的，就简单分析了下 

发现实现原理很简单直接，简单记录下 

项目地址： https://github.com/tideways/php-xhprof-extension 

从源码上看 xhprof 主要点有几点

## 入口
初始化时在PHP_MINIT_FUNCTION 做了一个比较有意思的操作、替换掉了PHP关键入口函数指针 
地址： https://github.com/tideways/php-xhprof-extension/blob/f6aed09e98caf1eab58d2fd12dd42fe23e33c7dc/tideways_xhprof.c#L67

```c
   PHP_MINIT_FUNCTION(tideways_xhprof)
   {
       REGISTER_INI_ENTRIES();
   
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_MEMORY", TIDEWAYS_XHPROF_FLAGS_MEMORY, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_MEMORY_MU", TIDEWAYS_XHPROF_FLAGS_MEMORY_MU, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_MEMORY_PMU", TIDEWAYS_XHPROF_FLAGS_MEMORY_PMU, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_CPU", TIDEWAYS_XHPROF_FLAGS_CPU, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_NO_BUILTINS", TIDEWAYS_XHPROF_FLAGS_NO_BUILTINS, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_MEMORY_ALLOC", TIDEWAYS_XHPROF_FLAGS_MEMORY_ALLOC, CONST_CS | CONST_PERSISTENT);
       REGISTER_LONG_CONSTANT("TIDEWAYS_XHPROF_FLAGS_MEMORY_ALLOC_AS_MU", TIDEWAYS_XHPROF_FLAGS_MEMORY_ALLOC_AS_MU, CONST_CS | CONST_PERSISTENT);
   
       _zend_execute_internal = zend_execute_internal; 
       zend_execute_internal = tideways_xhprof_execute_internal; //here 这里替换函数指针
   
       _zend_execute_ex = zend_execute_ex;
       zend_execute_ex = tideways_xhprof_execute_ex; //here 这里替换 脚本入口指针
   
       return SUCCESS;
   }
```

## 原理
替换的两个指针用途
 * zend_execute_ex 每一个php函数执行都会调用关键入口 
 * zend_execute_internal 每一个c函数执行的调用关键入口

可以理解成我们每一个函数和c函数调用之前都会经过这个proxy（类似AOP方式） 
然后在每个入口和结束时记录一下frame 即可。。。
 
### 替换后的 zend_execute_internal 
地址：https://github.com/tideways/php-xhprof-extension/blob/f6aed09e98caf1eab58d2fd12dd42fe23e33c7dc/tideways_xhprof.c#L166 

```c
ZEND_DLEXPORT void tideways_xhprof_execute_internal(zend_execute_data *execute_data, zval *return_value) {
    int is_profiling = 1;

    if (!TXRG(enabled) || (TXRG(flags) & TIDEWAYS_XHPROF_FLAGS_NO_BUILTINS) > 0) {
        execute_internal(execute_data, return_value TSRMLS_CC);
        return;
    }

    is_profiling = tracing_enter_frame_callgraph(NULL, execute_data TSRMLS_CC);

    if (!_zend_execute_internal) {
        execute_internal(execute_data, return_value TSRMLS_CC);
    } else {
        _zend_execute_internal(execute_data, return_value TSRMLS_CC);
    }

    if (is_profiling == 1 && TXRG(callgraph_frames)) {
        tracing_exit_frame_callgraph(TSRMLS_C);
    }
}
```
### 替换后的 zend_execute_ex  
地址： https://github.com/tideways/php-xhprof-extension/blob/f6aed09e98caf1eab58d2fd12dd42fe23e33c7dc/tideways_xhprof.c#L187 

```c
ZEND_DLEXPORT void tideways_xhprof_execute_ex (zend_execute_data *execute_data) {
    zend_execute_data *real_execute_data = execute_data;
    int is_profiling = 0;

    if (!TXRG(enabled)) {
        _zend_execute_ex(execute_data TSRMLS_CC);
        return;
    }

    is_profiling = tracing_enter_frame_callgraph(NULL, real_execute_data TSRMLS_CC);

    _zend_execute_ex(execute_data TSRMLS_CC);

    if (is_profiling == 1 && TXRG(callgraph_frames)) {
        tracing_exit_frame_callgraph(TSRMLS_C);
    }
}
```