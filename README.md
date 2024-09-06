<h1>Basic Homelab Running Active Directory (VirtualBox) | Add users w/ Powershell</h1>

<h2>Description</h2>
Lab consist of creating Active Directory in Windows Server 2019 and populating with Powershell script
<br />


<h2>Languages and Utilities Used</h2>

- <b>Active Directory</b> 
- <b>PowerShell</b>
- <b>Windows 2019 Server</b>
- <b>Windows 10</b>

<h2>Environments Used </h2>

- <b>Oracle VirtualBox</b>

<h2>Project walk-through:</h2>


<h3>Task A: Setting up Environment</h3>
<img src="https://github.com/user-attachments/assets/a5656faf-9905-423f-8cfe-436a4fbf62be"/>
<br />
<br />

1. The first step involves setting up the Windows 2019 Server and configuring it to match what we planned in the document. After the preliminary installation of installing the iso files to VirtualBox and initial setup we setup our internal IP as the internet facing NIC automatically gets their IP from our home router
<br />
<p align="center">
After determining which network connection is which using the IP address, we renamed them<br/>
<img src="https://github.com/user-attachments/assets/af1f6aec-a3e4-488e-86bf-658024d53de6"/>
<br />
<br />
Renamed PC DC for Domain Controller<br/>
<img src= "https://github.com/user-attachments/assets/882e0508-0bf2-4eca-9069-3bfc22a12658"/>
<br />
<br />
Setting IP4 for internal facing network using the diagram as reference<br/>
<img src= "https://github.com/user-attachments/assets/bac38f04-4956-4aa0-a68a-5797bff17159"/>
<br />
<br />
<br />

  
<h3>Task B: Installing Active Directory Domain Services on the server and create a domain</h3>
1.	Setting up and installing Active Directory Domain Services.
<br />
<p align="center">
Choosing correct server to add it to<br/>
<img src= "https://github.com/user-attachments/assets/ba21fcc1-01c9-43f2-af44-fa59696b41ff"/>
<br />
<br />
Choose Active Directory Domain Services<br/>
<img src= "https://github.com/user-attachments/assets/c14f9ab0-3c76-4665-8372-b05cbb1f7654"/>
<br />
<br />
Software has been installed for Active Directory Services but actual Domain still needs to be created<br/>
<img src= "https://github.com/user-attachments/assets/ea5c7884-8c24-45cd-a020-7d539df33fe0"/>
<br />
<br />
Add new forest and create Root domain name<br/>
<img src= "https://github.com/user-attachments/assets/b1ca428b-01c7-41e4-9e9f-9e747f373d02"/>
<br />
<br />
After restart MYDOMAIN will now show before user<br/>
<img src= "https://github.com/user-attachments/assets/18928983-76cf-4e33-af58-3cd0319030d8"/>
<br />
<br />
  
2.	Next we will create our own dedicated domain admin account rather than our built-in admin account. <br />
<br />
<p align="center">
We follow this path to start the process Start > Windows Administrative Tools > Active Directory Users and Computers<br/>
<img src= "https://github.com/user-attachments/assets/27003022-03cd-40bb-a389-352824f2a07b"/>
<br />
<br />
Next we create an organizational unit to put our admin account in the domain we created<br/>
<img src= "https://github.com/user-attachments/assets/223b7caa-b5fc-4b46-b00f-25a4578fb0dc"/>
<img src= "https://github.com/user-attachments/assets/eccf24ec-9e7f-4274-8ee9-efe87ba4c29c"/>
<br />
<br />
Creating an Admin account in this (‘a-’ to show admin)<br/>
<img src= "https://github.com/user-attachments/assets/f23f2ff8-7a58-472e-bfc2-3d811f74bc09"/>
<br />
<br />
Making the account an actual domain admin Properties > Member of > “Input domain admins into object names”<br/>
<img src= "https://github.com/user-attachments/assets/48ddc3d1-cbd5-45c4-9451-efbf2614997f"/>
<br />
<br />
Sign out and enter newly created admin account<br/>
<img src= "https://github.com/user-attachments/assets/3a730a64-57b1-4d24-8c58-2bd3f33a2c7f"/>
<br />
<br />
<br />


<h3>Task C: Install RAS / NAT</h3>
Allows Window 10 client to be on a private virtual network but still allowing it to access the internet through the domain controller<br/>
<br/>
<p align="center">
Add features: Add Roles and Features > “Check Remote Access” > Install routing<br/>
<img src= "https://github.com/user-attachments/assets/2b9c7c5f-a700-4d6d-9624-23cececead4e"/>
<img src= "https://github.com/user-attachments/assets/42c3f1d1-1467-4fd8-a60a-cf80d17750cb"/>
<br />
<br />
Go to Tools at the top right and then Remote Access<br/>
<img src= "https://github.com/user-attachments/assets/74f37487-593e-4278-8626-4af5d535f4a6"/>
<br />
<br />
Right Click DC (local) > Configure and Enable<br/>
<img src= "https://github.com/user-attachments/assets/c71a994a-d91f-461a-aac4-81ee06828961"/>
<br />
<br />
Install NAT to allow internal clients to connect to the Internet using one public IP address<br/>
<img src= "https://github.com/user-attachments/assets/8ae4de86-0f2e-49e2-8470-653a6a04169e"/>
<br />
<br />
Select _Internet_ for public interface<br/>
<img src= "https://github.com/user-attachments/assets/0bcf73b2-88b2-4214-8bc4-b902b11f505f"/>
<br />
<br />
<br />


