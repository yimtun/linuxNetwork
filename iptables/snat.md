kvm 宿主机

虚拟机使用kvm 网络 br1 
kvm br1 使用宿主机上的网桥设备 br1 是一个虚拟接口 没有接入任何物理网卡

虚拟机 通过 kvm 宿主机的网络br0 可以访问互联网 
虚拟机的网关指定为  宿主机br1 的地址 203.0.113.100



```
echo 1 >  /proc/sys/net/ipv4/ip_forward
```

```
cat ifcfg-br1
DEVICE=br1
ONBOOT=yes
BOOTPROTO=static
NM_CONTROLLED=no
IPADDR=203.0.113.100
NETMASK=255.255.255.0
USERCTL=no
TYPE=Bridge
```


```
<network>
  <name>br1</name>
  <uuid>0f54af04-e735-4a75-a40e-61fb2393f6a9</uuid>
  <forward mode='bridge'/>
  <bridge name='br1'/>
</network>
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
cat /etc/libvirt/qemu/networks/br0.xml 
<network>
  <name>br0</name>
  <uuid>25890db9-4091-4d23-9f39-1ab8536271cf</uuid>
  <forward mode='bridge'/>
  <bridge name='br0'/>
</network>
```



vm   eth1  bridge br1


```
ip 203.0.113.1
ip route add default via 203.0.113.100 dev eth1
```

```
ip route show
```

```
default via 203.0.113.100 dev eth1
169.254.0.0/16 dev eth1 scope link metric 1003
203.0.113.0/24 dev eth1 proto kernel scope link src 203.0.113.1
```





1 filter 表 FORWARD 链允许203.0.113.0/24 通过


```
iptables -A FORWARD -t filter -s 203.0.113.0/24 -j ACCEPT
```
查看

```
iptables -L FORWARD -t filter -n
```


2 做地址伪装 通过br0 访问互联网  还可以使用SNAT 策略
```
iptables -t nat -APOSTROUTING -s 203.0.113.0/24  -o br0  -j MASQUERADE
```

优化 排除本网段

```
iptables -t nat -APOSTROUTING -s 203.0.113.0/24   !  -d 203.0.113.0/24 -o br0  -j MASQUERADE
```

查看

```
iptables -t nat -L POSTROUTING -n
```


