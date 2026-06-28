## Traceroute Output

### Before

```
docker exec clab-dynamic-routing-bgp-host2 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.4.1  0.520 ms  0.504 ms  0.499 ms
 2  10.1.2.1  1.277 ms  1.287 ms  2.287 ms
 3  10.1.3.2  2.606 ms  2.855 ms  2.980 ms
 4  10.1.5.2  3.108 ms  3.129 ms  3.142 ms
```

### After

```
 docker exec clab-dynamic-routing-bgp-host2 traceroute -n -w 2 10.1.5.2
traceroute to 10.1.5.2 (10.1.5.2), 30 hops max, 60 byte packets
 1  10.1.4.1  1.375 ms  1.359 ms  1.417 ms
 2  10.1.6.2  1.898 ms  1.904 ms  1.964 ms
 3  10.1.5.2  0.936 ms  1.622 ms  1.639 ms
```

## BGP route detail showing two paths with different AS path lengths

```
 docker exec -it clab-dynamic-routing-bgp-srl2 sr_cli -c "show network-instance default protocols bgp routes ipv4 summary"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Show report for the BGP route table of network-instance "default"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Status codes: u=used, *=valid, >=best, x=stale, b=backup
Origin codes: i=IGP, e=EGP, ?=incomplete
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
+--------+----------------------+---------------------------------+--------+--------+----------------------------------------------------------------+
| Status |       Network        |            Next Hop             |  MED   | LocPre |                            Path Val                            |
|        |                      |                                 |        |   f    |                                                                |
+========+======================+=================================+========+========+================================================================+
| u*>    | 10.1.1.0/24          | 10.1.2.1                        |        |        | [65001] i                                                      |
| *      | 10.1.1.0/24          | 10.1.6.2                        |        |        | [65003, 65001] i                                               |
| u*>    | 10.1.2.0/24          | 0.0.0.0                         |        |        |  i                                                             |
| *      | 10.1.2.0/24          | 10.1.2.1                        |        |        | [65001] i                                                      |
| *      | 10.1.2.0/24          | 10.1.6.2                        |        |        | [65003, 65001] i                                               |
| u*>    | 10.1.3.0/24          | 10.1.2.1                        |        |        | [65001] i                                                      |
| *      | 10.1.3.0/24          | 10.1.6.2                        |        |        | [65003] i                                                      |
| u*>    | 10.1.4.0/24          | 0.0.0.0                         |        |        |  i                                                             |
| *      | 10.1.5.0/24          | 10.1.2.1                        |        |        | [65001, 65003] i                                               |
| u*>    | 10.1.5.0/24          | 10.1.6.2                        |        |        | [65003] i                                                      |
| u*>    | 10.1.6.0/24          | 0.0.0.0                         |        |        |  i                                                             |
| *      | 10.1.6.0/24          | 10.1.6.2                        |        |        | [65003] i                                                      |
+--------+----------------------+---------------------------------+--------+--------+----------------------------------------------------------------+
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
12 received BGP routes: 6 used, 12 valid, 0 stale
6 available destinations: 6 with ECMP multipaths
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

## Explanation of which best path algorithm step decided the winner

```
docker exec -it clab-dynamic-routing-bgp-srl2 sr_cli -c "show network-instance default protocols bgp routes ipv4 prefix 10.1.5.0/24 detail"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Show report for the BGP routes to network "10.1.5.0/24" network-instance  "default"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Network: 10.1.5.0/24
Received Paths: 2
  Path 1: <Valid,>
    Route source    : neighbor 10.1.2.1
    Route Preference: MED is -, No LocalPref
    BGP next-hop    : 10.1.2.1
    Path            :  i [65001, 65003]
    Communities     : None
    RR Attributes   : No Originator-ID, Cluster-List is [ - ]
    Aggregation     : Not an aggregate route
    Unknown Attr    : None
    Invalid Reason  : None
    Tie Break Reason: as-path-length
  Path 2: <Best,Valid,Used,>
    Route source    : neighbor 10.1.6.2
    Route Preference: MED is -, No LocalPref
    BGP next-hop    : 10.1.6.2
    Path            :  i [65003]
    Communities     : None
    RR Attributes   : No Originator-ID, Cluster-List is [ - ]
    Aggregation     : Not an aggregate route
    Unknown Attr    : None
    Invalid Reason  : None
    Tie Break Reason: none

Path 2 was advertised to:
[ 10.1.2.1 ]
Route Preference: MED is -, No LocalPref
Path            :  i [65002, 65003]
Communities     : None
RR Attributes   : No Originator-ID, Cluster-List is [ - ]
Aggregation     : Not an aggregate route
Unknown Attr    : None
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

Path 2 won because it only has to go though one link, while Path 1 has to go though two links.
