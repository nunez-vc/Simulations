# Lab-12
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="500" alt="Local Account, Named ACL, and Security" src="Lab-12 Topology.png" />
</p>

## Tasks:
All physical cabling is in place. A company plans to deploy 16 new sites. The sites will utilize both IPv4 and IPv6
networks.

Subnet 10.20.0.0/16 to meet the subnet requirements and maximize the number of hosts using the second subnet.
- Assign the first usable IP address to e0/0 on Sw101
- Assign the last usable IP address to e0/0 on Sw102

Subnet 2001:db8::/52 to meet the subnet requirements and maximize the number of hosts.
- Assign an IPv6 GUA using a unique 64-Bit interface identifier on e0/0 on Sw101
- Assign an IPv6 GUA using a unique 64-Bit interface identifier on e0/0 on Sw102

---

# Device pre-configurations
## SW-101 and SW-102
```
configure terminal
ipv6 unicast-routing
exit
```

---

# Task configurations
## SW-101
```
interface g1/0/1
 no switchport
 ip address 172.16.16.1 255.255.240.0
 ipv6 address 2001:db8:0:400::/54 eui-64
 no shutdown
end
```

## SW-102
```
interface g1/0/1
 no switchport
 ip address 172.16.31.254 255.255.240.0
 ipv6 address 2001:db8:0:400::/54 eui-64
 no shutdown
end
```
