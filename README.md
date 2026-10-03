# Home Lab Portfolio
A collection of all my projects, write-ups, and references for my home lab. The projects are numbered in the order they were performed. All documentation and tutorials referenced are included in the "references" folder for each project.

*Disclaimer: Artificial Intelligence is a fantastic tool, no doubt about it. However, this repo is a collection of ideas and learning experiences meant to challenge myself to learn new things and improve as an engineer. Thus, I pledge that no AI-generated work is being used in lieu of my own efforts.*

### 1. Initial Network Setup 

Because I am building a lab environment meant for testing and learning, I have to create a space where I am free to make mistakes without worrying about affecting other people. For this purpose, I sought to create a private subnet within my home network. With an enterprise router and a switch, I configured a private subnet, segmented into VLANs to compartmentalize devices for security purposes. An isolated lab VLAN was configured, with space for up to 20 devices. This VLAN is completely isolated from the rest of the network, making it an ideal environment to break things, and then fix them. 
