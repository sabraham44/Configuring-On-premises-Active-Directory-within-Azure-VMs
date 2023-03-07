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

<p>
</p>
<p>
In this lab we will create two VMs in the same vNET<br />

<li>Domain Controller VM (Windows Server 2022)</li>
<li>Client VM (Windows 10)</li>
<br/>
We will set the DC's Virtual Network Interface Card (vNIC) Private IP to be static, because its offering Active Directory services to the client machine and we don't want the IP address to change. The Client machine will be joined to the domain. We will control the DNS settings on the client machine, because we want the client machine to use the DC as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will want to ensure both DC-1 and Client-1 are on the same virtual network and subnet. Then we will try to ping DC-1 from Client-1 using -t (perpetual ping). This will not will not work at first, because . In order for the ping to be successfull, we have to enable ICMPv4 on the local windows Firewall. Now we are able to ping DC-1 successfully from Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/HvZBWzc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/1lrrGPw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Next we will log back into DC-1 to install Active Directory Domain Services. Promote the VM to DC, setup a new forest as "mydomain.com" then restart. We can then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly you should be able to run AD Users & Computers as shown below.
</p>
<img src="https://i.imgur.com/cGjvRke.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
In AD Users and Computers (ADUC), create Organizational Units (OU), called _EMPLOYEES. Create another OU named _ADMINS. In order to do that right click on the domain area. Select New, from the menu, then choose Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named Jane Doe, she will be admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group. 
</p>
<img src="https://i.imgur.com/hL7g5Y5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kcgvzdE.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
Moving forward we can use Jane_admin as the administrator account. 
<br/>
<h3> Join Client-2 to Domain (mydomain.com)</h3>
To start he joining process, set Client-1's DNS settings to the DC's Private IP addressl. Next, we restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS. 
</p>
<img src="https://i.imgur.com/jbrGTXW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://i.imgur.com/kvcm2cY.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. From the menu on the right select 'Rename this PC (Advanced). Then select to change the domain and enter "mydomain.com" next we input our credentials from mydomain.com\labuser. Your computer will restart and client-1 will be a part of mydomain.com
</p>
<br />
<p>
  <p>
<img src="https://i.imgur.com/Ze0Em5e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Client-1 has been successfully added to the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing these steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://i.imgur.com/SApOKiE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, to verify that noraml users can RDP into Client-1, we will use a PowerShell script to generate thousands of users into the domain, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://user-images.githubusercontent.com/126641333/223316246-8c842ff5-aede-4540-9301-57f866fe8732.png"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
