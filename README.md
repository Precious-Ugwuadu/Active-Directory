# Active-Directory
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- 
- Install Active Directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain
- Set up Remote Desktop for non-administrative users on Client-1
- Create several additional users & attempt to log into Client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
  
![image](https://github.com/user-attachments/assets/e34e0423-92c2-410d-9747-0707c13ca887)

I Started by creating a Resource Group.

</p>
<p>
  
![image](https://github.com/user-attachments/assets/06f39b5e-5a24-4515-98cf-90fb258c8aff)

A virtual network and a subnet.

![image](https://github.com/user-attachments/assets/87b5c79e-be2a-40df-a29d-f73414ccc3b3)

And a Windows Server 2022 virtual machine and named it “DC-1” to serve as the Domain Controller. for the Image setting i  made sure to choose "Windows Server 2025 Datacenter: Azure Edition - x64 Gen2" to  ensures i get the latest features, performance, and security updates for enterprise-grade domain services in Azure.

![image](https://github.com/user-attachments/assets/057aa27e-fad6-404d-998e-d206d9ca447e)

I created another VM and named it "Client-1" and i set the image as "Windows 10 Pro, version 22H2 - x64 Gen2" to create a client machine that simulates a typical end-user workstation. This will allows me to join the domain, test user login, apply Group Policies, and verify Active Directory functionality from a user perspective.

![image](https://github.com/user-attachments/assets/725fdaaf-2397-4939-9eaf-b00024c77463)

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

![image](https://github.com/user-attachments/assets/54d7e80a-97c7-4dd8-bd2a-db3b6eb18f31)

  
I copied the public IP address of dc-1 and connected to remote desktop. And i was logged into Server Manager.

<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
