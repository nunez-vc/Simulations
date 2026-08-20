# Lab-14
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="500" alt="Local Account, Named ACL, and Security" src="Lab-14 Topology.png" />
</p>

## Tasks
Refer to the topology. All physical cabling is in place. Configure local users accounts, modify the Named ACL
(NACL), and configure DHCP Snooping. The current contents of the NACL must remain intact.
1. Configure a local account on Sw103 with telnet access only on virtual ports 0-4. Use the following information:
- Username: devnet
- Password: access8cli
- Algorithm type: SHA256
- Privilege level: Exec mode
2. Using the minimum number of ACEs, modify the existing NACL "INTERNET_ACL" to control network traffic
destined for the Internet, and apply the ACL on R1:
- Allow HTTPS from 172.16.0.0/16
- Allow Telnet only for VLAN 101
- Restrict all other traffic and log the ingress interface, source MAC address, the packet's source and destination IP
addresses, and ports
3. Configure Sw101:
- Enable DHCP Snooping for VLAN 101
- Disable DHCP Option-82 data insertion
- Enable DHCP Snooping MAC address verification

---

# Device pre-configurations
## R1
```
no ip domain-lookup
!
! Interface e0/1 connects to the ISP/Internet
interface e0/1
 description Link to INTERNET (Public WAN)
 ip address 209.165.201.2 255.255.255.252
 no shutdown
!
! Interface e0/0 connects to multilayer switch SW103
interface e0/0
 description Link to SW103
 ip address 172.16.1.17 255.255.255.252
 no shutdown
!
! Interface e0/3 connects to multilayer switch SW104
interface e0/3
 description Link to SW104
 ip address 172.16.1.21 255.255.255.252
 no shutdown
end
```

## SW101
```
no ip domain-lookup
!
ip routing
! Create VLANs
vlan 101
 name VLAN_101
exit
!
! Physical Uplink to SW103 (Routed Port)
interface e0/2
 no switchport
 description Uplink to SW103
 ip address 172.16.1.6 255.255.255.252
 no shutdown
!
! Physical Link to SW102 (Routed Port)
interface e0/1
 no switchport
 description Link to SW102
 ip address 172.16.1.1 255.255.255.252
 no shutdown
!
! Access Port facing PC1 (VLAN 101)
interface e0/0
 description Link to PC1 (VLAN 101 Access Port)
 switchport mode access
 switchport access vlan 101
 no shutdown
!
! SVI Gateway for PC1 Subnet
interface Vlan 101
 description Gateway for PC1 (VLAN 101)
 ip address 172.16.101.1 255.255.255.0
 no shutdown
end
```

## SW102
```
no ip domain-lookup
!
ip routing
! Create VLANs
vlan 102
 name VLAN_102
exit
!
! Physical Uplink to SW104 (Routed Port)
interface e0/2
 no switchport
 description Uplink to SW104
 ip address 172.16.1.10 255.255.255.252
 no shutdown
!
! Physical Link to SW101 (Routed Port)
interface e0/1
 no switchport
 description Link to SW101
 ip address 172.16.1.2 255.255.255.252
 no shutdown
!
! Access Port facing PC2 (VLAN 102 Access Port)
interface e0/0
 description Link to PC2 (VLAN 102 Access Port)
 switchport mode access
 switchport access vlan 102
 no shutdown
!
! SVI Gateway for PC2 Subnet
interface Vlan 102
 description Gateway for PC2 (VLAN 102)
 ip address 172.16.102.1 255.255.255.0
 no shutdown
end
```

## SW103
```
no ip domain-lookup
!
ip routing
!
! Interface e0/1 connects SW103 back to Router R1
interface e0/1
 no switchport
 description Uplink to R1
 ip address 172.16.1.18 255.255.255.252
 no shutdown
!
! Interface e0/0 connects SW103 to SW104
interface e0/0
 no switchport
 description Link to SW104
 ip address 172.16.1.13 255.255.255.252
 no shutdown
!
! Interface e0/2 connects SW103 down to access switch SW101
interface e0/2
 no switchport
 description Link to SW101
 ip address 172.16.1.5 255.255.255.252
 no shutdown
end
```

## SW104
```
no ip domain-lookup
!
ip routing
!
! Interface e0/3 connects SW104 back to Router R1
interface e0/3
 no switchport
 description Uplink to R1
 ip address 172.16.1.22 255.255.255.252
 no shutdown
!
! Interface e0/0 connects SW104 to SW103
interface e0/0
 no switchport
 description Link to SW103
 ip address 172.16.1.14 255.255.255.252
 no shutdown
!
! Interface e0/2 connects SW104 down to access switch SW102
interface e0/2
 no switchport
 description Link to SW102
 ip address 172.16.1.9 255.255.255.252
 no shutdown
end
```

---

# Task configurations
## SW103
```
username devnet privilege 15 algorithm-type sha256 secret access8cli
!
line vty 0 4
login local
transport input telnet
end
```

## R1
```
ip access-list extended INTERNET_ACL
permit tcp 172.16.0.0 0.0.255.255 any eq 443
permit tcp 172.16.101.0 0.0.0.255 any eq telnet
deny ip any any log
end
```

## SW101
```
ip dhcp snooping vlan 101
no ip dhcp snooping information option
ip dhcp snooping verify mac-address
end
```
