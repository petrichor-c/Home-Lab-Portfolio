I make sure to reserve a static IP (192.168.10.4) within the lab VLAN, as per the instructions provided in Pi-hole's documentation
<img width="1890" height="882" alt="image" src="https://github.com/user-attachments/assets/dea9b205-fd4d-4a91-a1a9-1f2a46876b5b" />


Next, I need to setup a Proxmox LXC to house Pi-hole. From Proxmox's documentation on Linux containers:

*Container images, sometimes also referred to as “templates” or “appliances”, are tar archives which contain everything to run a container. Proxmox VE can utilize two main types of images: System Container Templates for creating full virtual environments, and Application Container Images based on the OCI standard for running specific applications.*

Pi-hole can run on a variety of Linux distributions:
<img width="613" height="454" alt="image" src="https://github.com/user-attachments/assets/d1d93971-1076-4770-821a-ceece178f9e8" />

For my lab, I will be going with Debian as my host operating system. It is the closest I can get to Raspberry Pi OS on the Proxmox platform.

I will be using the command line to build my container, because I'm an adult. Proxmox comes with a CLI tool, called the Proxmox VE Appliance Manager (`pveam`), which allows you to pull the latest templates.

First, I make sure my templates are up to date by running the command `pveam update`. Then, I can list all available templates with `pveam available`
<img width="1219" height="661" alt="image" src="https://github.com/user-attachments/assets/82e50fa2-c299-41ed-90db-1418343e26ff" />

However, I can take it a step further and pipe in `grep` to only list the available Debian templates. Both regular and turnkey Linux templates are listed together. Since I have no need for the turnkey templates, I can narrow down my `grep` parse even further by using the invert match option (-v). My complete command now becomes:

  `pveam available | grep "debian" | grep -v "turnkey"`

<img width="1203" height="205" alt="image" src="https://github.com/user-attachments/assets/5d342cb0-67c8-4fda-815c-ddecd5d0bf88" />

From the list, I picked Debian 13 Standard.
<img width="1207" height="670" alt="image" src="https://github.com/user-attachments/assets/9b3688bb-57d5-46df-bc3a-e1c543813eb7" />


We can then double-check that the template is available with `pveam list local`
<img width="844" height="118" alt="image" src="https://github.com/user-attachments/assets/45ac9d5a-4b93-4dfb-8319-bdbc60b0803d" />

Now, to create the container. Proxmox allows you to create a container with any settings you want with the `pct` CLI tool.
<img width="852" height="597" alt="image" src="https://github.com/user-attachments/assets/5fbb5e00-290c-4ef4-8bb8-fb43d40c9929" />

Since the container will host Pi-hole, we need to ensure it meets Pi-hole requirements. This means the container will have access to a minimum of 512MB of RAM, and 4GB of storage. Additionally, some additional settings to ensure there is no issue with the underlying Debian infrastructure. Following the structure provided by the `man` page for the `pct` command, we run the following command:

`pct create 100 local:vztmpl/debian-13-standard_13.6-1_amd64.tar.zst --hostname pihole --cores 1 --memory 512  --rootfs local-lvm:4 --net0 name=eth0,bridge=vmbr0,ip=192.168.10.4/24,gw=192.168.10.1 --unprivileged 1 --features nesting=1 --onboot 1 --start 1`

The breakdown for this command is as follows:
- `pct create 100 local:vztmpl/debian-13-standard_13.6-1_amd64.tar.zst` creates a container with ID=100 using the Debian 13 template we downloaded
- `--hostname pihole` set the containers name to "pihole"
- `cores 1` allocate 1 CPU core to the container
- `--memory 512` allocate the necessary memory (512MB), as per Pi-hole's requirements
- `rootfs local-lvm:4` allocate 4GB of storage in the local disk, as per Pi-hole's requirements
- `--net0 name=eth0,bridge=vmbr0,ip=192.168.10.4/24,gw=192.168.10.1` set the network interface for the container, as well as assigning it the reserved IP address and default gateway.
- `--unprivileged 1` isolates the container from the host
- `--features nesting=1` enable nesting to prevent permission and credential errors from the `systemd` on boot up
- `--onboot 1` set the container to boot when the host system boots
- `--start 1` start the container right after it is created

Running the command, waiting for a few seconds, and we observe that the container for Debian 13 standard is up and online

<img width="1918" height="889" alt="image" src="https://github.com/user-attachments/assets/2f75cce6-1fdb-42d6-a641-eaccbe92517f" />
