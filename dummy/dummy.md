> https://docs.openstack.org/operations-guide/ops-network-troubleshooting.html




Create and bring up a dummy interface, snooper0:

```
ip link add name snooper0 type dummy
ip link set dev snooper0 up
```


Add device snooper0 to bridge br-int:

```
ovs-vsctl add-port br-int snooper0
```


> https://blog.csdn.net/xiaoyulovly/article/details/93203634


```
1、创建dummy接口

ip link add dummy1 type dummy

ip link set dummy1 arp on

ip address add 10.0.2.2/24 broadcast + dev dummy1

ip link set dummy1 up

 

2、创建桥接接口

ip link add dummy1 type dummy

ip link add dummy2 type dummy

ip link add dummy-br0 type bridge

ip link set dummy1 arp on

ip link set dummy2 arp on

ip link set dev dummy1 master dummy-br0

ip link set dev dummy2 master dummy-br0

ip address add 10.0.2.1/24 broadcast + dev dummy-br0

ip address add 10.0.2.2/24 broadcast + dev dummy1

ip address add 10.0.2.3/24 broadcast + dev dummy2

ip link set dummy1 up

ip link set dummy2 up

ip link set dummy-br0 up
```
