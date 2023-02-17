# weblogic

启动managed server时报错:outofmemory error  perm size

控制台修改启动参数

-Xms1024m -Xmx1024m -XX:MaxPermSize=512m -Djava.awt.headless=true



