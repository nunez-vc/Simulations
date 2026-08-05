# Lab-4
Refer to the topology. All physical cabling is in place. 

<p align="center">
  <img width="300" alt="Local Account, Named ACL, and Security" src="Lab-4 Topology.png" />
</p>

--- 

# Tasks

IP connectivity between the three routers is configured. OSPF adjacencies must be established

1. Configure R1 and R2 Router IDs using the interface IP addresses from the link that is shared
between them.
2. Configure the R2 links with a max value facing R1 and R3. R2 must become the DR. R1 and R3 links facing R2 must remain with the default OSPF configuration for DR election. Verify the configuration after clearing the OSPF process.
3. Using a host wildcard mask, configure all three routers to advertise their respective Loopback1 networks.
4. Configure the link between R1 and R3 to disable their ability to add other OSPF routers

---

# Device Pre-configurations
