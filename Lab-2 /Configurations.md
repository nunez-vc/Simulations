Tasks
Refer to the topology. All physical cabling is in place. Configure local users accounts, modify the Named ACL
(NACL), and configure DHCP Snooping. The current contents of the NACL must remain intact.
Task 1
Configure a local account on Gw1 with telnet access only on virtual ports 0-4. Use the following information:
• Username: wheel
• Password: lock3path
• Algorithm type: Scrypt
• Privilege level: Exec mode
Task 2
Configure and apply a NACL on Gw1 to control network traffic from VLAN 10:
• Name: CORP_ACL
• Allow BOOTP and HTTPS
• Restrict all other traffic and log the ingress interface, source MAC address, the packetʼs source and destination IP
addresses, and ports
Task 3
Configure Sw1:
• Enable DNCP Snooping for VLAN 10
• Disable DHCP Option-82 data insertion
• Enable DHCP Snooping MAC address verification
• Enable trusted interfaces
