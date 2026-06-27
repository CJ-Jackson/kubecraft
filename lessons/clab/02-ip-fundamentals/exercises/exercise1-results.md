# Exercise 1

## Which pings succeeded and which failed

```
docker exec clab-ip-fundamentals-host1 ping -c 3 10.1.1.1
docker exec clab-ip-fundamentals-host2 ping -c 3 10.1.3.1

PING 10.1.1.1 (10.1.1.1): 56 data bytes
64 bytes from 10.1.1.1: seq=0 ttl=64 time=1.152 ms
64 bytes from 10.1.1.1: seq=1 ttl=64 time=2.041 ms
64 bytes from 10.1.1.1: seq=2 ttl=64 time=1.987 ms

--- 10.1.1.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 1.152/1.726/2.041 ms
PING 10.1.3.1 (10.1.3.1): 56 data bytes
64 bytes from 10.1.3.1: seq=0 ttl=64 time=3.068 ms
64 bytes from 10.1.3.1: seq=1 ttl=64 time=1.949 ms
64 bytes from 10.1.3.1: seq=2 ttl=64 time=1.828 ms

--- 10.1.3.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 1.828/2.281/3.068 ms
```

The ping from host1 to 10.1.1.1 and host2 to 10.1.3.1 have both succeeded

But

```
docker exec clab-ip-fundamentals-host1 ping -c 3 10.1.3.2
PING 10.1.3.2 (10.1.3.2): 56 data bytes
^C%
```

The cross-subnet ping from host1 to 10.1.3.2 has failed.

## The ARP Table

```
docker exec clab-ip-fundamentals-host1 arp -n
? (10.1.1.1) at 1a:53:02:ff:00:01 [ether]  on eth1
? (172.20.20.1) at 3a:6b:35:2c:fb:d0 [ether]  on eth0
```
```

+---------+---------+---------------+---------+-----------------+---------------------------------+
| Interfa | Subinte |   Neighbor    | Origin  |   Link layer    |             Expiry              |
|   ce    |  rface  |               |         |     address     |                                 |
+=========+=========+===============+=========+=================+=================================+
| etherne |       0 |      10.1.1.2 | dynamic | AA:C1:AB:AA:86: | 3 hours from now                |
| t-1/1   |         |               |         | 8E              |                                 |
| etherne |       0 |      10.1.2.2 | dynamic | 1A:20:03:FF:00: | 3 hours from now                |
| t-1/2   |         |               |         | 01              |                                 |
| mgmt0   |       0 |   172.20.20.1 | dynamic | 3A:6B:35:2C:FB: | 3 hours from now                |
|         |         |               |         | D0              |                                 |
+---------+---------+---------------+---------+-----------------+---------------------------------+
------------------------------------------------------------------------------------------------------
  Total entries : 3 (0 static, 3 dynamic)
-----------------------------------------------------------------------------------------------------
```
```
```

## Explanation of why cross-subnet ping fails

Because no ip route or APR was setup for that IP address.
