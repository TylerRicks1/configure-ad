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

- Step 1 Set up Resources in Azure
- Step 2 Ensure Connectivity between the Client and Domain Controller
- Step 3 Install Active Directory
- Step 4 Create an Admin and Normal User Account in AD
- Step 5 Join Client-1 to your domain
- Step 6 Setup Remote Desktop for non-administrative users on Client-1
- Step 7 Create users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<h2>Step 1| Set up your resources in Azure</h2>
<p>
<img src="https://i.imgur.com/4BaoTfw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In Azure create 2 Virtual Machines(VMs). One will be "DC-1" or Domain Controller using Windows Server 2022, the other "Client-1" or Client on Windows 10. 
 <br />
<img src="https://i.imgur.com/MhhXSW8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Make sure both VMs are on the same V-net. In azure set DC-1's ping assignment to static.
</p>
<br />

<h2>Step 2| Ensure Connectivity between the Client and Domain Controller</h2>
<p>
<img src="https://i.imgur.com/B331X9L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In Client-1, use the command terminal to ping DC-1's private IP address. Use this prompt ping -t<DC-1's Private IP>. The Request should timeout due to DC-1's firewall.

<img src="https://i.imgur.com/klRKCUr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Now we will disable the firewall. In DC-1 open "Windows Defender Firewall with Advanced Security". 
In Windows Defender Firewall with Advanced Security -> Inbound Requests -> ICMPv4 Echo Requests -> Enable. Now, in Client-1 verify that the perpetual ping you started earlier is no longer timing out.

<img src="https://i.imgur.com/kaOazQM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<h2>Step 3| Install Active Directory</h2>
<p>
<img src="https://i.imgur.com/J1naUid.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In DC-1 Install Active Directory Domain Services. 
Server Manager -> add roles/features--> Active Directory Domain Services -> Install -> Open

<img src="https://i.imgur.com/gtiixwV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Now configure DC-1 and add a new forest with the root domain name of "mydomain.com".


<img src="https://i.imgur.com/kNJQ3xe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Restart and log back into DC-1 as : "mydomain.com\user".
</p>
<p>

<br />

<h2>Step 4| Create an Administrator Account and Normal User Account in AD</h2>
<p>
<img src="https://i.imgur.com/5dOKfce.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In Active Directory Useres and Computers create two Organizational Units(OU) named "_EMPLOYEES" and "_ADMINS"

<img src="https://i.imgur.com/cCZLdGx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Create a new employee "Jane". Add "Jane" to the Domain Admins group
User -> Properties -> Member of -> type "domain_admins" into the text box -> apply
Close the Remote Desktop connection to DC-1 and reconnect into DC-1 as "mydomain\Jane"
</p>

</p>
<br />

<h2>Step 5| Join Client-1 to your domain</h2>
<p>
<img src="https://i.imgur.com/QyhLxg5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In Azure, set Client-1's DNS settings to DC-1's Private IP address. Restart Client-1.

<img src="https://i.imgur.com/LSpaLjJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

In Client-1 join to DC-1's domain.
System -> Rename Computer/Domain Changes -> and change the domain to "domain.com".
When prompted user "Jane"'s credentials to give permission to Client-1. Client-1 will now restart.

</p>
<br />

<h2>Step 6| Setup Remote Desktop for non-administrative users on Client-1</h2>
<p>
<img src="https://i.imgur.com/LA9y6Si.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to Client-1 as mydomain.com\jane and allow Domain_users to connect to Client-1.
System Properties -> Remote Desktop -> Change Users -> domain users -> apply. This allows for normal, non-administrative users to be able to log into Client-1. 
</p>
<br />

<h2>Step 7 Create users and attempt to log into client-1 with one of the users</h2>
<p>
<img src="https://i.imgur.com/GMzClsl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
Login to DC-1 as Jane and open PowerShell_ISE as an administrator. Next, create a new File and paste this script (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into the file. Run the script and observe the accounts being created. Lastly, attempt to log into Client-1 with one of the accounts (take note of the password in the script). If you are able to log in you have been successful in setting up active directory and allowing users to RDP into your DC-1. 
