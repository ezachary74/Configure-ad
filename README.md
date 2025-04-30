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


