```
brctl setageing br0 0
```

```
brctl setfd br0 0
```

```
brctl stp  br0  off
```

创建网桥 设置地址启动网桥

```
brctl  addbr  br1
ip addr add 203.0.113.1/24 dev br1
ip link set up br1
```
