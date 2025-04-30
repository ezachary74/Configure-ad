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
- Windows (HP Envy) laptop

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a resource group in the Azure portal
- Create a virtual network in the Azure portal
- Create two virtual machines labeled as "Client-1" and "DC-1"
- Turn off Windows Firewall on DC-1
- Join the Client-1 VM to the domain of the DC-1 VM

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/66d1f637-1a25-4282-971c-f21ecff41288)


</p>
<p>
The first step in this process is to create a "Resource Group". Resource groups can be found by searching "resource groups" in the search bar. Once entering resource groups, select "+create". Label the resource group name to "Active-Directory" and select "review and create". After reviewing your resource group, select "Create".
</p>
<br />

![image](https://github.com/user-attachments/assets/31914ebc-eb75-4eff-b851-756e1cdea2d8)


</p>
<p>
Create a virtual network by first searching "Virtual network" in the search bar (if not shown on screen) and select "+create". Enter the name of the resource group that you created in the "resource group" text box to be sure they are connected. Name the virtual network "Active-Directory-Vnet". Proceed and select "create" on the bottom of the screen. "Active-Directory-Vnet" should show on virtual networks page.
</p>
<br />

![image](https://github.com/user-attachments/assets/70ffb7a5-65c8-4a15-9fc7-e9adf9886460)

</p>
<p>
Just as we created the resource group and virtual network, create both virtual machines. One VM named DC-1, which runs on Windows Server 2022, and the other named Client-1, which runs on Windows 10. Change DC-1's private IP address from dynamic to "static", ensuring that it doesn't change.
</p>
<br />

![image](https://github.com/user-attachments/assets/a636fb0c-4820-4549-864f-224a3a019e70)


Next, we sign into DC-1 Virtual machine and disable the "Windows Firewall". We perform this by right-clicking the Windows icon, selecting “run”, and typing wf.msc. In the Windows Defender Firewall window, we ensure the firewall is turned off for all profiles. Select "apply" and then select "OK".


![image](https://github.com/user-attachments/assets/d5a75918-f1e3-4edc-ae9a-0df78de50bc8)

We then have to set Client-1’s DNS settings to DC-1’s Private IP address. We do this be selecting "Client-1" VM, "Settings", "DNS Servers", change to "Custom", and paste DC-1's private IP address. 


![image](https://github.com/user-attachments/assets/34313a25-c364-4cc5-a3fc-127683357d3a)


We now have to log into Client-1's virtual machine to perform a ping attempt on DC-1's private IP address. We do this by utilizing Remote Desktop to log into Client-1 VM. We open Windows PowerShell and use the command "ping 10.0.0.4" to test the connection. After ensuring a positive connection, perform the command "ipconfig /all". The output for the DNS settings should show DC-1’s private IP Address.


![image](https://github.com/user-attachments/assets/93a47495-5a0b-48ae-ab54-05690edcd7fc)


Next, we must login to DC-1 and install Active Directory Domain Services to the VM. Go to the Windows icon and navigate to "server manager". Go to "server roles", select "next", and check the box showing "Active Directory Domain Services". Select "next" until you reach the confirmation section, and then select "Install".


![image](https://github.com/user-attachments/assets/1e2ab022-4f68-486f-9058-dc4ee8b725bc)

Then, navigate to server manager and click on the flag in the top right corner. Select "Promote this server to a domain controller", making DC-1 a controller. 

![image](https://github.com/user-attachments/assets/4fc78aeb-af6e-4a61-988f-81fab5f99b06)

![image](https://github.com/user-attachments/assets/d1ef5e37-817e-47b0-81c0-b269df4a758c)

![image](https://github.com/user-attachments/assets/529c3ba5-af38-4058-a47f-b76c667dc428)


Setup a new forest as "mydomain.com", select "next". Enter your chosen password and select "next". Continue to select "next" until you navigate to "prerequisites check", select "install". Following this step, DC-1 should automatically restart. Log back in to DC-1 VM using "mydomain(back slash)"username", to login as a domain user instead of a local user. 


![image](https://github.com/user-attachments/assets/ea2cebd1-eb3c-4bee-b2b6-0593f053a9d2)


Then we open Active Directory Users and Computers from the Windows icon. We then create two organizational units called "_EMPLOYEES" and "_ADMINS". We do this by right-clicking mydomain.com, selecting “New” and choosing “Organizational Unit”.


![image](https://github.com/user-attachments/assets/eb46b61f-9d8f-4bc1-b64a-ee237b0b5ff3)


Next, we right-click on "_Admins" and create a new user named Jane Doe, under the username jane_admin. We proceed by adding Jane Doe as a domain admin. Right-click the user, select Properties. Navigate to the “Members Of” tab and click “Add”. In the “Enter object names” field, we type “domain admins” and press "enter".


![image](https://github.com/user-attachments/assets/424a5ead-72d8-47bd-a446-2e28174a1acf)


Log out and close the connection to DC-1. Log back in as “mydomain.com\jane_admin”. This account will be used to log in to DC-1 from now on.


![image](https://github.com/user-attachments/assets/ab90af60-3b86-40ac-a844-e20b0d1004a6)


Then we join the domain from the Client-1 VM. Login in to Client-1 VM. Right-click the Windows logo, select “System”, then “Rename this PC (Advanced)”. Select “Change”, select “Domain”, and enter the domain name “mydomain.com”. To apply the changes, the VM will then restart.


![image](https://github.com/user-attachments/assets/74edcdb6-00d9-41e8-9afa-d3edcc74b6a0)


Navigate back to DC-1 and open Active Directory Users and Computers through the Windows icon. Create another organizational unit named _CLIENTS under mydomain.com.


![image](https://github.com/user-attachments/assets/db475c69-5b7b-4f19-b296-eaf10e7f6209)


We then log into the Client-1 VM as jane_admin. Right-click the Windows logo, select “System”, then choose “Remote Desktop”. Click “Select users that can remotely access this PC” and add domain users.


![image](https://github.com/user-attachments/assets/e72f26f7-ee3f-4a8b-bad8-9e0410bea812)


The next step is to use the script found here: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1. Log into DC-1 as jane_admin and open PowerShell ISE as an administrator. Create a new file, paste the script, and execute by clicking "run script". You should see that thousands of accounts are created.


![image](https://github.com/user-attachments/assets/712c697c-d40d-4360-8f4b-80bf4d5839d8)


After observing the users being created in PowerShell, open Active Directory Users and Computers to verify that the accounts have been created in the appropriate _EMPLOYEES organizational unit.



