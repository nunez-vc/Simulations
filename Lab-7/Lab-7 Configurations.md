# Lab-7
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="600" alt="Local Account, Named ACL, and Security" src="Lab-7 Topology.png" />
</p>

## Tasks:
All physical cabling is in place. A company plans to deploy 16 new sites. The sites will utilize both IPv4 and IPv6 networks.
1. Subnet 172.16.0.0/16 to meet the subnet requirements and maximize the number of hosts using the second subnet:
- Assign the first usable IP address to e0/0 on Sw101
- Assign the last usable IP address to e0/0 on Sw102
2. Subnet 2001:DB8::/50 to meet the subnet requirements and maximize the number of hosts using the second subnet:
- Assign an IPv6 GUA using a unique 64-Bit interface identifier on e0/0 on Sw101
- Assign an IPv6 GUA using a unique 64-Bit interface identifier on e0/0 on Sw102

---

# Device Pre-configurations
## SW101
```
hostname Sw101
!
ipv6 unicast-routing
!
interface Ethernet0/0
 description Link to Sw102
 no switchport
 no shutdown
!
end
```

## SW102
```
hostname Sw102
!
ipv6 unicast-routing
!
interface Ethernet0/0
 description Link to Sw101
 no switchport
 no shutdown
!
end
```

---

# Task configurations
## SW101
```
interface Ethernet0/0
 ip address 172.25.32.1 255.255.248.0
 ipv6 address 2001:db8:0:20::1/69
!
end
```

## SW102
```
interface Ethernet0/0
 ip address 172.25.39.254 255.255.248.0
 ipv6 address 2001:db8:0:27:ffff:ffff:ffff:fffe/69
!
end
```
