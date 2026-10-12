# Home Server with Proxmox VE

From Proxmox's site: 

*Proxmox Virtual Environment is an open source server virtualization management solution based on QEMU/KVM and LXC. You can manage virtual machines, containers, highly available clusters, storage and networks with an integrated, easy-to-use web interface or via CLI*

<img width="1354" height="942" alt="image" src="https://github.com/user-attachments/assets/394d3b2a-fee4-4439-9cf6-ee2fcd2e0c85" />

Proxmox VE functions like a type-1 hypervisor. It allows me to spin up containers and virtual machines, all of which have direct access to hardware resources which allows the services to run more efficiently. 

Installing Proxmox is a simple matter of flashing a USB thumb drive with the latest ISO file. In my case, this was the Proxmox VE 9.2 ISO
<img width="1918" height="946" alt="image" src="https://github.com/user-attachments/assets/2757a403-33f6-478a-ba56-e822fa4f8167" />

Following the prompts from the installer, I perform a few routine steps like setting the root user and assigning the machine's IP address. As per Proxmox's documentation, it is recommended that the host machine is given a static IP address. Thus, I reserved 192.168.10.3 from my Lab VLAN for this purpose.

Once the installation finishes, the system reboots itself into the console:
<img width="1207" height="516" alt="image" src="https://github.com/user-attachments/assets/4f5a8009-c7c9-4c96-b2d8-69c577d526c8" />

We can then access the web-based management dashboard, by inputting the machine's IP address and port number on a web browser. In my case, my Proxmox host can be reached by typing in:

    http://192.168.10.3:8006
<img width="1918" height="943" alt="image" src="https://github.com/user-attachments/assets/d0d966eb-ddd3-4d03-9554-7319cc626666" />

Success!
We've got Proxmox running on the machine. I could stop here and proceed with spinning up VMs and LXCs for different tools and services.

However, as I read through the documentation, as well as third-party tutorials, I saw it fit to perform a few maintenance steps, simply to make my life easier in the future. This meant ensuring that my system auto-updates, and that it sticks with the non-subscription environment. 

Proxmox VE is a software product that comes in a few flavors, mainly Enterprise, Non-Subscription, and Test. Proxmox VE is automatically configured for the Enterprise configuration. However, this is a heavy-duty version of Proxmox; it is severe overkill for my intended purposes, on top of requiring a subscription to have full utilization of the system.

Therefore, I will switch my installation to using the Non-Subscription repository. This is the free version of the main Enterprise build, meaning I will be receiving updates that may not be fully tested for stability. However, given that this machine is being used in learning environment, I think dealing with the prospect of maybe some tools breaking in an update is a very small price to pay for such a powerful platform. 

Furthermore, the prospect of things possibly breaking gives further motivation for adopting good practices such as routine backups, efficient troubleshooting, and keeping up with software updates and patches. 

<img width="1918" height="949" alt="image" src="https://github.com/user-attachments/assets/b651fdff-f63a-4c90-b22b-c11eeafd13d4" />
<img width="1918" height="889" alt="image" src="https://github.com/user-attachments/assets/aa37446b-aa61-4522-9209-1ca796c0952c" />

The system is all setup and ready to go!

Up next: setting up Pi-hole with Unbound. A DNS sinkhole + a recursive DNS resolver.

Thank your for reading! 😁

