# Lab-2
Refer to the topology. All physical cabling is in place. Configure a Local User Accounts, Named ACL (NACL), and DHCP Snooping

<p align="center">
  <img width="300" alt="Local Account, Named ACL, and Security" src="Lab-2 Topology.png" />
</p>

## Task 1
```
Configure a local account on GW1 with telnet access only on virtual ports 0-4. Use the following information:
• Username: wheel
• Password: lock3path
• Algorithm type: Scrypt
• Privilege level: Exec mode
```

## Task 2
```
Configure and apply a NACL on GW1 to control network traffic from VLAN 10:
• Name: CORP_ACL
• Allow BOOTP and HTTPS
• Restrict all other traffic and log the ingress interface, source MAC address, the packetʼs source and destination IP
addresses, and ports.
```

## Task 3
```
Configure Sw1:
• Enable DNCP Snooping for VLAN 10
• Disable DHCP Option-82 data insertion
• Enable DHCP Snooping MAC address verification
• Enable trusted interfaces
```

---

# Device Pre-configurations 
## GW1 (Router + DHCP server)
```
hostname GW1
!
interface Ethernet0/0
 description Link to SW3
 ip address 192.168.1.1 255.255.255.252
 no shutdown
!
interface Ethernet0/2
 description Link to INTERNET
 ip address 209.165.201.2 255.255.255.252
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 209.165.201.1
ip route 192.168.10.0 255.255.255.0 192.168.1.2
ip route 192.168.20.0 255.255.255.0 192.168.1.2
!
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
!
ip dhcp pool VLAN10_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
!
ip dhcp pool VLAN20_POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
!
ip access-list extended CORP_ACL
 permit ip any any
!
interface Ethernet0/0
 ip access-group CORP_ACL in
!
end
write memory
copy running-config startup-config
```

## SW3 (L3 Distribution: VLAN 10/20 SVIs, DHCP relay)
```
hostname SW3
!
ip routing
!
vlan 10
!
vlan 20
!
interface Ethernet0/0
 description Link to HOSTC
 switchport mode access
 switchport access vlan 10
 no shutdown
!
interface Ethernet0/3
 description Link to HOSTD
 switchport mode access
 switchport access vlan 20
 no shutdown
!
interface Ethernet0/2
 description Trunk to SW1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet0/1
 description Trunk to SW2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet1/0
 description Link to GW1
 no switchport
 ip address 192.168.1.2 255.255.255.252
 no shutdown
!
interface Vlan10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.1.1
 no shutdown
!
interface Vlan20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.1.1
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 192.168.1.1
!
end
write memory
copy running-config startup-config
```

## SW1 (L2 access)
```
hostname SW1
!
vlan 10
!
interface Ethernet0/0
 description Link to HOSTA
 switchport mode access
 switchport access vlan 10
 no shutdown
!
interface Ethernet0/2
 description Trunk to SW3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet0/1
 description Trunk to SW2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
end
write memory
copy running-config startup-config
```

## SW2 (L2 access)
```
hostname SW2
!
vlan 20
!
interface Ethernet0/0
 description Link to HOSTB
 switchport mode access
 switchport access vlan 20
 no shutdown
!
interface Ethernet0/2
 description Trunk to SW3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Ethernet0/1
 description Trunk to SW1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
end
write memory
copy running-config startup-config
```

## HOSTA / HOSTB / HOSTC / HOSTD (VPCS — DHCP clients)
```
HOSTA> ip dhcp
HOSTA> save
HOSTB> ip dhcp
HOSTB> save
HOSTC> ip dhcp
HOSTC> save
HOSTD> ip dhcp
HOSTD> save
```

--- 

# Task Configurations
## Task 1: Local account on GW1 with telnet access only
```
username wheel privilege 1 algorithm-type scrypt secret lock3path
!
line vty 0 4
 login local
 transport input telnet
 exec-timeout 0 0
!
```

## Task 2: Named ACL policy to control network traffic from VLAN 10 on GW1
```
ip access-list extended CORP_ACL
 permit udp 192.168.10.0 0.0.0.255 any eq bootps
 permit udp 192.168.10.0 0.0.0.255 any eq bootpc
 permit tcp 192.168.10.0 0.0.0.255 any eq 443
 deny ip 192.168.10.0 0.0.0.255 any log-input
!
end
write memory
copy running-config startup-config

Note: ACL is already applied in on Ethernet0/0 in the preconfig — no re-application needed.
```

## Task 3: DHCP Snooping on SW1
```
ip dhcp snooping
ip dhcp snooping vlan 10
no ip dhcp snooping information option
ip dhcp snooping verify mac-address
!
interface Ethernet0/2
 ip dhcp snooping trust
!
interface Ethernet0/1
 ip dhcp snooping trust
!
end
write memory
copy running-config startup config
```



