interface exists:

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0@if37: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 4e:6e:19:0f:ab:89 brd ff:ff:ff:ff:ff:ff
    inet 172.20.20.2/24 brd 172.20.20.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 3fff:172:20:20::2/64 scope global flags 02
       valid_lft forever preferred_lft forever
    inet6 fe80::4c6e:19ff:fe0f:ab89/64 scope link
       valid_lft forever preferred_lft forever
40: eth1@if39: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 9500 qdisc noqueue state UP
    link/ether aa:c1:ab:33:0b:91 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::a8c1:abff:fe33:b91/64 scope link
       valid_lft forever preferred_lft forever

```