<h3>Task D: Setup a DHCP server</h3>
Allow Windows 10 clients to get an IP address that will allow them to browse the internet even when on private internal network<br/>
<br/>
<p align="center">
To add DHCP Server: Dashboard > Add Roles > “Check DHCP Server”<br/>
<img src= "https://github.com/user-attachments/assets/a515f941-68fd-41d4-a244-e69b2b708260"/>
<br />
<br />
Tools at the top right > DHCP<br/>
Create a scope to assign specific IP addresses according to diagram<br/>
<img src= "https://github.com/user-attachments/assets/fb1f5f2e-d062-4491-aea5-71654a69bc83"/>
<br />
<br />
Set lease duration for devices that will only connect temporarily to free IP addresses.<br/>
<img src= "https://github.com/user-attachments/assets/86a08359-25c1-4cc1-ba20-f5e7f4d22f6c"/>
<br />
<br />
<br />


<h3>Task E: Using PowerShell to populate AD</h3>
<br/>
<p align="center">
Downloaded PowerShell script and ran using Windows Powershell ISSE<br/>
<img src= "https://github.com/user-attachments/assets/ee7d2302-d33d-41a9-a793-a1324bcbf9f1"/>
<br />
<br />
Enable scripts to be run in CMD with command Set-ExecutionPolicy Unrestricted<br/>
<img src= "https://github.com/user-attachments/assets/9befc099-15fb-4398-b4f3-e45d3921bde4"/>
<br />
<br />
  
Script Summary: <br/>
Created variables for script; sets all users passwords as Password1 and gets names from txt file<br/>
<img src= "https://github.com/user-attachments/assets/9368e7b4-3c61-4c4f-9098-11740efc17e2"/>
<br />
<br />
Takes plain text passwords and creates as more secure object $password<br/>
<img src= "https://github.com/user-attachments/assets/78d19cc2-cbbd-4181-8189-aed64731d4fa"/>
<br />
<br />
Unchecks box to that prevents accidental object deletion<br/>
<img src= "https://github.com/user-attachments/assets/4af005b1-f6ae-4595-9eac-28935ef31943"/>
<br />
<br />
Runs for creating each individual user in which $n represents the current created account<br/>
<img src= "https://github.com/user-attachments/assets/b82e3097-b0af-4f55-9763-58ba4c018eb8"/>
<br />
<br />
Separates first split by space and takes the first BEFORE element<br/>
<img src= "https://github.com/user-attachments/assets/2551f533-86d1-40b7-b83f-805d6cba38bd"/>
<br />
<br />
Separates split by space and takes first element AFTER space<br/>
<img src= "https://github.com/user-attachments/assets/1c4afaf2-09cf-43fd-a490-3cab62c15ea4"/>
<br />
<br />
Takes character of first name and adds to last name to create username<br/>
<img src= "https://github.com/user-attachments/assets/4b9527d7-6329-466c-bfea-b37648ee7517"/>
<br />
<br />
For verification as it will display current process to us the admin<br/>
<img src= "https://github.com/user-attachments/assets/5a228ae3-a9a8-4cba-98eb-3beeade4e007"/>
<br />
<br />
Makes a new user in AD and sets password, names, ID, password requirements, sets in OU Users, and enables account<br/>
<img src= "https://github.com/user-attachments/assets/75e25724-a205-451a-8e31-47382b8538a4"/>
<br />
<br />
You have to be in the same directory as the file to run it<br/>
<img src= "https://github.com/user-attachments/assets/50d9d6ff-d04a-410c-8d9b-f4cf07e63905"/>
<br />
<br />
<br/>


<h3>Task F: Create windows 10 virtual machine in VirtualBox</h3>
Allow Windows 10 clients to get an IP address that will allow them to browse the internet even when on private internal network<br/>
<br/>
<p align="center">
Go to VirtualBox dashboard and create new “Client1”, 4096 MB memory; change client1 settings to bidirectional clipboard, and drag’n drop, increase processors and switch network to internal and then run machine and run iso<br/>
<img src= "https://github.com/user-attachments/assets/0de66894-48c2-47f6-a262-33dcf1ec33e3"/>
<br />
<br />
Ensure connection is found with ipconfig and test ping www.google.com<br/>
<img src= "https://github.com/user-attachments/assets/80de60fe-0cb6-4a18-8db4-85f1f55457c7"/>
<br />
<br />
Advance Rename computer to Client1and set Domain to mydomain.com<br/>
<img src= "https://github.com/user-attachments/assets/b559c66a-7996-4cd2-9eda-ec5b9cd4c7a3"/>
<br />
<br />
You can see newly created computer in address leases in DHCP<br/>
<img src= "https://github.com/user-attachments/assets/a858ffc8-4188-43f8-8520-a75883a33e6c"/>
<br />
<br />



  
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
