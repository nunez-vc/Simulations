# Lab-5
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="300" alt="Local Account, Named ACL, and Security" src="Lab-5 Topology.png" />
</p>

## Tasks:
R1 has been pre-configured with all the necessary commands. All physical cabling is in place and verified.
Connectivity from PC1, PC3, and the Server must be established to the switches, and each port must only allow one VLAN.
1. Configure the VLAN connecting to the switch port for PC3 with the name "SALES"
2. Configure the switch port connecting to Server1
3. Configure the switch port connecting to PC3
4. Ensure R1 discovers SW-1 via the Cisco proprietary neighbor discovery protocol and all other devices on the network are unable to discover SW-1.

---

# Device Pre-configurations
## SW1
```
hostname SW1
!
interface Ethernet0/0
 description Trunk to R1
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
interface Ethernet0/2
 description Link to PC1
 switchport mode access
!
end
write memory
copy running-config startup-config
```

## SW2
```
hostname SW2
!
interface Ethernet0/0
 description Trunk to R1
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
copy running-config  startup-config
```

## PC1 and PC3
```
PC1> ip 10.10.1.10/24 10.10.1.1
PC1> save
PC3> ip 10.30.1.10/24 10.30.1.1
PC3> save
```

## WinServer2022
```
On Powershell as administrator,
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 10.20.1.10 -PrefixLength 24 -DefaultGateway 10.20.1.1
```

---

# Task Configurations
## SW2
```
vlan 20
 name SERVER
!
vlan 30
 name SALES
!
interface Ethernet0/2
 description Link to Server1 (WinServer2022)
 switchport mode access
 switchport access vlan 20
 no shutdown
!
interface Ethernet0/3
 description Link to PC3
 switchport mode access
 switchport access vlan 30
 no shutdown
!
end
```

## SW1
```
cdp run
!
interface Ethernet0/0
 cdp enable
!
interface Ethernet0/1
 no cdp enable
!
interface Ethernet0/2
 no cdp enable
!
end
```
