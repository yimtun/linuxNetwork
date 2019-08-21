kvm nat1  nat2



```
vm1 nat1
ping nat2 gw  不走nat1 网关直接到达 nat2的网关 也就是nat2 在宿主机上的地址
```




```
[root@controller ~]# traceroute 203.0.113.229
traceroute to 203.0.113.229 (203.0.113.229), 30 hops max, 60 byte packets
 1  gateway (10.0.0.1)  0.564 ms  0.489 ms  0.446 ms
 2  203.0.113.229 (203.0.113.229)  2.573 ms  2.538 ms  2.506 ms


[root@controller ~]# traceroute 203.0.113.1         kvm 虚拟机地址
traceroute to 203.0.113.1 (203.0.113.1), 30 hops max, 60 byte packets
 1  gateway (10.0.0.1)  1.079 ms  1.006 ms  0.960 ms
 2  203.0.113.1 (203.0.113.1)  0.930 ms  0.900 ms  0.865 ms

[root@controller ~]# traceroute 203.0.113.100  100  是宿主机的地址
traceroute to 203.0.113.100 (203.0.113.100), 30 hops max, 60 byte packets
 1  203.0.113.100 (203.0.113.100)  0.819 ms  0.668 ms  0.582 ms
```



在Linux中的虚拟机的网卡都包含前半段和后半段，前半段在虚拟机上，后半段在宿主机上。
上图eth0为虚拟机上的网卡，对应的后半段为vnet0，vnet0为tap设备。
在虚拟机上所有发往eth0的数据就直接发往vnet0了，也可以将vnet0看作一块网卡。


在宿主机中创建一个桥设备，把宿主机的eth0放在桥上，这样虚拟机上的eth0将报文发给vnet0，
再直接发给宿主机上的eth0，将源地址改为宿主机上的eth0的地址
