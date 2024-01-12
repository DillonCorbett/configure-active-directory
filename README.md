<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployment in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setup the Domain Controller and Windows 10 virtual machines within Microsoft Azure.
- Step 2: Ensure that the Windows 10 machine can connect to the Domain Controller by enabling ICMPv4 on windows Firewall of the Domain Controller.
- Step 3: Install and setup Active Directory.
- Step 4: Creating OUs, Users, and assigning roles.
- Step 5: Add the Windows 10 VM to the domain.
- Step 6: 

<h2>Deployment and Configuration Steps</h2>

*<h3>Step 1</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/192c5457-a17a-4dd6-a22c-2dca15f8ab09)

<p>
Once in Microsoft Azure, begin by navigating to Virtual Machines. Click "Create" to start creating the virtual machines. This virtual machine will be the Domain Controller, so for this project it was named DC-1. For the operating system choose Windows Server 2022 and give it at least 2 virtual CPU's. Then create a username and password and click "Review & Create" at the bottom.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/ce4c88c9-85b2-403d-a7ed-94f91a7c5d19)

<p>
Now head to virtual machines within Azure, click on the new DC-1 machine. Then navigate to Networking under Settings, and click on the Network Interface. From here navigate to IP configurations, and click on "ipconfig1". Here you want to set the private IP address to "Static" so that the IP address stays the same for the Domain Controller. Once you change this Private IP address from Dynamic to Static click Save.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/6d59b31c-7ea9-4e2b-9da9-231a06801ab4)

<p>
With the Domain Controller setup, head back to Virtual Machines and create another one. This one with a regular Windows 10 operating system. After choosing operating system, virtual CPU's, and creating a username and passworkd, check the box at the bottom under Licensing. Make sure that it is in the same Resource Group and Network as the Domain Controller. To check the network, navigate to the Networking tab before you finish creating the VM and make sure it is the same virtual network that the Domain Controller is on. Once this is done click "Review & Create".
</p>
<br />

*<h3>Step 2</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/ea56d036-88a9-48c3-b037-9b2afc72dc8a)

<p>
Now that the Domain Controller and other VM are set up, use remote desktop to connect to the Windows 10 VM. Navigate to Virtual Machines in Azure to see the IP address needed for Remote Desktop, and use the username and password that were made to create the VM to login. Once logged into the VM open the command prompt and attempt to ping the Domain Controller using ping [private IP address] -t. The requests will not go through.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/32540005-50fc-4094-82b9-9790ac36d3f2)

<p>
To fix the connection between the Windows 10 machine and the Domain Controller, log into the Domain Controller using Remote Desktop the same way as the other VM with its IP address and username and password that were created for it. Then once logged in, open Windows Defender Firewall with Advanced Security from the windows search bar. Then navigate to Inbound Rules and find the two rules called "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)", select both of these and then click "Enable" on the right. Once this is done go back to the other virtual machine and check the ping, it should show that the ping is now going through and they can connect.
</p>
<br />

*<h3>Step 3</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/a264b4ed-1887-415d-9c5a-7643452544bd)

<p>
When you first logged into the Domain Controller VM, Service Manager should have opened. From in here click "Add roles and features" to begin setting up Active Directory. When going through the setup make sure under "Server Roles" to select "Active Directory Domain Services" and then continue hitting next and going through with the installation. 
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/a41d0f22-af36-4ccb-9774-7d5c3d3a9413)

<p>
After completing the installation, click the yellow exclamation mark in the top right, here click "Promote this server to a domain controller".
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/c38df92c-1431-4ec6-b8bf-ce13aa60c70f)

<p>
In Deployment Configuration, select "Add a new forest" and give the domain a new name. Click next when done, on the next page a password will need to be created. Then continue through the configuration process until you can click Install. When the install finishes it will log you out of the machine.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/e6e7a450-d0cc-4f15-bfdc-c32fc4fc3927)

<p>
Log back into the Domain Controller with remote desktop. Except this time replace the username on log in with the name you created for the domain followed by a slash and the username such as, sampledomain.com\projectUser.
</p>
<br />

