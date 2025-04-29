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
- Step 4

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
Just as we created the resource group and virtual network, create both virtual machines and set DC-1's private IP address to "static", ensuring that it doesn't change.
</p>
<br />
