---
id: 330
title: 并不是所有文件Append都是原子性的
date: 2019-11-22T14:45:49+08:00
author: 徐艺洲
layout: post
guid: http://www.fireidea.com/?p=330
permalink: /archives/330
categories:
  - Linux
---
线上业务是否稳定？我们都是通过日志来观测的 

如果日志有问题，那么我们监控也会跟着出现问题 

一直以来，我们写日志都是通过append方式去追加到文件结尾。  

而事实上文件的Append操作并不是原子性的，多进程或多线程同时append一个文件会导致文件内容混乱 

操作是否原子取决于操作系统内buffer的长度 

linux下可以通过  grep “BUF” /usr/include/linux/limits.h 查看 

常见linux系统这个值在4096，如果日志长度超过这个长度，多线程或多进程都会出现日志混乱情况 

## 测试一下
我们可以使用如下脚本做测试，测试下是否存在多线程append内容到同一个文件是否为原子性 

声明：此脚本来自 https://www.notthewizard.com/2014/06/17/are-files-appends-really-atomic/ 放在这里做演示是因为简单好展示

```bash
#############################################################################
#
# This script aims to test/prove that you can append to a single file from
# multiple processes with buffers up to a certain size, without causing one
# process' output to corrupt the other's.
#
# The script takes one parameter, the length of the buffer. It then creates
# 20 worker processes which each write 50 lines of the specified buffer
# size to the same file. When all processes are done outputting, it tests
# the output file to ensure it is in the correct format.
#
#############################################################################
 
NUM_WORKERS=20
LINES_PER_WORKER=50
OUTPUT_FILE=/tmp/out.tmp
 
# each worker will output $LINES_PER_WORKER lines to the output file
run_worker() {
    worker_num=$1
    buf_len=$2
 
    # Each line will be a specific character, multiplied by the line length.
    # The character changes based on the worker number.
    filler_len=$((${buf_len}-1)) # -1 -> leave room for \n
    filler_char=$(printf \\$(printf '%03o' $(($worker_num+64))))
    line=`for i in $(seq 1 $filler_len);do echo -n $filler_char;done`
    for i in $(seq 1 $LINES_PER_WORKER)
    do
        echo $line >> $OUTPUT_FILE
    done
}
 
if [ "$1" = "worker" ]; then
    run_worker $2 $3
    exit
fi
 
buf_len=$1
if [ "$buf_len" = "" ]; then
    echo "Buffer length not specified, defaulting to 4096"
    buf_len=4096
fi
 
rm -f $OUTPUT_FILE
 
echo Launching $NUM_WORKERS worker processes
for i in $(seq 1 $NUM_WORKERS)
do
    $0 worker $i $buf_len &
    pids[$i]=${!}
done
 
echo Each line will be $buf_len characters long
echo Waiting for processes to exit
for i in $(seq 1 $NUM_WORKERS)
do
    wait ${pids[$i]}
done
 
# Now we want to test the output file. Each line should be the same letter
# repeated buf_len-1 times (remember the \n takes up one byte). If we had
# workers writing over eachother's lines, then there will be mixed characters
# and/or longer/shorter lines.
 
echo Testing output file
 
# Make sure the file is the right size (ensures processes didn't write over
# eachother's lines)
expected_file_size=$(($NUM_WORKERS * $LINES_PER_WORKER * $buf_len))
actual_file_size=`cat $OUTPUT_FILE | wc -c`
if [ "$expected_file_size" -ne "$actual_file_size" ]; then
    echo Expected file size of $expected_file_size, but got $actual_file_size
else
  
    # File size is OK, test the actual content
 
    # Only use newer versions of grep because older ones are way too slow with
    # backreferences
    [[ $(grep --version) =~ [^[:digit:]]*([[:digit:]]+)\.([[:digit:]]+) ]]
    grep_ver="${BASH_REMATCH[1]}${BASH_REMATCH[2]}"
    if [ "$grep_ver" -ge "216" ]; then
        num_lines=$(grep -v "^\(.\)\1\{$((${buf_len}-2))\}$" $OUTPUT_FILE | wc -l)
    else
        # Scan line by line in bash, which isn't that speedy, but is good enough
        # Note: Doesn't work on cygwin for lines < 255
        line_length=$((${buf_len}-1))
        num_lines=0
        for line in `cat $OUTPUT_FILE`
        do
            if ! [[ $line =~ ^${line:0:1}{$line_length}$ ]]; then
                num_lines=$(($num_lines+1))
            fi;
            echo -n .
        done
        echo
    fi
 
    if [ "$num_lines" -gt "0" ]; then
        echo "Found $num_lines instances of corrupted lines"
        else
        echo "All's good! The output file had no corrupted lines. $size"
    fi
fi
 
rm -f $OUTPUT_FILE
```

## 测试效果如下展示：
```bash
# ./test_appends.sh 4096 ## 这里用4096 长度做测试
Launching 20 worker processes
Each line will be 4096 characters long
Waiting for processes to exit
Testing output file
.......................[snip]....
All's good! The output file had no corrupted lines. ## 测试结果没有出现异常

# ./test_appends.sh 4097 ## 这里使用4097 做测试
Launching 20 worker processes
Each line will be 4097 characters long
Waiting for processes to exit
Testing output file
.......................[snip]....
Found 27 instances of corrupted lines ## 27行日志出现问题
```

有兴趣的朋友可以用这个脚本测试一下，win、linux下都会复现，只不过这个长度不同罢了 


## 如何避免此类问题？
 * 一个线程只写一个文件，如文件名中增加线程+进程id
 * 内存队列汇总后统一一个线程落地
 * 网络推送到kafka等消息中间件中
 * 加锁、会导致QPS下降，性能大幅度降低
 
## 原理

 请参考 [write man 介绍](http://man7.org/linux/man-pages/man2/write.2.html) 其中写buff 4096 
 
 核心介绍就是如果日志内容长度超过buff长度，那么写操作需要多次进行，多次进行时没有锁住、就会被其他线程抢用，导致内容乱序. 
 
 目前很多开源日志组件都做了锁操作