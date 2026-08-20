# Lab-13
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="400" alt="Local Account, Named ACL, and Security" src="Lab-13 Topology.png" />
</p>

## Tasks:
All physical cabling is in place and verified. Router R1 is configured and passing traffic for VLANs 5 and 6. All
relevant ports are pre-configured as 802.1q trunks.
1. Configure SW-1 port E0/0 to permit only VLANS 5 and 6
2. Configure both SW-1 and SW-2's E0/1 ports to send and receive untagged traffic over VLAN 77
3. Configure SW-2 E0/2 port to permit only VLAN 6
4. Configure both SW-3 and SW-4 ports e0/0 and e0/1 for link aggregation using the industry standard protocol
with the following requirements:
- SW-3 ports must immediately negotiate the aggregation protocol
- SW-4 ports must not initiate the negotiation for the aggregation protocol
- Use the designated number assignment

---

# Device pre-configurations
## SW-1, SW-2, SW-3, and SW-4
```
vlan 5
 name VLAN_5
!
vlan 6
 name VLAN_6
!
vlan 77
 name Native_VLAN_77
exit
```

---

# Task configurations
## SW-1
```
interface e0/0
 switchport trunk allowed vlan 5,6
!
interface e0/1
 switchport trunk native vlan 77
```

## SW-2
```
interface e0/1
 switchport trunk native vlan 77
!
interface e0/2
 switchport trunk allowed vlan 6
```

## SW-3
```
interface range e0/0-1
 channel-group 34 mode active
!
interface port-channel 34
 switchport mode trunk
```

## SW-4
```
interface range e0/0-1
 channel-group 34 mode passive
!
interface port-channel 34
 switchport mode trunk
```

---

# Verification
## Trunking and Native VLAN Verification
```
show interfaces trunk

Verify that VLAN 77 is listed as the native VLAN on the trunks and that the allowed VLAN lists on
SW-1 and SW-2 match your configurations.
```

## LACP EtherChannel Verification
```
show etherchannel summary

Verify that Port-Channel 34 is in use (SU) and both member ports (E0/0 and E0/1) are bundled (P).
```
