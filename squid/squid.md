```
yum install squid -y
```



```
cache_mem 64 MB                                        
maximum_object_size 4 MB
cache_dir ufs /var/spool/squid 100 16 256
access_log /var/log/squid/access.log
```


```
systemctl start   squid.service
systemctl enable   squid.service
```



```
export https_proxy=http://192.168.94.128:3128
export http_proxy=http://192.168.94.128:3128
```
