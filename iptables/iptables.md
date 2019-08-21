centos7 

表名

-t 指定表名

```
filter  nat  mangle raw
```

链 

```
INPUT OUTPUT FORWARD PREROUTING POSTROUTING
```



查看规则
-L 可指定链

```
iptables -L FORWARD -n -t filter   --line-number
```



删除规则


```
iptables -L -n -t filter --line-number
Chain INPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:53
2    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:53
3    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:67
4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:67

Chain FORWARD (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     all  --  0.0.0.0/0            10.0.0.0/24          ctstate RELATED,ESTABLISHED
2    ACCEPT     all  --  10.0.0.0/24          0.0.0.0/0           
3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
4    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
5    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-port-unreachable
6    ACCEPT     all  --  203.0.113.0/24       0.0.0.0/0           

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination         
1    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0            udp dpt:68
```




```
iptables  -t filter -D FORWARD 6
```



ping 不通相关查看  reject-with icmp-port-unreachable

```
iptables -t filter  -L FORWARD
iptables -t filter  -L FORWARD -n
```



设置允许icmp

```
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT
```
???

```
iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT
```


