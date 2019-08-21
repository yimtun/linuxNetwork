添加一条规则到尾部


```
iptables -A INPUT -t filter -s 192.168.1.5 -j DROP
```


```
iptables -L  INPUT -t filter  

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     udp  --  anywhere             anywhere             udp dpt:domain
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
ACCEPT     udp  --  anywhere             anywhere             udp dpt:bootps
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:bootps
DROP       all  --  192.168.1.5          anywhere
```


```
[root@localhost ~]# iptables -L  INPUT -t filter  -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
DROP       all  --  192.168.1.5          0.0.0.0/0
[root@localhost ~]# iptables -L  INPUT -t filter  -n --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
5    DROP       all  --  192.168.1.5          0.0.0.0/0
```





再插入一条规则为第五行，将行数直接写到规则链的后面  原来第五行至后面的规则会往下排

```
iptables -I INPUT  5 -t filter -s 192.168.1.6 -j DROP
```


```
iptables -L  INPUT -t filter  -n --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
5    DROP       all  --  192.168.1.6          0.0.0.0/0
6    DROP       all  --  192.168.1.5          0.0.0.0/0
```



删除 5   6

使用编号删除
```
iptables -D INPUT 5 -t filter
```

使用语句删除

```
iptables -D INPUT   -t filter -s 192.168.1.6 -j DROP
iptables -D INPUT   -t filter -s 192.168.1.5 -j DROP
```




修改


iptables -R INPUT 5 -j ACCEPT



```
iptables -L  INPUT -t filter  -n --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
[root@localhost ~]# iptables -A INPUT -t filter -s 192.168.1.5 -j DROP
[root@localhost ~]# iptables -L  INPUT -t filter  -n --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
5    DROP       all  --  192.168.1.5          0.0.0.0/0           
[root@localhost ~]# iptables -R INPUT 5 -j ACCEPT
[root@localhost ~]# iptables -L  INPUT -t filter  -n --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67
5    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
```




#### reject-with

```
iptables -A INPUT -p tcp -m tcp --dport 5555 -j REJECT --reject-with icmp-port-unreachable
```

```
iptables -L FORWARD -n -t filter --line-number
Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination
1    ACCEPT     all  --  0.0.0.0/0            203.0.113.0/24       ctstate RELATED,ESTABLISHED
2    ACCEPT     all  --  203.0.113.0/24       0.0.0.0/0
3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
4    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
5    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
6    ACCEPT     all  --  0.0.0.0/0            10.0.0.0/24          ctstate RELATED,ESTABLISHED
7    ACCEPT     all  --  10.0.0.0/24          0.0.0.0/0
8    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
9    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
10   REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
11   ACCEPT     all  --  0.0.0.0/0            192.168.122.0/24     ctstate RELATED,ESTABLISHED
12   ACCEPT     all  --  192.168.122.0/24     0.0.0.0/0
13   ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
14   REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
15   REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
```



```
iptables -A FORWARD -t filter -s 0.0.0.0/0 -d 0.0.0.0/0 -j REJECT --reject-with icmp-port-unreachable
```

```
iptables -L FORWARD -n -t filter --line-number
```


删除

```
iptables -D  FORWARD -t filter -s 0.0.0.0/0 -d 0.0.0.0/0 -j REJECT --reject-with icmp-port-unreachable
```













