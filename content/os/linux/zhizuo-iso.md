制作环境：CentOS-6.7-minimal\_x86\_64  
首先挂载系统盘：

```
 mount -t iso9660 /dev/cdrom /mnt/cdrom﻿​ 
```

复制系统盘数据到/data/ISO/目录：

```
 rsync -a –exclude=Packages/ /mnt/cdrom/ /data/ISO/﻿​ 
```

配置ks.cfg问题：

```
cp /root/anaconda-ks.cfg /data/ISO/isolinux/﻿​ 
```

修改ks.cfg文件可以设置网络，防火墙，selinux等；直接删掉root密码那一行，安装时会提示输入密码。  
复制光盘中的Packages目录下的rpm包到/data/ISO/Packages下面。（只复制现在系统安装的rpm包，baidu上找复制脚本）  
接下来制作代码仓库:

```
cp /mnt/cdrom/repodata/*-minimal-x86_64.xml /data/minimal-x86_64.xml
cd /data/ISO
declare -x discinfo=$(head -1 .discinfo)
createrepo -u "media://$discinfo" -g /data/minimal-x86_64.xml .﻿​ 
```

执行打包ISO命令

```
mkisofs -o CentOS6.7-x86_64.iso -V CentOS6.7-x86_64 -input-charset utf-8 -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -R -J -v -T -joliet-long /data/ISO/﻿​ 
```

如果想向系统中加入数据，都必须打包成rpm包并复制到/data/ISO/Packages下，然后把包名添加到/data/minimal-x86\_64.xml中，再重新生成代码仓库。

