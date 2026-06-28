## Which pings succeeded?

All pings have succeeded.

```
docker exec clab-routing-basics-host1 ping -c 3 10.1.1.1
docker exec clab-routing-basics-host2 ping -c 3 10.1.4.1
docker exec clab-routing-basics-host3 ping -c 3 10.1.5.1
PING 10.1.1.1 (10.1.1.1) 56(84) bytes of data.
64 bytes from 10.1.1.1: icmp_seq=1 ttl=64 time=1.55 ms
64 bytes from 10.1.1.1: icmp_seq=2 ttl=64 time=1.12 ms
64 bytes from 10.1.1.1: icmp_seq=3 ttl=64 time=1.09 ms

--- 10.1.1.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 1.086/1.254/1.554/0.212 ms
PING 10.1.4.1 (10.1.4.1) 56(84) bytes of data.
64 bytes from 10.1.4.1: icmp_seq=1 ttl=64 time=0.943 ms
64 bytes from 10.1.4.1: icmp_seq=2 ttl=64 time=2.19 ms
64 bytes from 10.1.4.1: icmp_seq=3 ttl=64 time=1.44 ms

--- 10.1.4.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2025ms
rtt min/avg/max/mdev = 0.943/1.523/2.189/0.512 ms
PING 10.1.5.1 (10.1.5.1) 56(84) bytes of data.
64 bytes from 10.1.5.1: icmp_seq=1 ttl=64 time=0.987 ms
64 bytes from 10.1.5.1: icmp_seq=2 ttl=64 time=1.07 ms
64 bytes from 10.1.5.1: icmp_seq=3 ttl=64 time=1.31 ms

--- 10.1.5.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 0.987/1.121/1.311/0.138 ms

docker exec clab-routing-basics-host1 ping -c 3 10.1.4.2
docker exec clab-routing-basics-host1 ping -c 3 10.1.5.2
docker exec clab-routing-basics-host2 ping -c 3 10.1.5.2
PING 10.1.4.2 (10.1.4.2) 56(84) bytes of data.
64 bytes from 10.1.4.2: icmp_seq=1 ttl=62 time=0.472 ms
64 bytes from 10.1.4.2: icmp_seq=2 ttl=62 time=0.589 ms
64 bytes from 10.1.4.2: icmp_seq=3 ttl=62 time=0.462 ms

--- 10.1.4.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2036ms
rtt min/avg/max/mdev = 0.462/0.507/0.589/0.057 ms
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=62 time=0.433 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=62 time=0.550 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=62 time=0.528 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2048ms
rtt min/avg/max/mdev = 0.433/0.503/0.550/0.050 ms
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=61 time=0.779 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=61 time=0.799 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=61 time=0.793 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2052ms
rtt min/avg/max/mdev = 0.779/0.790/0.799/0.008 m
```

## Routing table output from srl1 showing both directly connected and static routes

