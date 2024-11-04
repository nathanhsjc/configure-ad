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

- Virtual Machine Creation
- Installing Active Directory
- Step 3 Active Directory Configuration
  

<h2>Deployment and Configuration Steps</h2>

<p>

  ![image](https://github.com/user-attachments/assets/6490b7d6-21e6-40ac-8a06-51609187ca92)
</p>
<p>
For the creation of this Active Directory lab you will need to create a resource group, virtual network and virtual machine in Azure. The virtual network and virtual machines need to be in the same resource group. Two virtual machines will need to be created. In this lab the names of our virtual machines will be DC-1 (Domain Controller) and CLIENT-1.

Configure the NIC (Network Interface Card) within the DC-1 virtual machine by navigating to settings and clicking on the NIC. Once you have done so edit the IP config and set it to static and click save.
</p>
<br />

<p>

  ![image](https://github.com/user-attachments/assets/e9f57467-cf5f-4a68-b885-f873a64d79c1)

</p>
<p>
Retrieve the public IP address for DC-1 and RPD (Remote Desktop Protocol) into the virtual machine. You will see Server Manager populate on your screen once you login. In this lab we will be disabling the firewall. Click on start and type in the Run command and type in wf.msc (windows firewall) and disable it. Click the Windows Defender Firewall Properties and click off to disable for each tab not including the IPsec Settings.
</p>
<br />

<p>

  ![image](https://github.com/user-attachments/assets/a5b74a66-7aa5-4e41-a111-2cbe51738353)

</p>
<p>
Navigate back to the Azure and retrieve the private IP address for DC-1. Proceed to navigate into CLIENT-1's network settings into the NIC and click DNS Servers, paste DC-1's private IP address into the Add DNS portion. When the computer needs to look up info from sites it will refer to DC-1 which is the reason for this. Click save to make sure the DNS settings have been saved and updated. Go back to virtual machines and click on CLIENT-1 and restart the virtual machine.
</p>
<br />
<p>  

![image](https://github.com/user-attachments/assets/e8e57732-5d12-4027-a33a-a0cb05ddb170)


</p>
<p>
After the restart has been completed, log into CLIENT-1 and ping DC-1's private IP address. To do this, after logging into CLIENT-1 open up PowerShell and ping the private IP address of DC-1 to confirm that it's functioning properly. This shows that the ping has suceeded.

<p>  

![image](https://github.com/user-attachments/assets/d50ff27d-3953-4f80-b308-a02b742a38b9)


</p>
<p>
Now in PowerShell we will run the ipconfig /all command. The output will show the private IP address for DC-1. This validates that the IP address for DC-1 is connected to CLIENT-1.

<p>

These next steps will be based on deploying Active Directory
  
</p>

<p>

![image](https://github.com/user-attachments/assets/35ce110b-cca4-4102-852a-e8d0de0a2bf1)

  
</p>
<p>
RDP into DC-1 and go into the start menu and click on Server Manager. Click add roles and features and proceed to click next until you land on server roles. Click on the Active Directory Domain Services box and click add features. After adding it click next on the boxes until you reach confirmation. Check mark the "restart the destination server automatically" if required box and then click on Install.

![image](https://github.com/user-attachments/assets/8db92c96-160c-4757-bce7-0a868a94cf2c)

Active Directory is now installed!

</p>


<p>


![image](https://github.com/user-attachments/assets/1bafd81b-b259-4af5-9576-192bd039a842)

  
</p>
<p>

Navigate back to the Server Manager dashboard adn click on the flag with the exclamation mark in the top right corner. CLick "Promote This Server to a Domain Controller" and select add a new forest and input mydomain.com. After this is done input your password into the Domain Controller Options and click next. Proceed to click next until you reach the Prerequisites check and click Install.

![image](https://github.com/user-attachments/assets/ac1d25d2-e6c2-42e3-9f9a-12a4c8a30004)

Here we can see that it is being installed. After the installation has been complete you will be signed out automatically due to the virtual machine being turned into a domain controller. You will now need to specify the domain user you are logging into when you RDP back into DC-1. As in this lab the user will log in as mydomain.com\labuser


</p>

<p>

![image](https://github.com/user-attachments/assets/5f1bc1f9-1aed-4a5c-b346-31e2069f15eb)

Open up Active Directory Users and Computers (ADUC) and create an Organizational Unit (OU) called _EMPLOYEES (OU's are folders in Active Directory). Click the start administrative tools and then click on Active Directory Users and Computers. Right click on mydomain.com > New > Organizational Unit. In this lab we will be typing in _EMPLOYEES as the name, after this is done click on "Ok". Create a second OU and name it _ADMINS. For this lab we will be creating an employee in the admins folder. Right click _Admins > New > User and input the name of the employee and create the account. 


![image](https://github.com/user-attachments/assets/ccac9ba5-8c72-4d50-b06b-bdbbe5cb00ca)

To give your employee administrative abilities. Right click on their name and select Properites > Member Of > Click Add > Type Domain Admins then hit apply and OK for it to sync. 

Log out and close out the connection to DC-1 and log back in as your employee using their username in combination with mydomain.com\

</p>

<p>

![image](https://github.com/user-attachments/assets/7d8e6aea-d8af-4102-8070-9b2229bad3bf)

In this step we will join CLIENT-1 to the domain.

Open up the CLIENT-1 virtual machine and open up Settings and then click on systems > then About. Once you are in the about page click on Rename this PC (advanced). Click on Change > Click Domain and type in mydomain.com in the Member of area. A window saying Computer Name/Domain Changes will pop up. Click on cancel as the DNS settings were set up to use DC-1's private IP address. This way it can automatically look up the Domain Controller for the mydomain.com domain. Another Computer Name/Domain change window will appear. In this you can input the mydomain.com\ with your admin user name. 

  ![image](https://github.com/user-attachments/assets/6c256173-2c65-454c-8403-a88a141fe6c9)

  A pop up will show indicating a successful completion of the domain name change. After closing it out, your computer will restart which indicates CLIENT-1 is a member of the domain. 

  </p>

  <p>

![image](https://github.com/user-attachments/assets/52dfee26-f60a-487a-baa8-345ee150876e)

  For the final step log back into DC-1 and open up Active Directory Users and Computers to verify that CLIENT-1 is apart of the system. Click on the arrow next to mydomain.com and click on the Computers folder. Once that you see it's there it will verify that CLIENT-1 is apart of the domain. To create users for Active Directory you can create a new OU called _CLIENTS. Drag the CLIENT-1 computer from the Computers folder into the newly created _CLIENTS. 
  </p>

</p>
<p>

This will be the wrap up for deploying and configuring Active Directory.
