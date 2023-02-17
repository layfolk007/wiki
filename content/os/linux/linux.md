# centos

#### centos7修改网卡名及禁用ipv6

1. 在/etc/default/grub里GRUB\_CMDLINE\_LINUX中添加参数ipv6.disable=1 net.ifnames=0 biosdevname=0并用grub2-mkconfig -o /boot/grub2/grub.cfg重启生成启动配置文件
2. 将/etc/sysconfig/network-scripts下的网卡配置文件改名为想要更改的名字，并将文件里的NAME和DEVICE也改为对应的名字
3. 重启系统

#### systemd

```
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=root
PIDFile=/root/.vnc/%H:%i.pid
PAMName=login
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

# linux高CPU占用调试

#### 1、用top命令查看哪个进程占用CPU高

gateway网关进程14094占用CPU高达891%，这个数值是进程内各个线程占用CPU的累加值。

PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND

14094 root      15   0  315m  10m 7308 S 891%  2.2   1:49.01 gateway

20642 root      17   0 17784 4148 2220 S  0.5  0.8   2:39.96 microdasys

1679 root      18   0 10984 1856 1556 R  0.3  0.4   0:22.21 sshd

22563 root      18   0  2424 1060  800 R  0.3  0.2   0:00.03 top

```
1 root      18   0  2156  492  460 S  0.0  0.1   0:01.59 init
```

#### 2、用top -H -p pid命令查看进程内各个线程占用的CPU百分比

\#top -H -p 14094

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

\#gstack 14094 &gt; gstack.log

在gstack.log中查找线程ID14086，由于函数栈会暴露函数细节，因此只显示了两个函数桢，线程ID14086对应线程号是37

Thread 37 \(Thread 0x4696ab90 \(LWP 14086\)\):

\#0  0x40000410 in \_\_kernel\_vsyscall \(\)

\#1  0x40241f33 in poll \(\) from /lib/i686/nosegneg/libc.so.6

#### 4、使用gcore命令转存进程映像及内存上下文

\#gcore 14094

该命令生成core文件core.14094

#### 5、用strace命令查看系统调用和花费的时间

\#strace -T -r -c -p 14094

-c参数显示统计信息，去掉此参数可以查看每个系统调用话费的时间及返回值。

% time     seconds  usecs/call     calls    errors        syscall

---

99.99   22.683879        3385      6702                     poll

0.00    0.001132           0      6702                     gettimeofday

0.00    0.000127           1       208       208          accept

0.00    0.000022          22         1                    read

0.00    0.000000           0         1                    write

0.00    0.000000           0         1                    close

0.00    0.000000           0        14                    time

0.00    0.000000           0         2                    stat64

0.00    0.000000           0         4                    clock\_gettime

0.00    0.000000           0         7                    send

0.00    0.000000           0        10        10          recvfrom

---

100.00   22.685160                 13652       218 total

#### 6、用gdb调试core文件，并线程切换到37号线程

gcore和实际的core dump时产生的core文件几乎一样，只是不能用gdb进行某些动态调试

\(gdb\) gdb gateway core.14094

\(gdb\) thread 37

\[Switching to thread 37 \(Thread 0x4696ab90 \(LWP 14086\)\)\]\#0  0x40000410 in \_\_kernel\_vsyscall \(\)

\(gdb\) where

\#0  0x40000410 in \_\_kernel\_vsyscall \(\)

\#1  0x40241f33 in poll \(\) from /lib/i686/nosegneg/libc.so.6

可以根据详细的函数栈进行gdb调试，打印一些变量值，并结合源代码分析为何会poll调用占用很高的CPU。

因为代码涉及到公司产权，顾不在此做详细分析，需要明白的是分析的流程和使用的命令。

流程为：进程ID-&gt;线程ID-&gt;线程函数调用栈-&gt;函数耗时和调用统计-&gt;源代码分析

# 让你提升命令行效率的 Bash 快捷键

#### 1、编辑命令

* Ctrl + a ：移到命令行首 
* Ctrl + e ：移到命令行尾 
* Ctrl + f ：按字符前移（右向） 
* Ctrl + b ：按字符后移（左向） 
* Alt + f ：按单词前移（右向） 
* Alt + b ：按单词后移（左向） 
* Ctrl + xx：在命令行首和光标之间移动 
* Ctrl + u ：从光标处删除至命令行首 
* Ctrl + k ：从光标处删除至命令行尾 
* Ctrl + w ：从光标处删除至字首 
* Alt + d ：从光标处删除至字尾 
* Ctrl + d ：删除光标处的字符 
* Ctrl + h ：删除光标前的字符 
* Ctrl + y ：粘贴至光标后 
* Alt + c ：从光标处更改为首字母大写的单词 
* Alt + u ：从光标处更改为全部大写的单词 
* Alt + l ：从光标处更改为全部小写的单词 
* Ctrl + t ：交换光标处和之前的字符 
* Alt + t ：交换光标处和之前的单词 
* Alt + Backspace：与 Ctrl + w 相同类似，分隔符有些差别

#### 2、重新执行命令

* Ctrl + r：逆向搜索命令历史 
* Ctrl + g：从历史搜索模式退出 
* Ctrl + p：历史中的上一条命令 
* Ctrl + n：历史中的下一条命令 
* Alt + .：使用上一条命令的最后一个参数 

#### 3、控制命令

* Ctrl + l：清屏 
* Ctrl + o：执行当前命令，并选择上一条命令 
* Ctrl + s：阻止屏幕输出 
* Ctrl + q：允许屏幕输出 
* Ctrl + c：终止命令 
* Ctrl + z：挂起命令 

#### 4、Bang \(!\) 命令

* !!：执行上一条命令 
* !blah：执行最近的以 blah 开头的命令，如 !ls 
* !blah:p：仅打印输出，而不执行 
* !$：上一条命令的最后一个参数，与 Alt + . 相同 
* !$:p：打印输出 !$ 的内容 
* !\*：上一条命令的所有参数 
* !\*:p：打印输出 !\* 的内容 
* ^blah：删除上一条命令中的 blah 
* ^blah^foo：将上一条命令中的 blah 替换为 foo 
* ^blah^foo^：将上一条命令中所有的 blah 都替换为 foo 

#### 友情提示

1. 以上介绍的大多数 Bash 快捷键仅当在 emacs 编辑模式时有效，若你将 Bash 配置为 vi 编辑模式，那将遵循 vi 的按键绑定。Bash 默认为 emacs 编辑模式。如果你的 Bash 不在 emacs 编辑模式，可通过 set -o emacs 设置。 
2. ^S、^Q、^C、^Z 是由终端设备处理的，可用 stty 命令设置。 

# 占用swap最多的进程
```
for i in $(ls /proc | grep "^[0-9]"); do awk '/Swap:/{a=a+$2}END{print '"$i"',a/1024"M"}' /proc/$i/smaps;done| sort -k2nr | head
```