查看本机路由

```
<network>
  <name>br1</name>
  <uuid>510682ab-7fd7-49f8-a21d-729cfe9b55a1</uuid>
  <forward mode='bridge'/>
  <bridge name='br1'/>
</network>
```

```
ip route show
```


router  vm

eth0 br0
eth1 br1

```
172.16.10.1/24   eth0  default gw 172.16.11.2   internet
203.0.113.1/24   eth1
```




client vm

eth0 br1

```
203.0.113.250/24  eth0  default gw 203.0.113.1
```


router vm

echo 1 >  /proc/sys/net/ipv4/ip_forward


方案1

```
iptables -t nat-A POSTROUTING -s 203.0.113.0/24  -j SNAT --to 172.16.10.1
```



方案2

```
iptables -t nat -APOSTROUTING -s 203.0.113.0/24   !  -d 203.0.113.0/24 -o  eth0   -j MASQUERADE
```



```
???

iptables -A FORWARD -t filter -s 203.0.113.0/24 -j ACCEPT
```



