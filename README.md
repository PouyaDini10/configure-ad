<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

**Step 1: Create the Domain Controller VM (Windows Server 2022) named “DC-1”**

What do we mean by Domain Controller? 

This will be the server that runs Active Directory Domain Services(AD DS) and handles all of the core functions.

Create a resource group
Create virtual network, called Active-Directory Vnet
Create the virtual machine
- Assign the appropriate Resource Group
- Put it in the same region as your virtual network
- Image: Windows Server 2022 Datacenter(Azure Edition) x64
- Administrator Account: Pick a username & password and save it for later
- Ensure it’s in the Active-Directory Vnet we created earlier
- Click Create to begin the creation process 

<img width="398" alt="image" src="https://github.com/user-attachments/assets/d93456ea-bf7e-427e-94b1-b7d5ab97f700" />
<br><br><br><br>

**Important Step:** Set Domain Controller’s NIC Private IP address to be static

In this scenario, we will be joining the Client-1 virtual machine to the domain, they rely on that static IP address to locate the domain controller using DNS. 

So, why do we want to set the DC’s(Domain Controller) private IP to be static? 

Very simple, we don't want to lose connectivity in any given situation. So we set the private IP address to static. 
<br><br>
**Setting DC's private IP to static**

<img width="467" alt="image" src="https://github.com/user-attachments/assets/b8e96c08-5ffe-4a4a-95e1-a4531071ef91" />

Next, we must login to the virtual machine and disable the windows firewall. We're only doing this to ensure that Client-1 is able to talk to DC-1 through our internal IP’s.
Lets first launch our Windows Server 2022 virtual machine to ensure a successful installation. On my computer, I will do a remote connection to the server using RDP & the Server’s Public IP and enter the appropriate credentials. Once logged in, if you see the server manager immediately pop up, everything looks good. In the search bar, type run, and input this command, “wf.msc”, this will open the firewall settings. Turn off the firewall state for all profiles, such as Domain, Private, and Public.

Disable all the firewall settings shown below:
![image](https://github.com/user-attachments/assets/2f926f97-3e9b-4cf7-a32c-22028b30b028)




**Step 2: Create the Client VM (Windows 10) named “Client-1**

Create the Windows 10 Virtual Machine 
-Assign to resource group Active Directory
-Name the VM Client-1
-Image: Windows 10 Pro
-2 vcpus(Virtual CPU’s) will be sufficient
-Create a username & password(be sure to document it for later)
-Ensure that it’s operating on the same Virtual Network we created earlier: Active Directory-VneT
-Click create virtual machine

**Important Step:** After the VM is created, set Client-1's DNS settings to DC-1's private IP address.

Why’re we doing this? 

The answer is very straightforward,

First let's examine the capabilities of a Windows Server 2022 VM(Domain Controller):
Hosts Active Directory Domain Services(AD DS)
Responsible for domain management
Handles logins, group policies, user accounts, computer accounts, and more
Basically it’s the focal point for operations


Our Client-1 virtual machine’s DNS server, which by default is the one offered from Azure, has no idea of our private network or the domain in which we are operating. So we need to configure the settings so it knows who to communicate with. 

To get started, on Azure,open Client-1's network settings, navigate to the DNS servers, and input the public IP of the DC-1 VM.

**Configuring DNS settings for Client-1:**

<img width="476" alt="image" src="https://github.com/user-attachments/assets/9bb926d3-40f2-44c4-b28f-c96c4dd62b67" />
























