# Ex. No: 8   Packet Tracer: Connect a Router to a LAN
# Date: 11/02/2026
________________________________________<br>
# Objective
Configure and verify router LAN/WAN interfaces in Cisco Packet Tracer and test end-to-end connectivity.<br>
•	Display key router information (interfaces, status, routing table).<br>
•	Configure IPv4 addresses and descriptions on R1 and R2 interfaces.<br>
•	Bring interfaces up and verify with show commands and pings. <br>
________________________________________<br>
# Apparatus / Tools Required
•	Cisco Packet Tracer<br>
•	2 Routers (R1, R2 — 2911 or equivalent)<br>
•	2 Switches (S1, S2)<br>
•	4 PCs (PC1–PC4) with NICs<br>
•	Copper straight-through cables for LAN links; Serial DCE/DTE cable for WAN link <br>
________________________________________<br>
# Network Topology Diagram

<img width="803" height="476" alt="image" src="https://github.com/user-attachments/assets/8fdfad57-2046-4f51-bf89-e1c2dad84a30" />

<br>



Device	Interface	IP Address	Subnet Mask	Default Gateway<br>
R1	G0/0	192.168.10.1	255.255.255.0	—<br>
R1	G0/1	192.168.11.1	255.255.255.0	—<br>
R1	S0/0/0 (DCE)	209.165.200.225	255.255.255.252	—<br>
R2	G0/0	10.1.1.1	255.255.255.0	—<br>
R2	G0/1	10.1.2.1	255.255.255.0	—<br>
R2	S0/0/0	209.165.200.226	255.255.255.252	—<br>
PC1	NIC	192.168.10.10	255.255.255.0	192.168.10.1<br>
PC2	NIC	192.168.11.10	255.255.255.0	192.168.11.1<br>
PC3	NIC	10.1.1.10	255.255.255.0	10.1.1.1<br>
PC4	NIC	10.1.2.10	255.255.255.0	10.1.2.1<br>
Note (per activity): Console password = cisco; Privileged EXEC password = class. <br>
<img width="639" height="369" alt="image" src="https://github.com/user-attachments/assets/40a3af25-1faa-4afd-870c-926799661dd4" />

