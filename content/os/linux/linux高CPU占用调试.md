---
title: "linux高CPU占用调试"
date: 2022-10-28T16:52:09+08:00
description: "linux高CPU占用调试"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["linux"]
series: ["linux"]
categories: ["linux"]
image:
---
#### 1、用top命令查看哪个进程占用CPU高
gateway网关进程14094占用CPU高达891%，这个数值是进程内各个线程占用CPU的累加值。

PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
14094 root      15   0  315m  10m 7308 S 891%  2.2   1:49.01 gateway
20642 root      17   0 17784 4148 2220 S  0.5  0.8   2:39.96 microdasys
1679 root      18   0 10984 1856 1556 R  0.3  0.4   0:22.21 sshd
22563 root      18   0  2424 1060  800 R  0.3  0.2   0:00.03 top
1 root      18   0  2156  492  460 S  0.0  0.1   0:01.59 init
#### 2、用top -H -p pid命令查看进程内各个线程占用的CPU百分比
`top -H -p 14094`
top中可以看到有107个线程，但是下面9个线程占用CPU很高，下面以线程14086为主，分析其为何high CPU

PID USER      PR  NI  VIRT  RES  SHR S %CPU MEM    TIME+  COMMAND
14086 root      25   0  922m 914m 538m R  101 10.0  21:35.46 gateway
14087 root      25   0  922m 914m 538m R  101 10.0  10:50.22 gateway
14081 root      25   0  922m 914m 538m S   99 10.0   8:57.36 gateway
14082 root      25   0  922m 914m 538m R   99 10.0  11:51.92 gateway
14089 root      25   0  922m 914m 538m R   99 10.0  21:21.77 gateway
14092 root      25   0  922m 914m 538m R   99 10.0  19:55.47 gateway
14094 root      25   0  922m 914m 538m R   99 10.0  21:02.21 gateway
14083 root      25   0  922m 914m 538m R   97 10.0  21:32.39 gateway
14088  root       25   0   922m 914m  538m R    97 10.0   11:23.12  gateway
#### 3、使用gstack命令查看进程中各线程的函数调用栈
`gstack 14094 > gstack.log`
在gstack.log中查找线程ID14086，由于函数栈会暴露函数细节，因此只显示了两个函数桢，线程ID14086对应线程号是37

Thread 37 (Thread 0x4696ab90 (LWP 14086)):
#0  0x40000410 in __kernel_vsyscall ()
#1  0x40241f33 in poll () from /lib/i686/nosegneg/libc.so.6
#### 4、使用gcore命令转存进程映像及内存上下文
`gcore 14094`
该命令生成core文件core.14094
#### 5、用strace命令查看系统调用和花费的时间
`strace -T -r -c -p 14094`
-c参数显示统计信息，去掉此参数可以查看每个系统调用话费的时间及返回值

% time     seconds  usecs/call     calls    errors        syscall
99.99   22.683879        3385      6702                     poll
0.00    0.001132           0      6702                     gettimeofday
0.00    0.000127           1       208       208          accept
0.00    0.000022          22         1                    read
0.00    0.000000           0         1                    write
0.00    0.000000           0         1                    close
0.00    0.000000           0        14                    time
0.00    0.000000           0         2                    stat64
0.00    0.000000           0         4                    clock_gettime
0.00    0.000000           0         7                    send
0.00    0.000000           0        10        10          recvfrom
100.00   22.685160                 13652       218 total
#### 6、用gdb调试core文件，并线程切换到37号线程
gcore和实际的core dump时产生的core文件几乎一样，只是不能用gdb进行某些动态调试
```
(gdb) gdb gateway core.14094
(gdb) thread 37
[Switching to thread 37 (Thread 0x4696ab90 (LWP 14086))]#0  0x40000410 in __kernel_vsyscall ()
(gdb) where
#0  0x40000410 in __kernel_vsyscall ()
#1  0x40241f33 in poll () from /lib/i686/nosegneg/libc.so.6
```
可以根据详细的函数栈进行gdb调试，打印一些变量值，并结合源代码分析为何会poll调用占用很高的CPU
流程为：进程ID-&gt;线程ID-&gt;线程函数调用栈-&gt;函数耗时和调用统计-&gt;源代码分