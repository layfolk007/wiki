# hadoop

===&gt;系列部署管理工具ambari，yum repo[下载地址](http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.7.3.0/ambari.repo)

客户端写文件到hdfs时报错：

```
INFO - Exception:org.apache.hadoop.security.AccessControlException: Permission denied: user=root, access=WRITE, inode="":hdfsuser:supergroup:rwxr-xr-x    Triggering method:eda9cube.service.impl.TaskServiceImpl.uploadTaskToS3()
```

原因：客户端上是root用户，服务端启动用的是其它用户

解决办法：

1. 在系统的环境变量或java JVM变量里面添加HADOOP\_USER\_NAME，这个值具体等于多少看自己的情况，为运行HADOOP上的Linux的用户名。
2. 在客户端上建一个和运行hadoop程序一样的用户
3. 使用HDFS的命令行接口修改相应目录的权限，hadoop fs -chmod 777 /
4. 在hdfs-site.xml中添加配置项：

```
<property>

  <name>dfs.permissions</name>

  <value>false</value>

</property>
```

hadoop远程无法上传文件，报错信息如下：

```
2015-08-24 06:22:07,391 WARN org.apache.hadoop.hdfs.server.namenode.FSNamesystem: Not able to place enough replicas, still in need of 1 to reach 1
 Not able to place enough replicas
2015-08-24 06:22:07,392 ERROR org.apache.hadoop.security.UserGroupInformation: PriviledgedActionException as:xsh cause:java.io.IOException: File /tmp/sql_1.sql could only be replicated to 0 nodes, instead of 1
2015-08-24 06:22:07,395 INFO org.apache.hadoop.ipc.Server: IPC Server handler 2 on 9000, call addBlock(/tmp/sql_1.sql, DFSClient_NONMAPREDUCE_1138223082_34, [Lorg.apache.hadoop.hdfs.protocol.DatanodeInfo;@28a50da4) from 202.110.153.198:12764: error: java.io.IOException: File /tmp/sql_1.sql could only be replicated to 0 nodes, instead of 1
java.io.IOException: File /tmp/sql_1.sql could only be replicated to 0 nodes, instead of 1
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getAdditionalBlock\(FSNamesystem.java:1920\)
at org.apache.hadoop.hdfs.server.namenode.NameNode.addBlock\(NameNode.java:783\)
at sun.reflect.GeneratedMethodAccessor7.invoke\(Unknown Source\)
at sun.reflect.DelegatingMethodAccessorImpl.invoke\(DelegatingMethodAccessorImpl.java:25\)
at java.lang.reflect.Method.invoke\(Method.java:597\)
at org.apache.hadoop.ipc.RPC$Server.call\(RPC.java:587\)
at org.apache.hadoop.ipc.Server$Handler$1.run\(Server.java:1432\)
at org.apache.hadoop.ipc.Server$Handler$1.run\(Server.java:1428\)
at java.security.AccessController.doPrivileged\(Native Method\)
at javax.security.auth.Subject.doAs\(Subject.java:396\)
at org.apache.hadoop.security.UserGroupInformation.doAs\(UserGroupInformation.java:1190\)
at org.apache.hadoop.ipc.Server$Handler.run\(Server.java:1426\)
```

可能原因：

1、datanode远程无法访问，发现是以hostname解析的IP返回，如果IP为私有，那么远程无法访问。

