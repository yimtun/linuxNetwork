kvm 宿主机


kvm 宿主机 清除关于 203.0.113.0/24 网段的 iptables 配置 和 路由配置

```
iptables -L -n -t nat --line-number
iptables -L -n -t filter --line-number
iptables -L -n -t raw --line-number
iptables -L -n -t mangle --line-number
ip route show
```


```
cat /etc/sysconfig/network-scripts/ifcfg-eth0 
DEVICE=eth0
ONBOOT=yes
BRIDGE=br0
```



```
cat /etc/sysconfig/network-scripts/ifcfg-br0 
DEVICE=br0
ONBOOT=yes
BOOTPROTO=static
NM_CONTROLLED=no
IPADDR=192.168.1.100
GATEWAY=192.168.1.1
NETMASK=255.255.255.0
DNS1=223.5.5.5
USERCTL=no
TYPE=Bridge
```

```
cat /etc/sysconfig/network-scripts/ifcfg-br1 
DEVICE=br1
ONBOOT=yes
BOOTPROTO=static
NM_CONTROLLED=no
USERCTL=no
TYPE=Bridge
```


kvm  network

```
virsh net-list
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 br0                  active     yes           yes
 br1                  active     yes           yes
```

```
cat /etc/libvirt/qemu/networks/br0.xml 
<network>
  <name>br0</name>
  <uuid>25890db9-4091-4d23-9f39-1ab8536271cf</uuid>
  <forward mode='bridge'/>
  <bridge name='br0'/>
</network>
```


```
cat /etc/libvirt/qemu/networks/br1.xml 
<network>
  <name>br1</name>
  <uuid>0f54af04-e735-4a75-a40e-61fb2393f6a9</uuid>
  <forward mode='bridge'/>
  <bridge name='br1'/>
</network>
```




vm1   use kvm br1 net

```
203.0.113.229 gw 203.0.113.1
```


vm2  router1  use kvm br1 net

```
203.0.113.1 gw 203.0.113.2
```


```
ip addr add 203.0.113.1/24 dev eth0
ip route add default via 203.0.113.2 dev eth0
echo 1 >  /proc/sys/net/ipv4/ip_forward
```


vm3 use  eth0 use kvm br0 net eth1 use kvm br1 net

```
eth0 
192.168.1.101/24  gw 192.168.1.1

eth1 
203.0.113.2/24  no gw
```


```
ip addr add 203.0.113.2/24 dev eth1
echo 1 >  /proc/sys/net/ipv4/ip_forward
iptables -t nat-A POSTROUTING -s 203.0.113.0/24  -j SNAT --to 192.168.1.101
```



```
指向网关 203.0.113.1 的设备 会报错

[root@controller ~]# ip netns exec qrouter-6c2e391b-22f1-4692-a4d7-bfd24599c600   ping     114.114.114.114
PING 114.114.114.114 (114.114.114.114) 56(84) bytes of data.
64 bytes from 114.114.114.114: icmp_seq=1 ttl=71 time=26.3 ms
From 203.0.113.1 icmp_seq=2 Redirect Host(New nexthop: 203.0.113.2)
From 203.0.113.1: icmp_seq=2 Redirect Host(New nexthop: 203.0.113.2)
64 bytes from 114.114.114.114: icmp_seq=2 ttl=78 time=25.9 ms
From 203.0.113.1: icmp_seq=3 Redirect Host(New nexthop: 203.0.113.2)
From 203.0.113.1 icmp_seq=3 Redirect Host(New nexthop: 203.0.113.2)
64 bytes from 114.114.114.114: icmp_seq=3 ttl=89 time=25.9 ms
From 203.0.113.1 icmp_seq=4 Redirect Host(New nexthop: 203.0.113.2)
From 203.0.113.1: icmp_seq=4 Redirect Host(New nexthop: 203.0.113.2)
64 bytes from 114.114.114.114: icmp_seq=4 ttl=70 time=25.9 ms
From 203.0.113.1 icmp_seq=5 Redirect Host(New nexthop: 203.0.113.2)
From 203.0.113.1: icmp_seq=5 Redirect Host(New nexthop: 203.0.113.2)
64 bytes from 114.114.114.114: icmp_seq=5 ttl=63 time=26.1 ms
```

故障复现  删除apr 记录 重新学习


vm1 
```
ip netns exec qrouter-6c2e391b-22f1-4692-a4d7-bfd24599c600   arp -d 203.0.113.1
ip netns exec qrouter-6c2e391b-22f1-4692-a4d7-bfd24599c600   arp -d 203.0.113.2
ip netns exec qrouter-6c2e391b-22f1-4692-a4d7-bfd24599c600   arp -n
[root@controller ~]#   空
```


vm2
```
arp -d 203.0.113.2
```







```
icmp重定向

ICMP重定向报文是ICMP控制报文中的一种。在特定的情况下，当路由器检测到一台机器使用非优化路由的时候，
它会向该主机发送一个ICMP重定向报文，请求主机改变路由。路由器也会把初始数据包向它的目的地转发。


发生ICMP重定向通常有两种情况：
1)当路由器从某个接口收到数据还需要从相同接口转发该数据时；
2)当路由器从某个接口到发往远程网络的数据时发现源ip地址与下一跳属于同一网段时。





ICMP 重定向消息：如果路由器发现发送端主机使用次优的路径发送数据时，那么它会返回一个 ICMP 重定向消息给这个主机，
这个消息包含了最合适的路由信息和源数据。主要发生在路由器持有更好的路由信息的情况下，
路由器会通过这个 ICMP 重定向消息给发送端主机一个更合适的发送路由。
```











