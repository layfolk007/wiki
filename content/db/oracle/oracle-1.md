---
title: "oracle-1"
date: 2023-02-14T15:12:53+08:00
description: "oracle-1"
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocFolding: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags: ["oracle"]
series: ["oracle"]
categories: ["oracle"]
image:
---
实例instance：SGA+后台进程，用来加载和管理数据库用，每个运行的数据库对应一个实例。

```
desc   v$instance
```

数据库：一组文件集合\(数据文件、控制文件、在线重做日志文件\)。

# 数据库创建方法

1、DBCA图形界面法。

2、DBCA命令法。`dbca -silent -createDatabase -templateName General_Purpose.dbc -gdbname sybell -sid sybell -responseFile NO_VALUE -characterSet AL32UTF8 -memoryPercentage 30 -emConfiguration LOCAL`

```
 -silent指定以静默方式执行

 -templateName指定用来创建数据库的模板名称

 -responseFile指定安装响应文件。NO\_VALUE表示没有响应文件。响应文件包含了在交互方式安装过程中对由用户提供的对安装问题的回答。在响应文件中为每个问题都保存为一个变量。例如，在响应文件中设置Oracle根目录和安装类型等参数的值。其保存在安装目录的response目录下。

 -emConfiguration指定EM的管理选项。LOCAL表示数据库由EM本地管理；CENTRAL表示数据库由EM集中管理；NOBACKUP表示不启用数据库的每天自动备份功能；NOEMAIL表示不启用邮件通知功能；NONE不使用EM管理数据库。
```

# 数据库实例的状态

1、shutdown 关闭

2、nomount 已启动，也称不装载，开辟SGA和运行后台进程

－－后台进程格式固定：ora\_4字符\_实例名；可以通过ps -ef \| grep ora\_ 命令查看

3、mount 已装载，读取控制文件controlfile

4、open  打开数据库文件

5、可以从shutdown分别到nomount,mount,open状态，但是从nomount不可以直接到open

6、状态切换：

```
startup---直接到open
startup noumount---到nomount状态，只能通过以下方法到open状态：
alter database mount;
alter database open;
```

7、数据库实例启动过程中会读取初始化参数文件pfile,spfile\(是pfile的二进制形态\)，两者都存在会先读spfile，因为是二进制形式，安全性好。

－－参数文件存在：$ORACLE\_HOME/dbs下，参数文件可以相互转换\(`create pfile from spfile`\)

```
可以指定pfile来启动，例：startup mount pfile=xxxx
```

－－查看参数：`show parameter/select * from v$parameter`

－－修改参数：`alter system set 参数＝值 scope=both|memory|spfile 也可以修改文件`

8、关闭数据库

```
shutdown immediate|normal|transactional|abort
```

－－用immediate是干净的立即关闭

－－用abort关闭后重新启动会自动恢复错误

－－不加任何选项就是normal

通过sqlplus远程连接数据库时，用sqlplu user/password@服务别名tnsname \[as sysdba\]\(sys用户使用\)

远程连接认证文件\(orapw实例名\)，存在$ORACLE\_HOME/dbs下

－如果此文件被删或忘记密码，可以通过以下方法创建

－进入存放目录orapwd file=orapw实例名 password=新密码

# listener

创建方法：

1、netca

2、netmgr\(还可以创建服务名，用于远程连接\)

3、直接修改配置文件listener.ora

# tnsname

创建方法：netmgr  配置文件tnsnames.ora

通过命令tnsping 服务别名 验证是否成功

# EM

emctl start\|stop\|status dbconsole

# 用户

```
数据库用户有六种型：数据库管理员、安全官员、网络管理员、应用程序开发员、应用程序管理员、数据库用户。
```

sys system

1、创建

create user xx identified by pass;

2、权限

grant connect,resource to xx;

3、建表

create table a\(name varchar2\(10\)\);

4、插入数据进表

insert into a values\('asdfdf'\);

commit;提交数据

5、锁定与解锁用户

alter user xx account lock/unlock;

# 表空间视图:v$tablespace

1、创建

create tablespace xx datafile size 20m;

2、增加大小

alter tablespace xx add datafile '路径/xx.dbf' size 20m;

# 数据文件视图：v$datafile

# 控制文件视图：v$controlfile  11g中默认有两个

# 日志文件视图：v$log    v$logfile