________________________________________<br>
# Procedure
# Part 1: Display Router Information (R1 shown; repeat on R2)
1.	Click R1 → CLI. If prompted, use console password cisco, then enable and password class. <br>
2.	View interface statistics (all):<br>
3.	R1# show interfaces<br>
4.	View a specific interface (example, Serial 0/0/0):<br>
5.	R1# show interfaces serial 0/0/0<br>
o	Note IP address and bandwidth from the output. <br>
6.	View G0/0 details:<br>
7.	R1# show interfaces gigabitEthernet 0/0<br>
o	Note IP, MAC address, and BW. <br>
8.	Get a brief summary of all interfaces:<br>
9.	R1# show ip interface brief<br>
o	Count Serial and Ethernet ports, and check their status (up/down). <br>
10.	Display routing table:<br>
11.	R1# show ip route<br>
o	Identify C (connected) and any dynamic routes; note how unknown networks are handled. <br>
________________________________________<br>
# Part 2: Configure Router Interfaces
R1 – LAN Interfaces<br>
1.	Enter global config: R1# configure terminal<br>
2.	G0/0 (LAN to S1 / VLAN with PC1)<br>
3.	R1(config)# interface g0/0<br>
4.	R1(config-if)# ip address 192.168.10.1 255.255.255.0<br>
5.	R1(config-if)# description LAN connection to S1<br>
6.	R1(config-if)# no shutdown<br>
7.	G0/1 (LAN to S1 / VLAN with PC2)<br>
8.	R1(config)# interface g0/1<br>
9.	R1(config-if)# ip address 192.168.11.1 255.255.255.0<br>
10.	R1(config-if)# description LAN connection to S1 (PC2 VLAN)<br>
11.	R1(config-if)# no shutdown<br>
R1 – WAN Serial (to R2)<br>
R1(config)# interface s0/0/0<br>
R1(config-if)# ip address 209.165.200.225 255.255.255.252<br>
R1(config-if)# clock rate 64000   ← (DCE end)<br>
R1(config-if)# description WAN link to R2<br>
R1(config-if)# no shutdown<br>
(Use show controllers serial 0/0/0 if you need to confirm DCE/DTE in PT.) <br>
R2 – LAN Interfaces<br>
R2# conf t<br>
R2(config)# interface g0/0<br>
R2(config-if)# ip address 10.1.1.1 255.255.255.0<br>
R2(config-if)# description LAN connection to S2 (PC3)<br>
R2(config-if)# no shutdown<br>
R2(config)# interface g0/1<br>
R2(config-if)# ip address 10.1.2.1 255.255.255.0<br>
R2(config-if)# description LAN connection to S2 (PC4)<br>
R2(config-if)# no shutdown<br>
R2 – WAN Serial (to R1)<br>
R2(config)# interface s0/0/0<br>
R2(config-if)# ip address 209.165.200.226 255.255.255.252<br>
R2(config-if)# description WAN link to R1<br>
R2(config-if)# no shutdown<br>
Save to NVRAM (both routers)<br>
R1# copy running-config startup-config<br>
R2# copy running-config startup-config<br>
(Short form allowed: wr.) <br>
________________________________________<br>
# Part 3: Verification & Testing<br>
1.	Quick interface check (both routers)<br>
2.	R1# show ip interface brief<br>
3.	R2# show ip interface brief<br>
o	Ensure all configured ports show correct IPs and Status/Protocol: up/up. <br>
4.	Routing table review<br>
5.	R1# show ip route<br>
6.	R2# show ip route<br>
o	Count C (connected) and any dynamic routes; total should align with the number of LANs/WANs in the topology. <br>
7.	End-to-end pings (examples from the activity):<br>
o	From PC1, ping PC4.<br>
o	From R2, ping PC2.<br>
o	You should also be able to ping all active router interfaces from the PCs. (Switches are not configured for management; you won’t ping them.) <br>
________________________________________<br>
# Commands Used (summary)
•	Mode/navigation: enable, configure terminal, end<br>
•	Interface config: interface g0/0 | g0/1 | s0/0/0, ip address A.B.C.D M.M.M.M, description ..., no shutdown<br>
•	DCE timing (if applicable): clock rate 64000<br>
•	Show/verify: show interfaces, show interfaces serial 0/0/0, show interfaces g0/0, show ip interface brief, show ip route<br>
•	Save: copy running-config startup-config / wr <br>
________________________________________<br>
# Output (Attach Screenshots)

<img width="1918" height="1016" alt="image" src="https://github.com/user-attachments/assets/bdd59d01-ef8d-4001-94fe-bf2e00fc2ab0" />


•	show ip interface brief and ip route on R1 (after configuration) <br>

<img width="921" height="729" alt="image" src="https://github.com/user-attachments/assets/a796e155-93bf-4208-9b49-a449221cc79a" />


•	show ip interface brief and ip route on R2 (after configuration)<br>

<img width="1891" height="675" alt="image" src="https://github.com/user-attachments/assets/ca79a6b8-1399-40af-bd26-6fae057b83de" />

•	Successful ping PC1 → PC4<br>
<img width="684" height="678" alt="image" src="https://github.com/user-attachments/assets/8779a15c-9adf-4818-8270-cdcb3e442f19" />

•	Successful ping R2 → PC2<br>

<img width="687" height="669" alt="image" src="https://github.com/user-attachments/assets/cb79cbb1-bff4-4705-a407-8cdfaf4443fb" />



•	Interface up messages after no shutdown on each link <br>
•No shutdown on R1:

<img width="628" height="121" alt="image" src="https://github.com/user-attachments/assets/54e704a6-9394-4235-9c70-d0ae2f383db4" />

•No shutdown on R2:
<img width="819" height="110" alt="image" src="https://github.com/user-attachments/assets/387c0fe7-56fa-4d45-a789-01cf5b7bb6c5" />

________________________________________<br>
# Result
R1 and R2 were configured with correct IPv4 addresses and interface descriptions, links were brought up, routing tables showed connected networks, and end-to-end connectivity between PCs across the WAN link was verified using pings. The configurations were saved to NVRAM for persistence.<br>

