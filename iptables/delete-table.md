```
iptables -F -t filter
iptables -L -t filter

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
[root@localhost ~]# 
```

设置链的默认规则

```
iptables -P FORWARD ACCEPT -t filter
```

清空一个表中的一个链

```
iptables -F FORWARD -t filter
```


删除一条规则

```
iptables -t nat -L  --line-numbers

iptables -t nat -L -n  --line-numbers

iptables -t nat -L  --line-numbers
Chain PREROUTING (policy ACCEPT)
num  target     prot opt source               destination         
1    DNAT       tcp  --  anywhere             localhost.localdomain  tcp dpt:ms-wbt-server to:172.16.101.249:3389
2    DNAT       tcp  --  anywhere             localhost.localdomain  tcp dpt:http to:10.255.0.6:80

```

```
iptables  -t nat -D PREROUTING 2
```







