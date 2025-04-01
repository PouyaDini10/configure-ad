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
<br><br><br>

Next, we must login to the virtual machine and disable the windows firewall. We're only doing this to ensure that Client-1 is able to talk to DC-1 through our internal IP’s.
Lets first launch our Windows Server 2022 virtual machine to ensure a successful installation. On my computer, I will do a remote connection to the server using RDP & the Server’s Public IP and enter the appropriate credentials. Once logged in, if you see the server manager immediately pop up, everything looks good. In the search bar, type run, and input this command, “wf.msc”, this will open the firewall settings. Turn off the firewall state for all profiles, such as Domain, Private, and Public.

**Disable all the firewall settings**
![image](https://github.com/user-attachments/assets/2f926f97-3e9b-4cf7-a32c-22028b30b028)

<br><br><br><br>


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

Our Client-1 virtual machine’s DNS server, which by default is the one offered from Azure, has no idea of our private network or the domain in which we are operating. So we need to configure the settings so it knows who to communicate with. 

To get started, on Azure, open Client-1's network settings, navigate to the DNS servers, and input the public IP of the DC-1 VM.

**Configuring DNS settings for Client-1:**

<img width="476" alt="image" src="https://github.com/user-attachments/assets/9bb926d3-40f2-44c4-b28f-c96c4dd62b67" />

Once you’ve configured the DNS settings, restart the Client-1 virtual machine on azure, remotely connect again and attempt to ping DC-1’s private IP. Observe for a successful ping(we ping to ensure both machines are able to communicate). 

![image](https://github.com/user-attachments/assets/0d87934c-ecbf-4062-8e22-dd29b5c00add)

We can even run the command “ipconfig /all” on powershell to observe that the DNS server’s private IP is set to the domain controller.

![image](https://github.com/user-attachments/assets/b04a7381-7056-461d-96b4-1ed1543810f6)


<h2>Deploying Active Directory</h2>

**Part 1**

**Install Active Directory**

Login to DC-1 and install Active Directory Domain Services:
- Open Server manager
- Navigate to Add Roles & Features under Manage in the top right hand corner
- Continue to Server Roles and check the box for Active Directory Domain Services
- Install



![image](https://github.com/user-attachments/assets/d7c9cfe3-0875-48ef-875f-8b116e31f1bd)

**Promote as a Domain Controller:**
- Navigate to the notifications icon and hit “promote this server a domain controller”
- Click Add a new forest
- In root domain, input mydomain.com (can be anything you want)
- Install


**Create a domain admin user within the domain:**
- In Active Directory Users and Computers(AD UC), create an Organizational Unit(OU) called “_EMPLOYEES”
- Create a new OU(Organizational Unit) called “_ADMINS”
- Create a new employee named “Jane Doe” with the username of jane_admin and provide a password for that account. 
- Add Jane Doe to the “Domain Admins” Security Group
- Log out/close your remote connection and attempt to log back in with “mydomain.com\jane_admin”


<img width="367" alt="image" src="https://github.com/user-attachments/assets/84a4e05e-d937-473e-a8fe-2ec84b4bf7a4" />

<img width="385" alt="image" src="https://github.com/user-attachments/assets/f60f5921-823c-42d3-9d8b-6dc8923fcfe4" />

<img width="452" alt="image" src="https://github.com/user-attachments/assets/17f30fe2-26d6-4b06-bb2f-57b8dc6b6854" />

**Join Client-1 to our domain(mydomain.com):**
- Open settings , go to the About and open the advanced settings
- Find Computer Name/Domain options 
- Under Domain input the domain address, mydomain.com
- It’s going to ask you for the username/password, input the details
- Login as the Domain Controller, Verify it shows up in the AD UC(Active Directory Users Computers).
- Create one last OU(Organizational Unit), “_CLIENTS”
















**Part 2: Setup Remote Desktop for non-administrative users on Client-1**




















