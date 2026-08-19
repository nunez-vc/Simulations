# Lab-9
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="400" alt="Local Account, Named ACL, and Security" src="Lab-9 Topology.png" />
</p>

## Tasks:
R1 has been pre-configured with all the necessary commands. All physical cabling is in place and verified.
Connectivity for PC1 and PC2 must be established to the switches, and each port must only allow one VLAN.
1. Configure SW-1 with VLAN 35 and label it exactly as SALES
2. Configure SW-2 with VLAN 39 and label it exactly as MARKETING
3. Configure the switch port connecting to PC1
4. Configure the switch port connecting to PC2
5. Configure SW-1 and SW-2 for universal neighbor discovery using the industry standard protocol and disable it on
the interface connecting to PC1

---

# Device pre-configurations
## SW-1
```
hostname SW-1
!
interface Ethernet0/0
 description Trunk to R1
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
interface Ethernet0/0
 description Trunk to R1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
end
```

## PC1
```
PC1> ip 10.35.1.99/24 10.35.1.1
PC1> save
```

## PC2
```
PC2> ip 10.39.1.99/24 10.39.1.1
PC2> save
```

---

# Task configurations
## SW-1
```
vlan 35
 name SALES
!
lldp run
!
interface Ethernet0/2
 description Link to PC1
 switchport mode access
 switchport access vlan 35
 no lldp transmit
 no lldp receive
 no shutdown
!
end
```

## SW-2
```
vlan 39
 name MARKETING
!
lldp run
!
interface Ethernet0/2
 description Link to PC2
 switchport mode access
 switchport access vlan 39
 no shutdown
!
end
```
