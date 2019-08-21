```
常用的掩码是24位的

0-255
0   是网络号
255 是广播地址






如果掩码是26位的，分为4个段，1-62/65-126/129-190/193-254

0-63  
0 网络号 
63 广播地址

可用地址 1-62



64-127
64  网络号
127 广播地址

可用地址 65-126

128-191
128 网络号
191 广播地址

可用地址 129-190


192-255
192 网络号
155 广播地址 

可用地址 193-254


10.20.1.0/26 指的就是0-63


自动计算网络号


255.255.255.192




openstack subnet create --network provider   --subnet-range 10.20.1.64/26 --allocation-pool start=10.20.1.70,end=10.20.1.120   --dns-nameserver 114.114.114.114 --gateway 10.20.1.65    provider




eth1
DEVICE=eth1
ONBOOT=yes
TYPE=Ethernet
BOOTPROTO=none
```
