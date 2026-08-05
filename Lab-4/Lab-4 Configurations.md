# Lab-4
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="600" alt="Local Account, Named ACL, and Security" src="Lab-4 Topology.png" />
</p>

--- 

# Tasks

IP connectivity between the three routers is configured. OSPF adjacencies must be established

1. Configure R1 and R2 Router IDs using the interface IP addresses from the link that is shared
between them.
2. Configure the R2 links with a max value facing R1 and R3. R2 must become the DR. R1 and R3 links facing R2 must remain with the default OSPF configuration for DR election. Verify the configuration after clearing the OSPF process.
3. Using a host wildcard mask, configure all three routers to advertise their respective Loopback1 networks.
4. Configure the link between R1 and R3 to disable their ability to add other OSPF routers

---

# Device Pre-configurations
## R1
```
hostname R1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.255
 no shutdown
!
interface Loopback1
 ip address 192.168.1.1 255.255.255.0
 no shutdown
!
interface Ethernet0/0
 description Link to R2
 ip address 10.10.12.1 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Link to R3
 ip address 10.10.13.1 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.10.12.0 0.0.0.255 area 0
 network 10.10.13.0 0.0.0.255 area 0
!
end
write memory
copy running-config startup-config
```

## R2
```
hostname R2
!
interface Loopback0
 ip address 10.10.2.2 255.255.255.255
 no shutdown
!
interface Loopback1
 ip address 192.168.2.2 255.255.255.0
 no shutdown
!
interface Ethernet0/0
 description Link to R1
 ip address 10.10.12.2 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Link to R3
 ip address 10.10.23.2 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.10.12.0 0.0.0.255 area 0
 network 10.10.23.0 0.0.0.255 area 0
!
end
write memory
copy running-config startup-config
```

## R3
```
hostname R3
!
interface Loopback0
 ip address 10.10.3.3 255.255.255.255
 no shutdown
!
interface Loopback1
 ip address 192.168.3.3 255.255.255.0
 no shutdown
!
interface Ethernet0/0
 description Link to R1
 ip address 10.10.13.3 255.255.255.0
 no shutdown
!
interface Ethernet0/1
 description Link to R2
 ip address 10.10.23.3 255.255.255.0
 no shutdown
!
router ospf 1
 network 10.10.13.0 0.0.0.255 area 0
 network 10.10.23.0 0.0.0.255 area 0
!
end
write memory
copy running-config startup-config
```

---

# Task Configurations
## R1
```
router ospf 1
 router-id 10.10.12.1
 network 192.168.1.1 0.0.0.0 area 0
!
interface Ethernet0/1
 ip ospf network point-to-point
!
end
write memory
copy running-config startup-config
```

## R2
```
router ospf 1
 router-id 10.10.12.2
 network 192.168.2.2 0.0.0.0 area 0
!
interface Ethernet0/0
 ip ospf priority 255
!
interface Ethernet0/1
 ip ospf priority 255
!
end
write memory
copy running-config startup-config
```

## R3
```
router ospf 1
 network 192.168.3.3 0.0.0.0 area 0
!
interface Ethernet0/0
 ip ospf network point-to-point
!
end
write memory
copy running-config startup-config
```