*<h3>Step 4</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/e05a8c4f-a1d5-4b5e-91b9-98f51fc11b37)

<p>
Once back in the Domain Controller VM and in Server Manager, navigate to Tools at the top right and click "Active Directory Users and Computers".
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/a610677b-0aad-4692-b490-d3c076d1a676)

<p>
Once in Users and Computers, you can begin to create Organizational Units and Users. To add an Organizational Unit to the domain, right click it and click New > Organizational Units. Then from within the OUs you can add users to them. Here I created Employees and Admins.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/1bb423fb-3f25-40da-8ea8-8f054bc5a8b7)

<p>
To create a user, right click the OU you wish to add them to and then click New > Users. Fill out the information and click next, create a password, for the sake of this tutorial uncheck "User must change password at next logon" and click next.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/e479a0a2-3e4f-4b23-80f0-4bf43561de5f)

<p>
To make this user an Admin, right click them and click "Add to group". Then in the text box type "Domain Admins" and click "Check Names" and then "Okay".
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/327a1bb7-c4cf-4a76-996d-ec425294d910)

<p>
Now log out and sign back in as the new admin user.
</p>
<br />

*<h3>Step 5</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/f637ff31-2733-425c-811d-14bba8a61d18)

<p>
Now VM-1 needs to be added to the domain that was created. From within Microsoft Azure, navigate to VM-1, go to Networking, and select its Network Interface. From here select DNS servers and it will have "Inherit from virtual network" selected. Change this to custom then input the private IP address of the Domain Controller VM and click "Save". Once this is done, navigate back to the orignial VM-1 page and click "Restart" at the top to restart the virtual machine. THen log back in as the original user for that VM. From in this VM, right click the Start button and click "System". Then go to "Rename this PC (advanced)" on the right side of this window so that this VM can be added to the domain.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/cfc068ed-4a56-4eee-bff2-927ab240602c)

<p>
Once here, click the "Change" button and then select "Domain". Input the name of the domain that was created and press "Ok", a login prompt will appear and login with the domain admin account that was created previously. Once this is done the VM will need to restart.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/798f7afa-c92f-4aa5-b7c2-69d70573df1e)

<p>
Now head back to the Domain Controller VM and to Active Directory Users and Computers. Click on the "Computers" OU and you should see VM-1 here, now apart of the domain.
</p>
<br />

*<h3>Step 6</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/29a9103f-629a-460a-b82c-bc9c1642342d)

<p>
With the VM now on the domain, it must be set so that all non-admin users can remotely access this PC. Sign into VM-1 with the admin account, right click the Start button and select "System". Then go to "Remote Desktop" and under "User accounts" click "Select users that can remotely access this PC".
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/1a5e3221-9813-4571-9637-26492f7cd650)

<p>
Next, click the "Add" button, input "Domain Users" into the text box, click "Check Names" and then click "Ok". Now all users within the domain can sign into this VM. 
</p>
<br />

*<h3>Step 7</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/d1cfc21a-1646-40ad-a7e1-197dab88476c)

<p>
Back on the Domain Controller, I made a couple sample users. These will be used to test to see if any user can now log into the other VM.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/149be5fa-920b-4b6e-a119-80a0ac5d581a)

<p>
Using these new user accounts login to VM-1 with their account.
</p>
<br />

![image](https://github.com/noles498/configure-active-directory/assets/143885547/639619a3-90b6-4958-af04-e76828cf442c)

<p>
Once logged in, open command prompt and type in "whoami" as well as "hostname" to see that the new user is logged into the VM-1 virtual machine on the domain that was created.
</p>
<br />

*<h3>Step 8</h3>*

![image](https://github.com/noles498/configure-active-directory/assets/143885547/313accfc-e929-4767-823f-e6b1d8db2e97)

<p>
If a user needs their password reset or account unlocked, head to Active Directory Users and Computers. Then find the user, right click their name and select "Reset Passord". Here you can give them a new password and unlock their account. There is also the option when right clicking the user to disable to account if that needs to be done instead.
</p>
<br />









