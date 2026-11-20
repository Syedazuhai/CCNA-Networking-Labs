\# CCNA Lab 03 — Floating Static Routes and Network Failover



\## Project Overview



This practical lab was completed using Cisco Packet Tracer as part of my preparation for the Cisco CCNA 200-301 certification.



The aim was to design and configure a network with a primary route and an alternative backup route, then investigate how the network responds when the primary connection fails.



\## Network Topology



The network consists of:



\- 3 Cisco 2911 routers (R1, R2 and R3)

\- 2 Cisco 2960 switches

\- 2 PCs

\- A primary connection between R1 and R2

\- An alternative path through R3



!\[Network Topology](screenshots/01\_Network\_Topology.png)



\## IP Addressing



| Device | Interface | IP Address |

|---|---|---|

| R1 | G0/0 | 192.168.10.1/24 |

| R1 | G0/1 | 10.0.0.1/30 |

| R1 | G0/2 | 10.0.0.5/30 |

| R2 | G0/0 | 192.168.20.1/24 |

| R2 | G0/1 | 10.0.0.2/30 |

| R2 | G0/2 | 10.0.0.9/30 |

| R3 | G0/0 | 10.0.0.6/30 |

| R3 | G0/1 | 10.0.0.10/30 |

| PC1 | NIC | 192.168.10.10/24 |

| PC2 | NIC | 192.168.20.10/24 |



\## Routing Configuration



\### Primary Static Route — R1



```cisco

ip route 192.168.20.0 255.255.255.0 10.0.0.2 1

```



\### Floating Backup Route — R1



```cisco

ip route 192.168.20.0 255.255.255.0 10.0.0.6 120

```



The primary route uses administrative distance (AD) 1, while the backup route uses AD 120.



The router prefers the primary route when it is available. When the primary route becomes unavailable, the floating static route can be installed in the routing table.



Equivalent primary and backup routes were configured on R2 for the return path.



\## Testing and Verification



\### Test 1 — Normal Network Operation



Verified connectivity between PC1 and PC2 while the primary route was active.



!\[Primary Route](screenshots/02\_Primary\_Route\_AD1.png)



\### Test 2 — Primary Link Failure



Disabled R1's primary interface using:



```cisco

interface gigabitEthernet 0/1

shutdown

```



The routing table showed the floating static route with AD 120 via R3.



!\[Backup Route](screenshots/03\_Backup\_Route\_AD120.png)



\### Test 3 — Verify Alternative Path



Used traceroute from PC1 to PC2 to verify that traffic followed the backup path through R3.



```text

tracert 192.168.20.10

```



!\[Backup Path Traceroute](screenshots/04\_Backup\_Path\_Traceroute.png)



\### Test 4 — Restore Primary Connection



Restored R1's primary interface:



```cisco

interface gigabitEthernet 0/1

no shutdown

```



Verified that the primary static route with AD 1 returned to the routing table.



!\[Automatic Recovery](screenshots/05\_Automatic\_Recovery.png)



\## Results



The lab demonstrated successful network failover using floating static routes.



When the primary connection failed, the backup route became active and traffic was redirected through R3.



After restoring the primary connection, the router returned to the preferred route.



\## Skills Developed



\- Cisco IOS CLI configuration

\- IPv4 addressing and subnetting

\- Static and floating static routing

\- Administrative distance

\- Network redundancy and failover

\- Ping and traceroute troubleshooting

\- Routing table verification

\- Technical documentation



\## Tools Used



Cisco Packet Tracer and Cisco IOS CLI.



\## Conclusion



This project improved my practical understanding of how floating static routes provide a backup path in a network.



It also strengthened my confidence in troubleshooting routing problems, testing network connectivity and verifying configuration changes.

