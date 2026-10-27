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

Create the Ansible project directory. Then, create the inventory file using the command `nano inventory.ini`
<img width="361" height="178" alt="image" src="https://github.com/user-attachments/assets/adb4a389-7dc2-4680-8a82-af4cf0595ed7" />

Check that everything is working properly using Ansible's connectivity test:
<img width="1719" height="456" alt="image" src="https://github.com/user-attachments/assets/ccfe2547-83fe-4102-93e4-7b8ce6a4a20a" />

After creating a basic web page template, proceed to creating the playbook `site.yml`. The playbook installs Nginx, makes sure the service is running, installs the personalized page, and returns basic diagnostic system information:
<img width="970" height="996" alt="image" src="https://github.com/user-attachments/assets/65776fdd-7ad8-433c-b8ce-08b7d6542a5b" />

During the first run, I encountered an error:

<img width="1071" height="412" alt="image" src="https://github.com/user-attachments/assets/89a81e87-28f0-4028-b8d9-628367210695" />

Turned out to be just some formatting error in the yml file:

<img width="592" height="982" alt="image" src="https://github.com/user-attachments/assets/e4101af1-1eda-4d9e-bfda-67e8bfc1bca2" />

Once the syntax errors are fixed, ran the playbook again:
<img width="1731" height="409" alt="image" src="https://github.com/user-attachments/assets/e86c8e25-2510-477f-a82f-57ed19be4388" />

Now, I got an authentication error. After double-checking the `ssh` configuration for both worker VMs, I checked if `sudo` is configured properly. After some research and a lot of hair pulling, I discovered that my OS of choice (Ubuntu Server 26.04.1 LTS) uses `sudo-rs`, an implementation of `sudo` made in Rust, which is has become the default `sudo` implementation for Ubuntu since version 25.10. It is a re-implementation made to reduce exposure to classes of memory-safety errors. `sudo-rs` handles authentication prompts differently from the usual `sudo`, hence why it is incompatible with Ansible. This was fixed by going back to the .init file and adding a new field to the `[web:vars]` field, forcing the hosts to use the legacy version of `sudo`:

<img width="418" height="241" alt="image" src="https://github.com/user-attachments/assets/22f09177-41fa-4ed8-93dd-eeb1f4e07087" />

After going in and forcing the playbook to use traditional `sudo` for authentication, the playbook ran successfully and provided the requested info from both managed hosts:

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

This was just another typo, this time within my .ini file that was leftover as I was playing around with the configuration:

<img width="405" height="229" alt="image" src="https://github.com/user-attachments/assets/7e976fb3-f9c7-470e-aadc-9ebbbd63fc7b" />

After correcting the typo, and re-running the script:

<img width="1723" height="547" alt="image" src="https://github.com/user-attachments/assets/efb18336-3476-410a-aaa1-be049787253a" />

<img width="1737" height="1078" alt="image" src="https://github.com/user-attachments/assets/09ab9e28-2109-468b-b72a-d7faa2eb67ef" />


The script works perfectly!

Now to try it on a PowerShell terminal. First, write the PowerShell script:
<img width="1414" height="456" alt="image" src="https://github.com/user-attachments/assets/c2b34cde-4ba5-4c01-8f4f-72e117e9af47" />

Running the PowerShell script yielded a permission error:
<img width="1348" height="318" alt="image" src="https://github.com/user-attachments/assets/7efc1f32-abb6-4b9d-8fef-dc5ddb3b1406" />

I then changed the execution policy to allow scripts to run for my user:
<img width="1342" height="483" alt="image" src="https://github.com/user-attachments/assets/d0fb7e9d-ddde-4555-91d0-201ff9ebf9e4" />

Re-try running the script from my PowerShell terminal:
<img width="1105" height="493" alt="image" src="https://github.com/user-attachments/assets/ccb9d460-14ce-4b76-b2fe-0df0597442f1" />
<img width="1918" height="873" alt="image" src="https://github.com/user-attachments/assets/9bbcfbce-accf-4427-a084-33c064fdab69" />
<img width="1911" height="465" alt="image" src="https://github.com/user-attachments/assets/91d096ae-d292-4eee-b619-3dddc2886f19" />

Everything is running perfectly!

Now, to take it a step further: adding Identity Access Management

I will have two roles: one admin, and one viewer.

Generate the admin SSH key:
<img width="976" height="507" alt="image" src="https://github.com/user-attachments/assets/fcab27a0-4a51-4c8c-b713-f55ce2c16cf2" />

Generate the viewer SSH key:
<img width="1056" height="460" alt="image" src="https://github.com/user-attachments/assets/2dd735dc-ceb5-47ba-8234-056aa269e2d7" />

Take note of the public keys, as they will be needing for the Ansible configuration:
<img width="1042" height="117" alt="image" src="https://github.com/user-attachments/assets/659ec989-1124-47a7-bd62-57c96127a325" />

create the IAM .yml file:
<img width="1101" height="234" alt="image" src="https://github.com/user-attachments/assets/29d8b9e2-1686-4dcb-9b8c-42a7897a3bf2" />

Create the password hashes to store in the Ansible vault:
<img width="1278" height="307" alt="image" src="https://github.com/user-attachments/assets/ccbf9380-1104-4bb1-b705-b841049601a4" />

Create the .yml file with the password hashes and encrypt it:
<img width="1221" height="342" alt="image" src="https://github.com/user-attachments/assets/1d40af76-bbd0-44ee-aa9a-85d73f1498d7" />

Run the `iam.yml` file:
<img width="1252" height="1078" alt="image" src="https://github.com/user-attachments/assets/b9b1a9a3-fb12-4df0-b99a-298a586b180e" />
