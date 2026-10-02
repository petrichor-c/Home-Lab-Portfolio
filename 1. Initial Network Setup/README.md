# Private Subnet for Home Lab 

I have been reading a lot of literature and watching content on home labs and self-hosting. I find that it would make for a fantastic environment to try tools and applications. To break things, and then fix them. A sandbox for me to learn and experiment as much as I want.

However, I also want to do things properly and, more importantly, safely. Thus, I saw it as appropriate to procure some hardware, read some manuals, watch some tutorials, and configure my own private sub-net within my home network. For this, I have set up a network in a "router on a stick" topology. This will serve as my starting point.

The ISP's Broadband Gateway connects to a tp-link Omada Gigabit VPN Gateway (ER605). Then, an 8-port Gigabit switch is connected to this router, allowing for complete control of the sub-net infrastructure, isolation from the home network provided by the ISP gateway, and having ample space to add more devices in the near future. I also procured a wireless access point, for future projects :)

<img width="4032" height="3024" alt="IMG_5708" src="https://github.com/user-attachments/assets/889aaffa-cb74-46dd-8fa4-71995a052bf1" />
<img width="4032" height="3024" alt="IMG_5706" src="https://github.com/user-attachments/assets/42cbbc23-8427-4885-aed5-f4214bff04f6" />


Just to get things started, I sought to set up a sort of sandbox VLAN, isolated from the rest of the network. This will allow me to setup and test the initial home lab environment, without having to worry on how my overall network may be affected. The VLAN will have space for up to 20 devices, for now. To start, only two hosts will be present: the mini PC acting as a server running Proxmox Virtual Environment, and my main laptop. 

The switch will be configured such that ports 1 and 2 belong to the VLAN, while port 8 is set as the trunk port.
<img width="1006" height="751" alt="switch_vlan_ports" src="https://github.com/user-attachments/assets/33ed184c-85ed-44bd-8834-8f1478c9c649" />


## Initial Private Subnet Configuration ("Router on a stick" topology)

<img width="523" height="655" alt="image" src="https://github.com/user-attachments/assets/76f4223c-01ab-4a8e-a80e-e61e2ea2a35d" />  


Since all the hardware is set in place, now it is just a matter of configuring the VLAN. 

First, access the router's management portal and create the new LAN, with VLAN ID = 10. The VLAN will be isolated from the rest of the network, and the DHCP range will be set to accommodate up to 20 devices, for now. 
Thus, we end with a LAN with the following properties:
- Default Gateway: 192.168.10.1
- IP Address Range: 192.168.10.2 - 192.168.10.21
- Subnet Mask: 255.255.255.0
<img width="958" height="473" alt="router_VLAN" src="https://github.com/user-attachments/assets/4d2c5c59-770c-4487-8dd4-4dd0a8f64e4c" />

Furthermore, need to configure the tags on the ports, as port 5 on the router will act as the uplink for port 8 of the switch (trunk port).  
<img width="1917" height="942" alt="image" src="https://github.com/user-attachments/assets/c52e283c-2c3a-485d-bc96-db42bf681a0b" />  


I have to highlight that the initial Proxmox configuration asks that the host is assigned a static IP address. Therefore, I have reserved the address 192.168.10.3 for the mini PC running Proxmox VE.  

<img width="959" height="470" alt="reserved_IP_VLAN" src="https://github.com/user-attachments/assets/e26a7f98-5218-4d82-8e96-a0a7791f7169" />

Used the console to double-check and ensure that not only do the assigned ports assigned correct IP addresses for VLAN 10, but that it is also isolated from the rest of the network:

<img width="673" height="384" alt="checking_ip_VLAN" src="https://github.com/user-attachments/assets/e4846715-765d-46c8-acc1-7d4c64de7186" />


<img width="668" height="383" alt="checking_isolated_VLAN" src="https://github.com/user-attachments/assets/bb475017-802a-4697-bdd3-c85b58878916" />


