```
[root@localhost ~]# ip a | grep vnet0
51: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 1000
[root@localhost ~]# ip a | grep vnet1
52: vnet1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr-203 state UNKNOWN group default qlen 1000
[root@localhost ~]# brctl  addif virbr-10 vnet0
[root@localhost ~]# ps -ef |grep qemu | grep vnet0
[root@localhost ~]# ip a | grep vnet1
52: vnet1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr-203 state UNKNOWN group default qlen 1000
[root@localhost ~]# ip a | grep vnet0
51: vnet0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master virbr-10 state UNKNOWN group default qlen 1000
[root@localhost ~]# 
```