```
docker exec -it clab-routing-basics-srl1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
------------------------------------------------------------------------------------------------------
+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+
| Prefi |  ID   | Route | Route | Activ | Origi | Metri | Pref  | Next- | Next- | Backu | Backu |
|   x   |       | Type  | Owner |   e   | n Net |   c   |       | hop ( | hop I |   p   |   p   |
|       |       |       |       |       | work  |       |       | Type) | nterf | Next- | Next- |
|       |       |       |       |       | Insta |       |       |       |  ace  | hop ( | hop I |
|       |       |       |       |       |  nce  |       |       |       |       | Type) | nterf |
|       |       |       |       |       |       |       |       |       |       |       |  ace  |
+=======+=======+=======+=======+=======+=======+=======+=======+=======+=======+=======+=======+
| 10.1. | 2     | local | net_i | True  | defau | 0     | 0     | 10.1. | ether |       |       |
| 1.0/2 |       |       | nst_m |       | lt    |       |       | 1.1 ( | net-  |       |       |
| 4     |       |       | gr    |       |       |       |       | direc | 1/1.0 |       |       |
|       |       |       |       |       |       |       |       | t)    |       |       |       |
| 10.1. | 2     | host  | net_i | True  | defau | 0     | 0     | None  | None  |       |       |
| 1.1/3 |       |       | nst_m |       | lt    |       |       | (extr |       |       |       |
| 2     |       |       | gr    |       |       |       |       | act)  |       |       |       |
| 10.1. | 2     | host  | net_i | True  | defau | 0     | 0     | None  |       |       |       |
| 1.255 |       |       | nst_m |       | lt    |       |       | (broa |       |       |       |
| /32   |       |       | gr    |       |       |       |       | dcast |       |       |       |
|       |       |       |       |       |       |       |       | )     |       |       |       |
| 10.1. | 3     | local | net_i | True  | defau | 0     | 0     | 10.1. | ether |       |       |
| 2.0/2 |       |       | nst_m |       | lt    |       |       | 2.1 ( | net-  |       |       |
| 4     |       |       | gr    |       |       |       |       | direc | 1/2.0 |       |       |
|       |       |       |       |       |       |       |       | t)    |       |       |       |
| 10.1. | 3     | host  | net_i | True  | defau | 0     | 0     | None  | None  |       |       |
| 2.1/3 |       |       | nst_m |       | lt    |       |       | (extr |       |       |       |
| 2     |       |       | gr    |       |       |       |       | act)  |       |       |       |
| 10.1. | 3     | host  | net_i | True  | defau | 0     | 0     | None  |       |       |       |
| 2.255 |       |       | nst_m |       | lt    |       |       | (broa |       |       |       |
| /32   |       |       | gr    |       |       |       |       | dcast |       |       |       |
|       |       |       |       |       |       |       |       | )     |       |       |       |
| 10.1. | 4     | local | net_i | True  | defau | 0     | 0     | 10.1. | ether |       |       |
| 3.0/2 |       |       | nst_m |       | lt    |       |       | 3.1 ( | net-  |       |       |
| 4     |       |       | gr    |       |       |       |       | direc | 1/3.0 |       |       |
|       |       |       |       |       |       |       |       | t)    |       |       |       |
| 10.1. | 4     | host  | net_i | True  | defau | 0     | 0     | None  | None  |       |       |
| 3.1/3 |       |       | nst_m |       | lt    |       |       | (extr |       |       |       |
| 2     |       |       | gr    |       |       |       |       | act)  |       |       |       |
| 10.1. | 4     | host  | net_i | True  | defau | 0     | 0     | None  |       |       |       |
| 3.255 |       |       | nst_m |       | lt    |       |       | (broa |       |       |       |
| /32   |       |       | gr    |       |       |       |       | dcast |       |       |       |
|       |       |       |       |       |       |       |       | )     |       |       |       |
| 10.1. | 0     | stati | stati | True  | defau | 1     | 5     | 10.1. | ether |       |       |
| 4.0/2 |       | c     | c_rou |       | lt    |       |       | 2.0/2 | net-  |       |       |
| 4     |       |       | te_mg |       |       |       |       | 4 (in | 1/2.0 |       |       |
|       |       |       | r     |       |       |       |       | direc |       |       |       |
|       |       |       |       |       |       |       |       | t/loc |       |       |       |
|       |       |       |       |       |       |       |       | al)   |       |       |       |
| 10.1. | 0     | stati | stati | True  | defau | 1     | 5     | 10.1. | ether |       |       |
| 5.0/2 |       | c     | c_rou |       | lt    |       |       | 3.0/2 | net-  |       |       |
| 4     |       |       | te_mg |       |       |       |       | 4 (in | 1/3.0 |       |       |
|       |       |       | r     |       |       |       |       | direc |       |       |       |
|       |       |       |       |       |       |       |       | t/loc |       |       |       |
|       |       |       |       |       |       |       |       | al)   |       |       |       |
+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+-------+
------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 11
IPv4 prefixes with active routes     : 11
IPv4 prefixes with active ECMP routes: 0
-----------------------------------------------------------------------------------------------------
```


