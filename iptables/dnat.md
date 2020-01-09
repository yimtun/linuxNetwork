10.0.0.11 controller



iptables -t nat -A PREROUTING -d 172.16.101.32   -p tcp --dport 80 -j DNAT --to-destination  10.0.0.11:80
iptables -t nat -A POSTROUTING -j MASQUERADE






