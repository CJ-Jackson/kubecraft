## Routing Table

### Before

```
docker exec -it clab-dynamic-routing-bgp-srl1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+------------------------------+-------+------------+----------------------+----------+----------+---------+------------+------------------+------------------+------------------+-----------------------------+
|            Prefix            |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    | Next-hop (Type)  |     Next-hop     | Backup Next-hop  |  Backup Next-hop Interface  |
|                              |       |            |                      |          | Network  |         |            |                  |    Interface     |      (Type)      |                             |
|                              |       |            |                      |          | Instance |         |            |                  |                  |                  |                             |
+==============================+=======+============+======================+==========+==========+=========+============+==================+==================+==================+=============================+
| 10.1.1.0/24                  | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.1.1         | ethernet-1/1.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.1.1/32                  | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.1.255/32                | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
| 10.1.2.0/24                  | 3     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.2.1         | ethernet-1/2.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.2.1/32                  | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.2.255/32                | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
| 10.1.3.0/24                  | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.3.1         | ethernet-1/3.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.3.1/32                  | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.3.255/32                | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
+------------------------------+-------+------------+----------------------+----------+----------+---------+------------+------------------+------------------+------------------+-----------------------------+
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 9
IPv4 prefixes with active routes     : 9
IPv4 prefixes with active ECMP routes: 0
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

### After

```
docker exec -it clab-dynamic-routing-bgp-srl1 sr_cli -c "show network-instance default route-table ipv4-unicast summary"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 unicast route table of network instance default
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+------------------------------+-------+------------+----------------------+----------+----------+---------+------------+------------------+------------------+------------------+-----------------------------+
|            Prefix            |  ID   | Route Type |     Route Owner      |  Active  |  Origin  | Metric  |    Pref    | Next-hop (Type)  |     Next-hop     | Backup Next-hop  |  Backup Next-hop Interface  |
|                              |       |            |                      |          | Network  |         |            |                  |    Interface     |      (Type)      |                             |
|                              |       |            |                      |          | Instance |         |            |                  |                  |                  |                             |
+==============================+=======+============+======================+==========+==========+=========+============+==================+==================+==================+=============================+
| 10.1.1.0/24                  | 2     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.1.1         | ethernet-1/1.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.1.1/32                  | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.1.255/32                | 2     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
| 10.1.2.0/24                  | 3     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.2.1         | ethernet-1/2.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.2.1/32                  | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.2.255/32                | 3     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
| 10.1.3.0/24                  | 4     | local      | net_inst_mgr         | True     | default  | 0       | 0          | 10.1.3.1         | ethernet-1/3.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (direct)         |                  |                  |                             |
| 10.1.3.1/32                  | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (extract)   | None             |                  |                             |
| 10.1.3.255/32                | 4     | host       | net_inst_mgr         | True     | default  | 0       | 0          | None (broadcast) |                  |                  |                             |
| 10.1.4.0/24                  | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.2.0/24      | ethernet-1/2.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (indirect/local) |                  |                  |                             |
| 10.1.5.0/24                  | 0     | bgp        | bgp_mgr              | True     | default  | 0       | 170        | 10.1.3.0/24      | ethernet-1/3.0   |                  |                             |
|                              |       |            |                      |          |          |         |            | (indirect/local) |                  |                  |                             |
+------------------------------+-------+------------+----------------------+----------+----------+---------+------------+------------------+------------------+------------------+-----------------------------+
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
IPv4 routes total                    : 11
IPv4 prefixes with active routes     : 11
IPv4 prefixes with active ECMP routes: 0
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

## All cross-subnet pings succeeding

```
docker exec clab-dynamic-routing-bgp-host1 ping -c 3 10.1.4.2
docker exec clab-dynamic-routing-bgp-host1 ping -c 3 10.1.5.2
docker exec clab-dynamic-routing-bgp-host2 ping -c 3 10.1.5.2
PING 10.1.4.2 (10.1.4.2) 56(84) bytes of data.
64 bytes from 10.1.4.2: icmp_seq=1 ttl=62 time=0.438 ms
64 bytes from 10.1.4.2: icmp_seq=2 ttl=62 time=0.523 ms
64 bytes from 10.1.4.2: icmp_seq=3 ttl=62 time=0.572 ms

--- 10.1.4.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2029ms
rtt min/avg/max/mdev = 0.438/0.511/0.572/0.055 ms
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=62 time=110 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=62 time=0.608 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=62 time=0.559 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2015ms
rtt min/avg/max/mdev = 0.559/36.981/109.777/51.474 ms
PING 10.1.5.2 (10.1.5.2) 56(84) bytes of data.
64 bytes from 10.1.5.2: icmp_seq=1 ttl=61 time=0.737 ms
64 bytes from 10.1.5.2: icmp_seq=2 ttl=61 time=0.862 ms
64 bytes from 10.1.5.2: icmp_seq=3 ttl=61 time=0.894 ms

--- 10.1.5.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2043ms
rtt min/avg/max/mdev = 0.737/0.831/0.894/0.067 m
```
