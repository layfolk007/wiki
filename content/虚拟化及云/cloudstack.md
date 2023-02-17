### cloudstack4.4添加XenServer6.x 出现问题时，尝试以下方法：

I haven't managed to read every post in this thread, but I think I get the gist of what's being asked.

Since XenServer 6.1 openvswitch has become the default stack rather than bridge mode.

To enable bridge mode for security groups you need to do the

xe-switch-network-backend bridge

command and also set

net.bridge.bridge-nf-call-iptables = 1

net.bridge.bridge-nf-call-arptables = 1

in /etc/sysctl.conf

```
sed -i -e '/net.bridge.bridge-nf-call-iptables/ c\net.bridge.bridge-nf-call-iptables = 1' -e '/net.bridge.bridge-nf-call-arptables/ c\net.bridge.bridge-nf-call-arptables = 1' /etc/sysctl.conf
```

should do that for you

then reboot

