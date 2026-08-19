# Lab-10
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="400" alt="Local Account, Named ACL, and Security" src="Lab-10 Topology.png" />
</p>

## Tasks:
VLANS 35 and 45 have been configured in all three switches. All physical connectivity has been installed and
verified All inter-switch links must be operational.
1. Configure SW-1 and SW-2 switch ports e0/0 and e0/1 for 802.1q trunking allowing all VLANS
2. Configure the inter-switch links on SW-1 e0/2, SW-2 e0/2, and SW-3 e0/0 and e0/1 to use native VLAN 35
3. Configure SW-1 and SW-2 switch ports e0/0 and e0/1 for link aggregation SW-1 should immediately negotiate
LACP and SW-2 must only respond to LACP requests.

---

# Device pre-configurations
## SW-1, SW-2, SW-3
```
vlan 35
 name VLAN_35
!
vlan 45
 name VLAN_45
exit
```

---

# Task configurations
## SW-1
```

interface e0/0
 channel-group 12 mode active
!
interface e0/1
 channel-group 12 mode active
!
interface port-channel 12
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 35
!
interface e0/2
 switchport trunk native vlan 35
```

## SW-2
```
interface e0/0
 channel-group 12 mode passive
!
interface e0/1
 channel-group 12 mode passive
!
interface port-channel 12
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 35
!
interface e0/2
 switchport trunk native vlan 35
```

## SW-3
```
interface e0/0
 switchport trunk native vlan 35
!
interface e0/1
 switchport trunk native vlan 35
```

---

# Verification
## Verify EtherChannel Summary
```
show etherchannel summary

Expected status: Port-Channel Po12 should show flags (SU) (Layer 2, In Use) with ports e0/0
and e0/1 flagged as (P) (bundled in port-channel).
```

## Verify Trunk Port and Native VLAN Status
```
show interfaces trunk

Expected status: Po12 and e0/2 should be shown as operational trunks, with Native VLAN 35
explicitly assigned.
```
