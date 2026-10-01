# Private Subnet for Home Lab 

I have been reading a lot of literature and content on home labs and self-hosting. I find that it would make for a fantastic environment to try tools and applications. To break things, and then fix them. A sandbox for me to learn and experiment as much as I want.

However, I also want to do things properly and, more importantly, safely. Thus, I saw it as appropriate to procure some hardware, read some manuals, watch some tutorials, and configure my own private sub-net within my home network. For this, I have set up a network in a "router on a stick" topology. This will serve as my starting point.

The ISP's Broadband Gateway connects to a tp-link Omada Gigabit VPN Gateway (ER605). Then, an 8-port Gigabit switch is connected to this router, allowing for complete control of the sub-net infrastructure, while also having ample space to add devices in the near future.

Just to get things started, I sought to set up a sort of sandbox VLAN, isolated from the rest of the network. This will allow me to setup and test the initial home lab environment, without having to worry on how my overall network may be affected (at least, not yet). The VLAN will have space for up to 10 devices, for now. To start, only two hosts will be present: the mini PC acting as a server running Proxmox Virtual Environment, and my main laptop.

The VLAN has ID = 10. The address space is 192.168.10.1/24 - 192.168.10.14/24

I have to highlight that the initial Proxmox configuration asks that the host is assigned a static IP address. Therefore, I have reserved the address 192.168.10.3 for the mini PC running Proxmox VE. 

The switch has been configured as such: ports 1 and 2 belong to VLAN 10, while port 8 is set as the trunk port.

## Initial Private Subnet Configuration ("Router on a stick" topology)

<img width="523" height="655" alt="image" src="https://github.com/user-attachments/assets/76f4223c-01ab-4a8e-a80e-e61e2ea2a35d" />

 Used the console to double-check and ensure that both ports assign correct IP addresses for the VLAN:
 
