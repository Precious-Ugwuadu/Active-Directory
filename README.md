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

After the installation, I will promote DC-1 as an actual domain controller by configuring it in the "new forest." I will click on the above yellow bar to add the forest  and the root domain name: mydomain.com, and complete the configuration.

![image](https://github.com/user-attachments/assets/988a86c1-b7a2-4954-9700-b6e0dc9fffc3)

After completing the configuration, the system automatically logged me out. To log back in, I needed to use domain credentials, so I entered the username in this format: mydomain.com\labuser, which tells the system to log me in as a user from the domain instead of the local machine.

![image](https://github.com/user-attachments/assets/b00f26db-deb8-40f5-9032-1ab76100ad3f)

In Active Directory Users and Computers (ADUC), I created two Organizational Units (OUs) named _EMPLOYEES and _ADMINS. To do this, I clicked Start > Windows Administrative Tools > Active Directory Users and Computers, then right-clicked on mydomain.com, selected New > Organizational Unit, entered the name, and clicked OK. I repeated the same steps to create the second OU.

![image](https://github.com/user-attachments/assets/ea2e96ba-300c-4c79-988d-31d92edff569)

Next, I Created a new employee named “Jane Doe” with the username of “jane_admin” by clicking on _ADMINS > Right click the empty space >  New > User and fill out the necessary details.

![image](https://github.com/user-attachments/assets/bdeedaaa-51f2-4cc9-9258-9a68a267e19f)

This account is not yet an admin, to make it one i will Add jane_admin to the “Domain Admins” Security Group to give it full administrative access across the domain. By right clicking account > Properties > member of > Add > "Domain Admins > Check name > OK > Apply > OK. This process made it an actual admin. 

I closed the connection to DC-1 and log back in as “mydomain.com\jane_admin” To do this: I Open Remote Desktop Connection (RDC) Clicked on  Edit > More Choices > Use a Different Account, Entered my username: “mydomain.com\jane_admin”  and my password, then OK. This successfully logged me in as the domain admin.

![image](https://github.com/user-attachments/assets/261c2ab6-bd9a-4ea7-a8d7-103b4f08e6f9)

I went back to Azure, copied the public IP address of Client-1 and logged into remote desktop. I right clicked on the start button > System > Rename this PC (advanced) > computer name > Change > Domain > mydomain.com

![image](https://github.com/user-attachments/assets/a1f3ee37-f865-413b-ba52-c1dfccb7903e)

I typed in my credentials “mydomain.com\jane_admin” and my password > ok. And i was able to join the domain.

![image](https://github.com/user-attachments/assets/1a5ec3a9-a2ff-4369-94ef-3ecf8f69634d)

I got this pop up message and this will restart my PC to join the domain and we it comes back on it will be a member of the domain.

![image](https://github.com/user-attachments/assets/f8f207b5-3382-4eed-901d-0441f46efe89)

I Logged into the Domain Controller and verify Client-1 shows up in ADUC.
HOW TO: 
I opened RDC > logged in as DC-1 using it's public IP address from Azure, once i sucessfully logged in i Clicked on start > Active Directory Users and Computers > expand mydomain.com > Computers then i can see Client-1.

![image](https://github.com/user-attachments/assets/20754af4-d132-4ff9-ad60-4c2bfc645d53)

I double clicked it and i can see the fully qualified name of Client-1. Create a new OU named “_CLIENTS” and drag Client-1 into it. 

![image](https://github.com/user-attachments/assets/de376515-1c20-4bc6-8ace-d51f3bb7c98b)

The next step is to Setup Remote Desktop for non-administrative users on Client-1. 
HOW TO:
Open Remote Desktop Connection (RDC) > Click on Show Options > Enter your username: mydomain.com\jane_admin Click on connect >  Enter your password: > Click on Yes and This successfully logged me in. 

![image](https://github.com/user-attachments/assets/6d588919-7364-460c-b520-f1f4f158b295)

I right clicked the start menu > System > Remote Desktop 

![image](https://github.com/user-attachments/assets/bdb0741b-ac1e-4fb0-97c0-681d5c506f47)

Allow “domain users” access to remote desktop
HOW TO:
From the above image click on > Select users that can remotely access this pc > Add > Type Domain Amins > Check Names > Type Domain Users (Which means all users counts by default)  > Check Names > OK > Click on MYDOMAIN\Domain Users > OK.

![image](https://github.com/user-attachments/assets/145131f2-0c8e-4d41-a8c5-0d7cf35b7f05)

Next, I Created several additional user accounts, then try logging into Client-1 using one of those accounts.
HOW TO:
Go to remote desktop and log in as DC-1. Search for Windows PowerShell ISE > Right Click it > Run as Adminstrator > Yes. Once PowerShell is open > Click on File > New > Open the Script https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 > save it as > Create-users > Paste the Script

</p>
<p>
  
![image](https://github.com/user-attachments/assets/2fcb63e2-28be-4889-85c3-e7227cf12ab2)

The Script will start creating alot of users.

![image](https://github.com/user-attachments/assets/2b186b18-a8d4-4cb0-a9b3-cffe0c9bfd58)

Once done, open Active Directory Users and Computers (ADUC) and verify that the accounts are listed under the _EMPLOYEES Organizational Unit.

I Signed in to Client-1 using one of the user accounts, making sure to use the correct password: "Password1" provided in the script.
HOW TO:
Go to Remote desktop
nid.xis

![image](https://github.com/user-attachments/assets/13a5df9b-68f0-47ca-95d2-d7ad0c717705)

After logging in, I opened powershell and i can see "nid.xis

![image](https://github.com/user-attachments/assets/d3dd2844-f1ca-41d8-9985-233ee71f3e7f)

I also went to folder > This PC > Windows C: > Users > and i can see all the profiles of all the local users i logged into.

![image](https://github.com/user-attachments/assets/4e669c8c-853b-4e23-a8f8-463ed9974ccb)

Handling Account Lockouts:
Log in to DC-1. Choose any user account you created earlier. Try logging in with that account using an incorrect password 10 times to trigger an account lockout.
HOW TO:
Use DC-1 publick IP address from Azure and log into Remote Desktop > Active Directory Users and Computers > _EMPLOYEES > Pick any name (bad.vew) > log in with wrong password > after 10 attempts, i was logged out. But i had to configure account lockout policy first in Active Directory.

![image](https://github.com/user-attachments/assets/8dcbc822-5f42-4eff-b193-2264dfe4c531)

HOW TO:
Go to DC-1 > Click start and type gpmc.msc > i saw the above image > Edit the Default Domain Policy > Navigate to
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy

![image](https://github.com/user-attachments/assets/3bfac286-6049-453e-8149-b8ada2b96301)

Set:
Account lockout threshold (5 invalid attempts) > Account lockout duration (30 minutes) > Reset account lockout counter after (10 minutes) > Click Apply and OK. This policy will apply to all domain users once updated. 

![image](https://github.com/user-attachments/assets/42fb3c10-3afc-49d4-86e8-d7b2469709b4)

Run gpupdate /force to apply it immediately. 

![image](https://github.com/user-attachments/assets/c6c69af4-f736-4669-aa08-cf3b1924d130)

Next, I will Observe that the account has been locked out within Active Directory. Unlock the account, Reset the password, Attempt to login with it. 
HOW TO:
On DC-1, open Active Directory Users and Computers (ADUC) > Find and double-click the locked user account > Go to the Account tab — you’ll see it's locked > Uncheck "Account is locked out" to unlock it > Click Reset Password, enter a new password, and click OK > Try logging in with the updated password — it should work.



Next, Enabling and Disabling Accounts > Disable the same account in Active Directory > Attempt to login with it, observe the error message > Re-enable the account, then attempt to log in again to confirm access is restored.

![image](https://github.com/user-attachments/assets/64e8a1c8-6bd5-4594-bf74-bd0513a4df26)

To Disable an Account:
Open ADUC on DC-1 > Find the user account (e.g., under _EMPLOYEES OU) > Right-click the account and select Disable Account > A small red down-arrow will appear on the user icon, showing it’s disabled.

![image](https://github.com/user-attachments/assets/41456383-1df7-418d-88e2-7719ebc4346e)

To Attempt Login:
Try logging in with the disabled account on Client-1 > You’ll see an error message like > “Your account has been disabled. Please see your system administrator.”
To Re-enable the Account:
Back in ADUC, right-click the same user account > Select Enable Account > Try logging in again — it should work now.

![image](https://github.com/user-attachments/assets/aa31c5c3-b030-4015-b7a6-783fc7e3a784)

Finally Viewing Logs:
Check the event logs on the Domain Controller. Review the event logs on the Client machine as well.
HOW TO:
On Domain Controller (DC-1) > Press Windows + R, type eventvwr.msc, and press Enter > In Event Viewer, go to: Windows Logs > Security or System > to view login attempts, lockouts, and system events.

On Client Machine (Client-1):
Do the same: Press Windows + R, type eventvwr.msc, and hit Enter > Check Windows Logs > Security or System to see login attempts and error messages. This helps track user actions and troubleshoot issues.

</p> >
<br />
