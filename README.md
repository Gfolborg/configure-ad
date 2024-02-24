<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Client (Windows 10 22H2) and Domain Controller (Windows Server 2022) virtual machines
- Log on to both VM's and check the connectivity between the 2 VM's
- On the Domain Controller
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/Z5TidAg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Azure, create two virtual machines. "DC1" will host the Active Directory Domain Service and act as the Domain Controller. "Client-1" will be added to the "DC1" server.
</p>
<br />

<p>
<img src="https://i.imgur.com/GEVzoz3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click DC1 machine > Network Settings > dc1439 (Network Interface Card)
</p>
<br />

<p>
<img src="https://i.imgur.com/vR0ZJm3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click IP configurations > ipconfig1. An "Edit IP configuration" panel will display on the right of the screen.
</p>
<br />

<p>
<img src="https://i.imgur.com/ImmDhxT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the panel, select "Static" since the Domain Controller will act as the DNS and don't want the Domain Controllers IP address to change which could disrupt the clients connected to the Domain Controller.
</p>
<br />

<p>
<img src="https://i.imgur.com/IKifvKp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into DC1 and a Windows Srver Manager will automatically start. Click "Add roles and features" to install Active Directory.
</p>
<br />

<p>
<img src="https://i.imgur.com/82Jmhrm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On Add and Features Wizard window, click "Next" until you reach Server Roles.
</p>
<br />

<p>
<img src="https://i.imgur.com/pvXKQ1I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under Server Roles, select "Active Directory Domain Services", add features, then continue to click Next until you reach Install.
</p>
<br />

<p>
<img src="https://i.imgur.com/ztzs5cd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Going back to the Server Manager, on the top right of the window there will be a flag with a warning icon. Click the flag and click Promote this server to a domain controller.
</p>
<br />

<p>
<img src="https://i.imgur.com/gLajwHC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this window, start by clicking "Add a new forest" and name your domain. In this example, the domain will be names mydomain.com
<br />

<p>
<img src="https://i.imgur.com/mfS0lSW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On this window, put in a password and continue to click Next until you see and click Install. It may take a few minutes to install. The computer will restart causing the RDP to disconnect with the virtual machine. 
</p>
<br />

<p>
<img src="https://i.imgur.com/ZVxqExT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
RDP back into the domain controller. On the taskbar, search and click "Active Directory Users and Computers".
</p>
<br />

<p>
<img src="https://i.imgur.com/Pd5mdQs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the Active Directory window, I clicked on "mydomain.com" then right clicked to bring up more otions and clicked New > Organizational Unit and created 3 files: _EMPLOYEES _CLIENTS _ADMINS
</p>
<br />

<p>
<img src="https://i.imgur.com/gbwT6L3.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/rtRALJ6.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/2jIjC0H.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
I wanted to add an Admin account and use that account to control the Domain Controller. Since it's not good practice to be logged as the main server account. I right clicked "_ADMIN" > New > User and started to created an admin named "Jane Doe".
</p>
<br />

<p>
<img src="https://i.imgur.com/TrAAsdF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I then needed to add Jane Doe to the "Domain Admins" by right clicking on the User's name > Add to a group.
</p>
<br />

<p>
<img src="https://i.imgur.com/4yGFgdq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From there I typed "Domain Admins" in the "Enter the object names to select" field then clicked "Check Names" to link what I typed into the field. Then I clicked "Ok". Jane Doe is now the admin and is able to controll the Active Directory as the admin. So I will log off and sign back on as mydomain\Jane Doe.
</p>
<br />

<p>
<img src="https://i.imgur.com/7RS3Ayq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Going back to the Azure dashboard, I went into Client-1 vNIC and configured the DNS server to be the server of the Domain Controller. By designating the domain controller the DNS server, the domain "mydomain.com" would be recognized by the domain controller when you are trying to connect Client-1 to that domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/PYCfdDv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next is to connected the Client-1 to the mydomain network. On the Client-1 virtual machine, right click Windows symbol on the taskbar and click on "system".
</p>
<br />

<p>
<img src="https://i.imgur.com/l9zu57h.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/82eT412.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/DSarQeK.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

</p>
<p>
An "About" window will be displayed. Click on Rename this PC (advanced). On the system property window, click Change to change Client-1 domain. I then selected "Domain" and entered "mydomain.com" in the text field and clicked "Ok".
</p>
<br />

<p>
<img src="https://i.imgur.com/WI9r1ES.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
A Windows Security window popped up and I used the domain controllers admin username mydomain\Jane Doe and password, clicked "Ok". From there the Client-1 virtual machine has been added to the mydomain network. Client-1 will now restart. You can now log into Client-1 virtual machine as mydomain\Jane Doe.
</p>
<br />

<p>
<img src="https://i.imgur.com/TpydbTz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
It's time to add the Domain User group to this computer. This way any user in the Active Directory will have access to that computer. I'll log  back on to Client-1 as the admin mydomain\Jane Doe, go to system settings and click "Remote desktop".
</p>
<br />

<p>
<img src="https://i.imgur.com/WKnLZTq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this window, click "Select users that can remotely access this PC".
</p>
<br />

<p>
<img src="https://i.imgur.com/G5XR3qL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
On the next window, click "Add" and in the text field on the following screen, type in "Domain Users" then click "Check Name" and "Ok". Now any users in the Active Directory "Domain Users" can have access to the Client-1 computer.
</p>
<br />

<p>
<img src="https://i.imgur.com/gx1BVQz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, on the domain controller, I will have Employees created by running Windows Powershell ISE as an administrator, entering a script to generate these employees to use for this demonstration.
</p>
<br />

<p>
<img src="https://i.imgur.com/G8unqz4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open and refresh the Active Directory. Under the _EMPLOYEES folder, you could see all of the users that were created. In the Powershell script, all of the users have the same passwords but those could be changed in Active Directory. This can be used to practice on the active directory.
</p>
<br />
