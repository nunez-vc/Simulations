# Lab-3: Static Routing with Redundant Links and Floating Routs
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="300" alt="Local Account, Named ACL, and Security" src="Lab-3 Topology.png" />
</p>

--- 

# Task
```
IP connectivity and OSPF are preconfigured on all devices where necessary. Do not make any changes to the IP addressing or OSPF.
The company policy uses connected interfaces and next hops when configuring static routes except for load balancing or
redundancy without floating static. Connectivity must be established between subnet 172.20.20.128/25 on the Internet and the LAN
at 192.168.0.0/24 connected to SW1:
```

## Task 1
```
Configure reachability to the switch SW1 LAN subnet in router R2.
```

## Task 2
```
Configure default reachability to the Internet subnet in router R1.
```

## Task 3
```
Configure a single static route in router R2 to reach to the Internet subnet considering both redundant links between
routers R1 and R2. A default route is NOT allowed in router R2.
```

## Task 4
```
Configure a static route in router R1 toward the switch SW1 LAN subnet where the primary link must be through Ethernet0/1,
and the backup link must be through Ethernet0/2 using a floating route. Use the minimal administrative distance value when
required.
```

---

# Device Pre-configurations
## R1
```
hostname R1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.255
!
interface Ethernet0/0
 description Link-to-R3
 ip address 10.10.13.1 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Primary-Link-to-R2
 ip address 10.10.12.1 255.255.255.128
 no shutdown
!
interface Ethernet0/2
 description Backup-Link-to-R2
 ip address 10.10.12.129 255.255.255.128
 no shutdown
!
router ospf 1
 router-id 1.1.1.1
 network 10.10.1.1 0.0.0.0 area 0
 network 10.10.12.0 0.0.0.127 area 0
 network 10.10.12.128 0.0.0.127 area 0
 network 10.10.13.0 0.0.0.255 area 0
 end
!
write memory
yes
!
copy running-config startup-config
```

## R2
```
hostname R2
!
interface Ethernet0/0
 description Link-to-SW1
 ip address 10.10.31.2 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Primary-Link-to-R1
 ip address 10.10.12.2 255.255.255.128
 no shutdown
!
interface Ethernet0/2
 description Backup-Link-to-R1
 ip address 10.10.12.130 255.255.255.128
 no shutdown
!
router ospf 1
 router-id 2.2.2.2
 network 10.10.12.0 0.0.0.127 area 0
 network 10.10.12.128 0.0.0.127 area 0
 end
!
write memory
yes
!
copy running-config startup-config
```

## R3
```
hostname R3
!
interface Ethernet0/0
 description Link-to-R1
 ip address 10.10.13.3 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Link-to-Internet
 ip address 10.10.254.3 255.255.255.0
 no shutdown
!
router ospf 1
 router-id 3.3.3.3
 network 10.10.13.0 0.0.0.255 area 0
 end
!
write memory
yes
!
copy running-config startup-config
```

## SW1
```
hostname SW1
!
interface Ethernet0/0
 description Link-to-R2
 no switchport
 ip address 10.10.31.1 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description User-LAN
 no switchport
 ip address 192.168.0.1 255.255.255.0
 no shutdown
!
ip routing
end
!
write memory
yes
!
copy running-config startup-config
```

---

# Task Configurations
## Task 1: Configure reachability to the SW1 LAN subnet in R2.
```
ip route 192.168.0.0 255.255.255.0 Ethernet0/0
```

## Task 2: Configure default reachability to the Internet subnet in R1.
```
ip route 0.0.0.0 0.0.0.0 Ethernet0/0 10.10.13.3
```

## Task 3: Configure a single static route in R2 toward the Internet subnet using both redundant links.
```
ip route 172.20.20.128 255.255.255.128 10.10.12.1 10.10.12.129
```

## Task 4: Configure static routes in R1 toward the SW1 LAN.
```
ip route 192.168.0.0 255.255.255.0 Ethernet0/1 10.10.12.2
ip route 192.168.0.0 255.255.255.0 Ethernet0/2 10.10.12.130 2
```
