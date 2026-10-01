# Private subnet for Home Lab 

I have been reading a lot of literature and content on home labs and self-hosting. I find that it would make for a fantastic environment to try tools and applications. To break things, and then fix them. A sandbox for me to learn and experiment as much as I want.

However, I also want to do things properly and, more importantly, safely. Thus, I saw it as appropriate to procure some hardware, read some manuals, watch some tutorials, and configure my own private sub-net within my home network. For this, I have set up a network in a "router on a stick" topology. This will serve as my starting point.

The ISP's Broadband Gateway connects to a tp-link Omada Gigabit VPN Gateway (ER605). Then, an 8-port Gigabit switch is connected to this router, allowing for complete control of the sub-net infrastructure, while also having ample space to add devices in the near future.

## Private Sub-net ("Router on a stick" topology")

<img width="523" height="655" alt="image" src="https://github.com/user-attachments/assets/76f4223c-01ab-4a8e-a80e-e61e2ea2a35d" />

Just to get things started, I sought to set up a VLAN for the mini PC, which will behave as a server running Proxmox Virtual Environment, which will allow me to run a massive variety of containers (Docker, LXC, etc.) and virtual machines. 
