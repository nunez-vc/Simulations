# Lab-6
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="400" alt="Local Account, Named ACL, and Security" src="Lab-6 Topology.png" />
</p>

## Tasks:

All physical cabling is in place. Routers R3 and R4 are fully configured and inaccessible. Configure static routes for
various connectivity to the ISP and the LAN that resides on R4.
1. Configure a route on R1 to ensure that R1 prefers R2 when traffic is destined to the server only.
2. Configure a default route on R2 to the ISP
3. Configure a route on R1 to ensure that R1 will use R2 for the R4 LAN if the link fails between R3 and R4
4. Configure a route on R1 to ensure that R1 prefers R3 when traffic is destined to the R4 LAN at 10.0.41.0/24

---

# Device Pre-configurations
## R1
```
hostname R1
!
interface Ethernet0/0
 description Link to R2
 ip address 10.0.12.1 255.255.255.252
 no shutdown
!
interface Ethernet0/1
 description Link to R3
 ip address 10.0.13.1 255.255.255.224
 no shutdown
!
end
```

## R2
```
hostname R2
!
interface Ethernet0/0
 description Link to R1
 ip address 10.0.12.2 255.255.255.252
 no shutdown
!
interface Ethernet0/1
 description Link to R4
 ip address 10.0.24.1 255.255.255.248
 no shutdown
!
interface Ethernet0/2
 description Link to ISP
 ip address 209.165.200.226 255.255.255.224
 no shutdown
!
end
```

## R3
```
hostname R3
!
interface Ethernet0/0
 description Link to R1
 ip address 10.0.13.2 255.255.255.224
 no shutdown
!
interface Ethernet0/1
 description Link to R4
 ip address 10.0.34.1 255.255.255.240
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 10.0.13.1
ip route 10.0.41.0 255.255.255.0 10.0.34.2
!
end
```

## R4
```
hostname R4
!
interface Ethernet0/0
 description Link to R2
 ip address 10.0.24.2 255.255.255.248
 no shutdown
!
interface Ethernet0/1
 description Link to R3
 ip address 10.0.34.2 255.255.255.240
 no shutdown
!
interface Ethernet0/2
 description Link to Server
 ip address 10.0.41.1 255.255.255.0
 no shutdown
!
ip route 0.0.0.0 0.0.0.0 10.0.34.1
ip route 0.0.0.0 0.0.0.0 10.0.24.1 200
!
end
```

## ISP
```
hostname ISP
!
interface Ethernet0/0
 description Link to R2
 ip address 209.165.200.225 255.255.255.224
 no shutdown
!
ip route 10.0.0.0 255.0.0.0 209.165.200.226
!
end
```

---


# Task Configurations
## R1 
```
ip route 10.0.41.10 255.255.255.255 10.0.12.2      --> Task 1
ip route 10.0.41.0 255.255.255.0 10.0.12.2 200     --> Task 3
ip route 10.0.41.0 255.255.255.0 10.0.13.2         --> Task 4
```

## R2
```
ip route 0.0.0.0 0.0.0.0 209.165.200.225           --> Task 2
```

---

# Verification
## 1. Confirm interfaces are up
```
On every router: show ip interface brief
Note: All configured interfaces should read up / up.
```

## 2. Confirm R1's routing table has all three routes
```
show ip route static

Expected result:
S    10.0.41.10/32 [1/0] via 10.0.12.2
S    10.0.41.0/24 [1/0] via 10.0.13.2
S    10.0.41.0/24 [200/0] via 10.0.12.2

Note: Only the AD-1 route shows as active/installed — the AD-200 floating route exists in the static route list but won't appear as
the selected path in show ip route (no S prefix line for it) unless the primary is down.
```

## 3. Confirm R2's default route
```
show ip route static

Expected result:
S*   0.0.0.0/0 [1/0] via 209.165.200.225
```

## 4. Prove Task 1 and Task 4 are both taking effect at once
```
From R1:
traceroute 10.0.41.10
The first hop should be through R2 (10.0.12.2).
```
      
