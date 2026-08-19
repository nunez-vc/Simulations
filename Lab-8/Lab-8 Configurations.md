# Lab-8
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="400" alt="Local Account, Named ACL, and Security" src="Lab-8 Topology.png" />
</p>

## Tasks:
R1 and R2 are pre-configured with all the necessary commands. All physical cabling is in place and verified.
Connectivity for PC1 and PC2 must be established to the switches; each port must only allow one VLAN and be
operational.
1. Configure SW-1 with VLAN 15 and label it exactly as OPS
2. Configure SW-2 with VLAN 66 and label it exactly as ENGINEERING
3. Configure the switch port connecting to PC1
4. Configure the switch port connecting to PC2
5. Configure the E0/2 connections on SW-1 and SW-2 for neighbor discovery using the vendor-neutral standard
protocol and ensure that E0/0 on both switches uses the Cisco proprietary protocol

---

# Device Pre-configurations
## SW-1
```
hostname SW-1
!
interface Ethernet0/2
 description Trunk to R1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet0/0
 description Link to SW-2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
end
```

## SW-2
```
hostname SW-2
!
interface Ethernet0/2
 description Trunk to R2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet0/0
 description Link to SW-1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
end
```

## PC1
```
PC1> ip 172.16.15.10/24 172.16.15.1
PC1> save
```

## PC2
```
PC2> ip 192.168.66.50/24 192.168.66.1
PC2> save
```

---

# Task configurations
## SW-1
```
vlan 15
 name OPS
!
lldp run
!
interface Ethernet0/1
 description Link to PC1
 switchport mode access
 switchport access vlan 15
 no shutdown
!
interface Ethernet0/2
 no cdp enable
 lldp transmit
 lldp receive
!
interface Ethernet0/0
 cdp enable
 no lldp transmit
 no lldp receive
!
end
```

## SW-2
```
vlan 66
 name ENGINEERING
!
lldp run
!
interface Ethernet0/1
 description Link to PC2
 switchport mode access
 switchport access vlan 66
 no shutdown
!
interface Ethernet0/2
 no cdp enable
 lldp transmit
 lldp receive
!
interface Ethernet0/0
 cdp enable
 no lldp transmit
 no lldp receive
!
end
```
