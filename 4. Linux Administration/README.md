allocate the IPv4 addresses for the VMs:
<img width="1101" height="147" alt="image" src="https://github.com/user-attachments/assets/0c8e0dcf-e150-4181-a759-ac8e3500348a" />

check that the VMs have the correct network interface configuration
<img width="1098" height="220" alt="image" src="https://github.com/user-attachments/assets/21f568bd-09ad-4a09-8d58-acb582975cca" />

<img width="1045" height="210" alt="image" src="https://github.com/user-attachments/assets/47104993-1bef-4648-b077-a4299ce84e1c" />


<img width="834" height="165" alt="image" src="https://github.com/user-attachments/assets/dcb667a9-d3a7-4a5c-835e-fc1d5d540b8b" />

Generate the SSH keys:
<img width="858" height="454" alt="image" src="https://github.com/user-attachments/assets/40e97c1a-77a8-4e52-9166-aefa9509591c" />

Share the public key to the worker VMs:
web01
<img width="1248" height="829" alt="image" src="https://github.com/user-attachments/assets/72de39d1-861e-4f60-bd15-5b62dd8912dc" />

web02
<img width="1264" height="829" alt="image" src="https://github.com/user-attachments/assets/143fab6b-6919-4eb0-aa28-b6fa78b6e1c4" />

Create the Ansible project directory. Then, create the inventory file using `nano inventory.ini`
<img width="361" height="178" alt="image" src="https://github.com/user-attachments/assets/adb4a389-7dc2-4680-8a82-af4cf0595ed7" />

Check that everything is working properly using Ansible's connectivity test:
<img width="1719" height="456" alt="image" src="https://github.com/user-attachments/assets/ccfe2547-83fe-4102-93e4-7b8ce6a4a20a" />

After creating a basic web page template, proceed to creating the playbook `site.yml`. The playbook installs Nginx, makes sure its service is running, installs the personalized page, and displays basic system information:
<img width="970" height="996" alt="image" src="https://github.com/user-attachments/assets/65776fdd-7ad8-433c-b8ce-08b7d6542a5b" />

Encountered a syntax error
<img width="1071" height="412" alt="image" src="https://github.com/user-attachments/assets/89a81e87-28f0-4028-b8d9-628367210695" />

Turned out to be just some formatting error in the spaces:
<img width="592" height="982" alt="image" src="https://github.com/user-attachments/assets/e4101af1-1eda-4d9e-bfda-67e8bfc1bca2" />

Once the syntax error is fixed, try running the playbook:
<img width="1731" height="409" alt="image" src="https://github.com/user-attachments/assets/e86c8e25-2510-477f-a82f-57ed19be4388" />

Got an authentication error. After double-checking the `ssh` configuration for both worker VMs, I checked if `sudo` is configured properly. After some research and a lot of hair pulling, I discovered that my OS of choice (Ubuntu Server) uses `sudo` version `sudo-rs`, which is incompatible with Ansible due to the way authentication credentials are wrapped. This was fixed by going back to the .init file and adding a new field to the `[web:vars]` field
<img width="418" height="241" alt="image" src="https://github.com/user-attachments/assets/22f09177-41fa-4ed8-93dd-eeb1f4e07087" />

After going in and forcing the playbook to use traditional `sudo` for authentication, the playbook ran successfully and provided the requested info from both worker hosts:
<img width="1726" height="1078" alt="image" src="https://github.com/user-attachments/assets/989f7b76-0620-4612-a8ac-90d444a38949" />

Double-check that both worker VMs work as intended by entering their addresses in a web browser, and see if we get the correct web pages:

web01
<img width="1911" height="244" alt="image" src="https://github.com/user-attachments/assets/6b8f19fc-c144-4131-ad9b-c654406da63f" />

web02
<img width="1917" height="235" alt="image" src="https://github.com/user-attachments/assets/389bf3de-d1f5-461c-94f1-c890e99c5735" />

I can now craft a bash script to automate the whole process:
<img width="982" height="357" alt="image" src="https://github.com/user-attachments/assets/56476484-c5a0-4477-8b28-653f3c2a1914" />

Running the script starts off well, but gives a new error:
<img width="1726" height="1078" alt="image" src="https://github.com/user-attachments/assets/2ada969a-d790-45c1-a83d-8a1b6b714153" />

This was just another typo, this time within my .init file. Correcting the typo, and re-running the script:
<img width="1723" height="547" alt="image" src="https://github.com/user-attachments/assets/efb18336-3476-410a-aaa1-be049787253a" />

<img width="1737" height="1078" alt="image" src="https://github.com/user-attachments/assets/09ab9e28-2109-468b-b72a-d7faa2eb67ef" />

The script works perfectly!
