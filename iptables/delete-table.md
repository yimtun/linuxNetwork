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
