# Lab-1
Refer to the topology. All physical cabling is in place. Configure a local user account, a Named ACL (NACL), and security.

<p align="center">
  <img width="300" alt="Local Account, Named ACL, and Security" src="Lab-1 Topology.png" />
</p>

## Task 1
```
Configure a local account on SW1 with telnet access only on virtual ports 0-4. Use the following information:
• Username: support
• Password: max2learn
• Privilege level: Exec mode
```

## Task 2
```
Configure and apply a single NACL on SW1 using the following:
• Name: ENT_ACL
• Restrict only PC2 on VLAN 200 from pinging PC1 
• Allow only PC2 on VLAN 200 to telnet to Sw101 
• Prevent all other devices from telnetting from VLAN 200
• Allow all other network traffic from VLAN 200
```

## Task 3
```
Configure security on interface Ethernet 0/0 of SW2:
• Set the maximum number of secure MAC addresses to four.
• Drop packets with unknown source addresses until the number of secure MAC addresses drops below the
configured maximum value. No notification action is required.
• Allow secure MAC addresses to be learned dynamically
```

# Device Pre-configs: Local Account, NACL, Port Security Lab
---

## R1
```
hostname R1
!
interface Ethernet0/0
 description Link to Sw101
 ip address 192.168.3.1 255.255.255.252
 no shutdown
!
interface Ethernet0/1
 description Link to INTERNET
 ip address 209.165.201.2 255.255.255.252
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 209.165.201.1
ip route 192.168.100.0 255.255.255.0 192.168.3.2
ip route 192.168.200.0 255.255.255.0 192.168.3.2
!
end
write memory
copy running-config startup-config
```
## SW1
```
hostname SW1
!
ip routing
!
vlan 100
!
vlan 200
!
interface Ethernet0/0
 description Link to PC1
 switchport mode access
 switchport access vlan 100
 no shutdown
!
interface Ethernet0/1
 description Link to R1
 no switchport
 ip address 192.168.3.2 255.255.255.252
 no shutdown
!
interface Ethernet0/2
 description Trunk to SW2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 no shutdown
!
interface Vlan100
 ip address 192.168.100.1 255.255.255.0
 no shutdown
!
interface Vlan200
 ip address 192.168.200.1 255.255.255.0
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 192.168.3.1
!
end
write memory
copy running-config startup-config
```

## SW2
```
hostname SW2
!
vlan 200
!
interface Ethernet0/0
 description Link to PC2
 switchport mode access
 switchport access vlan 200
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
## PC1 / PC2 (VPCS)
```
PC1> ip 192.168.100.10/24 192.168.100.1
PC1> save

PC2> ip 192.168.200.10/24 192.168.200.1
PC2> save
```

---

## Task 1: Local Account
### SW1
```
username support privilege 1 secret max2learn
!
line vty 0 4
 login local
 transport input telnet
!
```

## Task 2: Named ACL
### SW1
```
ip access-list extended ENT_ACL
 deny icmp host 192.168.200.10 host 192.168.100.10 echo
 permit tcp host 192.168.200.10 host 192.168.200.1 eq 23
 deny tcp 192.168.200.0 0.0.0.255 host 192.168.200.1 eq 23
 permit ip any any
!
interface Vlan200
 ip access-group ENT_ACL in
!
end
write memory
copy running-config startup-config
```

## Task 3: Port Security on PC Port 2
### SW2 
```
interface Ethernet0/0
 switchport port-security
 switchport port-security maximum 4
 switchport port-security violation protect
!
end
write memory
copy running-config startup-config
```



