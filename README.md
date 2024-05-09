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

- Setup 2 Virtual Machines in Azure:
   - Domain Controller VM (Windows Server 2022) with a static private IP.
   - Client VM (Windows 10) in the same Resource Group and Vnet as the Domain Controller.
- Login to both VMs using Remote Desktop (RDP).
- Enable Inbound Rules for "Core Networking Diagnostics" in the Domain Controller's Firewall to ensure connectivity with the Client Machine.
- Install Active Directory Domain Services in the Domain Controller VM.
- Create Administrator and Standard User Accounts in Active Directory.
- Link the Client VM to the domain and login using an Administrator account.
- Set up Remote Desktop for Non-Administrative users on the Client VM.
- Create additional users and attempt to log into the Client VM as one of the newly created users.




<h2>Setup Resources in Azure</h2>

<p>

- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
      - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
- Set Domain Controller’s NIC Private IP address to be static
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

<p>
Frist, we have to resource group in Azure and we will called it "AD-lab" 
<img src="https://i.imgur.com/WrSg5Je.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<P> In that "AD-lab" we will have to created two VM one is "DC - Domain Control" and the other one is "Client-1" </P>
<img src="https://i.imgur.com/MDsWR4o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p> in DC 1 we will change the private IP address from Dynamic to Static </p>
<img src="https://i.imgur.com/nfXul70.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/QSq4Pns.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h2>Ensure Connectivity between the client and Domain Controller
</h2>

<p>
  
- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
- Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
    - Wf.msc 
    - Inbound rule 
    - Portocal—> ICMPv4 → enable it 
- Check back at Client-1 to see the ping succeed
</p>

<p>you can try to ping it but it will just shoe "time request out" because we need to enable the ICMPv4. 
</p>
<img src="https://i.imgur.com/MrpVy5c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
Frist, we will copied the private IP address of DC-1. 
<img src="https://i.imgur.com/PnC53KN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<P> go to wf.msc and then to Inbound rule, then enable ICMPv4  </P>
<img src="https://i.imgur.com/xgUPhLJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/7yZ5KFE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<P> once we enable to "ICMPv4" we will get a reply back from it  </P>
<img src="https://i.imgur.com/aa89kgt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2>Install Active Directory </h2>

- Login to DC-1 and install Active Directory Domain Services
    - Go to server manager → add role and feature 
    - Click on ( active directory domain service) 
    - Install it 
- Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
- Restart and then log back into DC-1 as user: mydomain.com\labuser

<p> Go to server manager → add role and feature </p>
<img src="https://i.imgur.com/wmFH1GQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>Install it</p>
<img src="https://i.imgur.com/XGNCtMH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<P> Promote the server to Domain Controller </P>
<img src="https://i.imgur.com/Xu3z5kB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<P> Add mydomain.com </P>
<img src="https://i.imgur.com/nEKPY1y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2> Create an Admin and Normal User Account in AD </h2>

- In Active Directory Users and Computers (ADUC)
    - create an Organizational Unit (OU) called “_EMPLOYEES”
    - Create a new OU named “_ADMINS”
    - Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
    - Add jane_admin to the “Domain Admins” Security Group
    - Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
    - User jane_admin as your admin account from now on

<p> Go to server manager → click on active directory users and computers, click on mydomain.com and created organizational unit called _EMPLOYEES and _ADMINS</p>
<img src="https://i.imgur.com/Q3JUZfK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/3p5gudN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/MgszmJ2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p>Within the _ADMINS we will create a user called " Jane Doe" and we will add Jane Doe to Domain Admins security group </p>
<img src="https://i.imgur.com/73InzWG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/hpKzBn3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/oKxqE0j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<P> After that we will log out and log back with "jane_admin"  and to check it we can just go to the command line and type "hostname" </P>
<img src="https://i.imgur.com/LWW1KND.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h2> Join Client-1 to your domain (mydomain.com) </h2>

- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
    - Login → go to system and then change the name 
- From the Azure Portal, restart Client-1
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the     domain
<p> Go to Client-1 from Azure Portal, and set it's DNS setting to Domain Conroller private IP address </p>
<img src="https://i.imgur.com/twJUU32.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/tUippYW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<p> Log in to Domain Controller and verify and client-1 in (AUDC) inside computers container on the root of the domain  </p>
<img src="https://i.imgur.com/pKhAW2a.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>



<h2> Setup Remote Desktop for non-administrative users on Client-1 </h2>

- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user now
<img src="https://i.imgur.com/80ZEn74.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/lhvjtn7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/q4D5uCm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<h2> Create a bunch of additional users and attempt to log into client-1 with one of the users </h2>

- Login to DC-1 as jane_admin
- Open PowerShell_ise as an administrator
- Create a new File and paste the contents of the script into it <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">scripts</a>
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
- Attempt to log into Client-1 with one of the accounts (take note of the password in the script)

<p> After we log in to DC we will go go Powershell_ISE and run it as administrator </p>
<p> After that we will run the "SCRIPT" as the link provided and it will created a whole buch of users, pick one user and try to log into the Client -1 with that users name </p>

<img src="https://i.imgur.com/wbG1kNg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/AceetRV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/I8TDYlU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<p> In this case, I picked baw.vag </p>
<img src="https://i.imgur.com/fB1ueq8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<p> WIth "baw.vag" we will log into the Client -1 with that user name </p>
<img src="https://i.imgur.com/fDhdpBL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  







