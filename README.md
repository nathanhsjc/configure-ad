<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

## Table of Contents

- [Environments and Technologies Used](#environments-and-technologies-used)
- [Operating Systems Used](#operating-systems-used)
- [High-Level Deployment and Configuration Steps](#high-level-deployment-and-configuration-steps)
- [Deployment and Configuration Steps](#deployment-and-configuration-steps)
- [Practical Applications](#practical-applications)
- [Conclusion](#conclusion)

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used 

- Windows Server 2022
- Windows 10 (21H2)

## High-Level Deployment and Configuration Steps

- Virtual Machine Creation
- Installing Active Directory
- Step 3 Active Directory Configuration
  

## Deployment and Configuration Steps

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

  For the next step log back into DC-1 and open up Active Directory Users and Computers to verify that CLIENT-1 is apart of the system. Click on the arrow next to mydomain.com and click on the Computers folder. Once that you see it's there it will verify that CLIENT-1 is apart of the domain. To create users for Active Directory you can create a new OU called _CLIENTS. Drag the CLIENT-1 computer from the Computers folder into the newly created _CLIENTS. 
  </p>

</p>
<p>
We will setup Remote Desktop for the domain users. We will do this by logging into CLIENT-1 as your admin account (jane_admin in this project that I am using) and navigate into the computers settings. Click on About > Remote Desktop > User Accounts 


![image](https://github.com/user-attachments/assets/d758df98-622b-4541-8ff0-8f31acba8bf6)

</p>

<p>

Next we will click add and type in (Domain Users), click Check Name and then proceed to click "Ok" to finish. Now our domain users can use Remote Desktop and log into this computer and log into CLIENT-1 as a normal non-administrative user


  ![image](https://github.com/user-attachments/assets/35ad63fa-7e02-4ee3-af20-7478c0a45046)

-Image of adding "Domain Users"
</p>
  
  
  ---

## Practical Applications

Once Active Directory (AD) is set up and configured, it can be used in various practical applications. Below are some of the key ways it can be used:

### 1. **User and Group Management**
   - **Create and Manage Users**: Active Directory allows for the central management of user accounts.
    - **Group Management**: Organize users into groups for easier resource management.
#### Instructions:
Log into DC-1 with the public IP address and open up Windows PowerShell ISE as an administrator by ricking clicking on PowerShell. For this project we will be creating 500 users though you are free to create as many users as you want. 

![image](https://github.com/user-attachments/assets/49740be3-d5c1-4579-b4e1-7816884f430d)


</p>

Click create a new script (Paper icon underneath File). There are various scripts you can use. Here is an example script that we will be using for this project that will create the users and add them into the _EMPLOYEES folder in Active Directory. Save the script below by clicking File and saving it as a File. After that is done paste the script into PowerShell
 # ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 500
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}


![image](https://github.com/user-attachments/assets/77e15d90-2510-473e-bc83-d26c942576b3)

This is how it will look when pasted into PowerShell.

</p>

Next run the script by clicking the green arrow icon.

![image](https://github.com/user-attachments/assets/2282bf76-e4cf-40aa-8ca7-f9b7f1abe3e5)


We can observe the script running and users being generated in PowerShell.

All of the users we have created will share the same password so when attempting to login you can use the same password until you configure a password change setting for the users that are generated. 



![image](https://github.com/user-attachments/assets/f333bc0e-f704-4639-8bbd-a5711ff7452c)

This is what the users that are created will look like in Active Directory in the _EMPLOYEES folder that we have created earlier.


### 2. **Access Control**
   - **Role-Based Access Control (RBAC)**: Grant specific roles to users for controlling access to network resources.
   - **Permissions Management**: Control access to shared resources.

#### Instructions

We can assign a role to groups or users by rick clicking and selecting Properties. Here I will show an example on a random user I selected in the _EMPLOYEES folder. I have also created another OU called _TechSupport and added a group within called Technicians. One way to move user's is by clicking on their name and dragging it into the OU. 

![image](https://github.com/user-attachments/assets/c96af2a5-8ad3-41c8-83d9-012beb0263b6)


After creating the _TechSupport OU and created the Technicians group I right on a user clicked Properties and clicked "Add to a group". You will then input the group into the "Enter the object names to select" area.

![image](https://github.com/user-attachments/assets/8a8d0eb8-91cf-456d-b97a-42e189b7ecc6)

Verify the name by clicking "Check Names" and click Ok. The user has been successfully added to the group.


### 3. **Group Policy Management**
   - **Security Policies**: Enforce company-wide security policies like password length, screen lock timers, etc.
   - **Automated Configuration**: Apply system configurations automatically.

---

## Conclusion

This will be the wrap-up for deploying and configuring Active Directory. By implementing AD in this setup, businesses can centralize and secure their user management, enforce security policies, and facilitate easier resource access management across the organization.
