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

Firstly, I will prepare the Active Directory infrastructure in Azure 
  
![image](https://github.com/user-attachments/assets/e34e0423-92c2-410d-9747-0707c13ca887)

I started by creating a Resource Group.

</p>
<p>
  
![image](https://github.com/user-attachments/assets/06f39b5e-5a24-4515-98cf-90fb258c8aff)

A virtual network and a subnet.

![image](https://github.com/user-attachments/assets/87b5c79e-be2a-40df-a29d-f73414ccc3b3)

And a Windows Server 2022 virtual machine and named it “DC-1” to serve as the Domain Controller. for the Image setting i  made sure to choose "Windows Server 2025 Datacenter: Azure Edition - x64 Gen2" to  ensures i get the latest features, performance, and security updates for enterprise-grade domain services in Azure.

![image](https://github.com/user-attachments/assets/057aa27e-fad6-404d-998e-d206d9ca447e)

I created another VM and named it "Client-1," and I set the image as "Windows 10 Pro, version 22H2 - x64 Gen2" to create a client machine that simulates a typical end-user workstation. This will allow me to join the domain, test user login, apply Group Policies, and verify Active Directory functionality from a user perspective.

![image](https://github.com/user-attachments/assets/725fdaaf-2397-4939-9eaf-b00024c77463)

![image](https://github.com/user-attachments/assets/54d7e80a-97c7-4dd8-bd2a-db3b6eb18f31)

I copied the public IP address of DC-1 and connected to the remote desktop. And I was logged into Server Manager.

<br />

 ![image](https://github.com/user-attachments/assets/7a45f566-d78e-467a-9c2c-69846265290d) 

I right-clicked the start button and clicked on "RUN," and typed "wf.msc" for Windows Firewall.

![image](https://github.com/user-attachments/assets/69f95c12-584a-46b4-bd53-d5867370c888)

I clicked on Windows Defender Firewall and turned off the firewall state of the Domain profile and Private profile.

![image](https://github.com/user-attachments/assets/64f7ae0e-a317-4b3b-b251-ffea3ff6e0e3)

I went back to Azure portal and copied the priviate IP address of dc-1, i went to networking, network settings, clicked on ipconfig1 (primary) on the left i clicked on DNS Servers and changed it from inherit to custom and pasted the private IP address of dc-1 and this will change it from Vnet DNS Server to dc-1. Whenever the computer wants to search for anything on the web, it looks to DC-1 for it. This process will allow me to join the domain. 

![image](https://github.com/user-attachments/assets/9e874d27-86e3-4c95-9aff-3825fb7d94e9)

I copied the public IP address of client-1 and logged into the remote desktop and tried pinging the private IP address of DC-1, and it was successful. 

![image](https://github.com/user-attachments/assets/66b676ba-46ad-45d0-88c8-2b92236c95e5)

On Client-1 in remote desktop, I opened PowerShell and ran the command ipconfig /all. This will display the network configuration, and under DNS Servers, you should see the private IP address of DC-1, confirming that Client-1 is using it for DNS resolution.

Secondly, I stated Deploying Active Directory. 

<p>

![image](https://github.com/user-attachments/assets/655b1a81-ef5a-475f-9a8c-4daf10a18dae)

I went to Azure, copied the IP address of DC-1 and logged into the remote desktop. I clicked on the start button and searched for Server Manager, I clicked on Add roles and features, Next, and Next.

![image](https://github.com/user-attachments/assets/e651fbe2-1431-454c-a737-af0c4847a27d)

When you see the above image, Service Selection, make sure there is only one which should be DC-1

![image](https://github.com/user-attachments/assets/4def1546-cfa7-4719-ac51-8bea8d9a80fd)

Select AD Domain Services and add features.

![image](https://github.com/user-attachments/assets/af6c2056-66e2-4ddf-aa34-db210027fab8)

Check the start destination service automatically if required, and install. 

![image](https://github.com/user-attachments/assets/b914c48b-19ff-4088-b3c0-e2fb83a2992d)

![image](https://github.com/user-attachments/assets/72427a18-638f-4083-a6a8-b9d93637792a)

After the installation, I will promote DC-1 as an actual domain controller by configuring it in the "new forest" I will click on the above yellow bar



</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
