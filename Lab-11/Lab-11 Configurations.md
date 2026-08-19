# Lab-11
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="500" alt="Local Account, Named ACL, and Security" src="Lab-11 Topology.png" />
</p>

## Tasks:
All physical cabling is in place and verified. Switch SW-1 is pre-configured and inaccessible. SW-2 and SW-3 ports
must be configured and operational to complete the configuration.
1. Configure SW-2 and SW-3 ports E0/0 to use the industry standard encapsulation method for trunking and only
tag VLAN 10.
2. Configure SW-2 and SW-3 ports E0/0 to send and receive untagged traffic over VLAN 11.
3. Configure SW-2 and SW-3 ports E0/2 and E0/3 to use the industry standard encapsulation method for trunking
and tag all VLANS.
4. Configure SW-2 and SW-3 ports E0/2 and E0/3 for link aggregation using the industry standard protocol with the
following requirements:
- SW-2 ports must not initiate the negotiation for the aggregation protocol
- SW-3 ports must immediately negotiate the aggregation protocol
- Use the designated number assignment

---

# Device pre-configurations
## SW-1, SW-2 and SW-3
```
vlan 10
 name VLAN_10
vlan 11
 name VLAN_11
exit
```

---

# Task configurations
## SW-2
```
interface e0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport access vlan 10
 switchport trunk native vlan 11
!
interface e0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 23 mode passive
!
interface e0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 23 mode passive
!
interface port-channel 23
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

## SW-3
```
interface e0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport access vlan 10
 switchport trunk native vlan 11
!
interface e0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 23 mode active
!
interface e0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 23 mode active
!
interface port-channel 23
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

---

# Verifications
## Verify LACP Bundling
```
show etherchannel summary

Expectation: Both e0/2 and e0/3 should show a flag of (P) (bundled in port-channel)
under Po23 and the port-channel state should be (RU) or (SU)
```

## Verify Trunking & Native VLAN
```
show interfaces trunk

Expectation: e0/0 and Po23 should be listed as active trunks, with e0/0 showing
Native VLAN 11 and Po23 showing Native VLAN 1.
```
