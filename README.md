# Cisco-Packet-Project
Recently completed a hands on lab to set up a basic routed network with VLANs and ACLs. Here's a quick breakdown of what I did💻 :

Goals:
- Build a small office network with two departments (IT + HR).
- Use VLANs to separate traffic.
- Apply an ACL so HR cannot access IT’s server, but IT can access HR’s resources.

1) Built Topology Setup:
1x Router - 2911
1x Switch - 2960
4x PCs - 2 in HR 2 in IT
1x Server - IT VLAN

Connections:
- Router(gigabit0/0) - switch(gigabit0/1) - straight through
- PCs (1,2,3 &4) - switch (fast ethernet0 -> 0/1,0/2,0/3,0/4) - straight through
- Server - switch(fast ethernet0 -> 0/5) - straight through

2) Assigning (Configurating) VLANS
- F0/1 + F0/2 = VLAN 10 (HR)
- F0/3 + F0/4 + F0/5 = VLAN 20 (IT)

3) Configure Router for VLAN Routing
Router G0/0.20 is the gateway for IT PCs/Server (192.168.20.1) subnet - (255.255.255.0)
Router G0/0.10 is the gateway for HR PCs (192.168.10.1)

I ran into a few problems as the router gateway number for both g0/0.10 and g0/0.20 was given the same IP address, many tries later the problem was fixed by: exit the terminal and re-do the HR & IT gateways as they were merged together by mistake.

4) Assign IP addresses to pc/server

HR VLAN 10
- Pc 1 (192.168.10.2 /24) gateway (192.168.10.1)
- Pc 2 (192.168.10.3 /24) gateway (192.168.10.1)

IT VLAN 20
- Pc 3 (192.168.20.2 /24) gateway (192.168.20.1)
- Pc 4 (192.168.20.3 /24) gateway (192.168.20.1)
- Server (192.168.20.10 /24) gateway (192.168.20.1)

5) Configure ACL on router
Block HR server from having access to IT server

access-list 100 deny ip 192.168.10.0 0.0.0.255 host 192.168.20.10
access-list 100 permit ip any any

This lab helped me deepen my understanding of VLAN segmentation, inter-VLAN routing, and implementing basic network security through ACLs. Facing and solving configuration issues along the way was a great reminder that troubleshooting is a key part of real-world networking. Every mistake turned into a learning opportunity.
