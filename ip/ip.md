添加地址

```
ip addr add 203.0.113.1/24 dev eth0
```


删除地址


```
ip addr del   203.0.113.100/24  dev br1
```


设置默认网关

```
ip route add default via 203.0.113.2 dev eth0
```
