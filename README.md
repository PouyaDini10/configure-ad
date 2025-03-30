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

**Step 2: Create the Client VM (Windows 10) named “Client-1**

Create the Windows 10 Virtual Machine 
Assign to resource group Active Directory
Name the VM Client-1
Image: Windows 10 Pro
2 vcpus(Virtual CPU’s) will be sufficient
Create a username & password(be sure to document it for later)
Ensure that it’s operating on the same Virtual Network we created earlier: Active Directory-VneT
Click create virtual machine

**Important Step:** Set Domain Controller’s NIC Private IP address to be static

In this scenario, we will be joining the Client-1 virtual machine to the domain, they rely on that static IP address to locate the domain controller using DNS. 

An appropriate question that follows, well why’s DNS involved? Naturally, we might think that DNS just converts websites to IP’s, but DNS can go even more beyond this especially in an Active Directory paradigm. In an Active Directory environment, almost everything must be easily identifiable and that’s where DNS will be our best friend. 

Without DNS, Active Directory would be useless!

So, why do we want to set the DC’s(Domain Controller) private IP to static? 

Very simple, we don't want to lose connectivity in any given situation. So we set the private IP address to static. 
<br><br>
Set DC's private IP to static

<img width="467" alt="image" src="https://github.com/user-attachments/assets/b8e96c08-5ffe-4a4a-95e1-a4531071ef91" />























