<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial shows the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Active Directory Domain Services
- Group Policy

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2> High-Level Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin User with the Domain
- Step 3: Join the Client to Domain 
- Step 4: Setup Group Policy for non-administrative users
- Step 5: Create a user and login to the Client 


<h2>Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory

<img width="539" alt="Screenshot 2025-04-21 at 3 54 16 PM" src="https://github.com/user-attachments/assets/6db2805e-0851-48bb-bb43-014f4b4ec053" />

First let's get logged into the Domain (Windows Server) virtual machine. Next, go to Server Manager and click add roles and features.

![image](https://github.com/user-attachments/assets/9668cb37-c0ee-41a2-92a9-b3d5ae3e6b2d)

Click next until you get to Server roles. Check Active Directory Domain Services and then click Add Features. Click next until you can select install and close the window after installation.

<img width="898" alt="Screenshot 2025-04-21 at 4 09 34 PM" src="https://github.com/user-attachments/assets/1d0e3d16-b5b5-45c6-b7aa-1215edd0c2b4" />

In Server Manager click the flag icon at the top. Then click Promote this server to a Domain Controller. 

<img width="750" alt="Screenshot 2025-04-21 at 4 54 12 PM" src="https://github.com/user-attachments/assets/005984c1-b9a7-4274-97c6-bf5d18f9a0d8" />

Select add a new forest option and make the root domain: mydomain.com. Then click next and set a DSRM password, after that click next until you can select install. After installation the VM should automatically restart. 

- Step 2: Create a Domain Admin User with the Domain

![image](https://github.com/user-attachments/assets/3c9da7dc-e81c-492c-a586-0683a26cd46a)

Log back into the Domain VM as the original admin (for the rest of the tutorial don't use these admin credentials), including the root domain: mydomain.com.

<img width="959" alt="Screenshot 2025-04-21 at 5 17 32 PM" src="https://github.com/user-attachments/assets/793a40a1-86eb-4a88-9cb8-7be90e451386" />

Go to Active Directory Users and Computers, right click mydomain.com, then click New and Organizational Unit.

![image](https://github.com/user-attachments/assets/b9a1af2e-7bb4-4583-a572-3e150c985035)

Create an Organizational Unit named _EMPLOYEES and another one named _ADMINS.

<img width="655" alt="Screenshot 2025-04-21 at 5 24 41 PM" src="https://github.com/user-attachments/assets/af9ac196-b1a0-49ca-b9c7-73cf001642ed" />

Create an admin user and password. 

<img width="686" alt="Screenshot 2025-04-21 at 5 26 29 PM" src="https://github.com/user-attachments/assets/42fec3ed-6fa9-4e63-944c-9dc9315b1b92" />

Right click the user you just created and select Properties.

<img width="669" alt="Screenshot 2025-04-21 at 5 27 11 PM" src="https://github.com/user-attachments/assets/c608e183-62e3-4600-b769-0be2cd78a93c" />

Click the Member Of tab and click Add. Type Domain Admins, select Check names, and select OK, apply, and OK.

<img width="430" alt="Screenshot 2025-04-21 at 5 37 20 PM" src="https://github.com/user-attachments/assets/6893aa68-ec44-4b5f-a755-9d5dcd54ed95" />

Now log out of the VM and log back in as the admin user we just created using their user log on name. Continue to use this login for logging into the Domain VM for the rest of the tutorial.

- Step 3: Join the Client to the Domain

![Screenshot 2025-04-21 at 8 06 53 PM](https://github.com/user-attachments/assets/41e782d5-f87b-49d9-abca-eb3483f6eac2)

On the Microsoft Azure website go into the Client (Windows 10) VM. Select Networking settings and the virtual NIC.

<img width="1295" alt="Screenshot 2025-04-21 at 8 10 52 PM" src="https://github.com/user-attachments/assets/43dfa231-f26f-4773-95b9-7e17039d8b3d" />

Click DNS Servers and choose custom. Type the Domain VMs private IP address and click save. Restart the Client VM and get logged into it using the admin credentials.

- Step 3: Join the Client to the Domain

<img width="637" alt="Screenshot 2025-04-21 at 8 18 51 PM" src="https://github.com/user-attachments/assets/94103bec-a0e5-421e-9a23-3587ef6cd464" />

Go to System in Settings and scroll down and click Rename this PC (advanced)

<img width="721" alt="Screenshot 2025-04-21 at 8 22 59 PM" src="https://github.com/user-attachments/assets/c41f6580-25c4-4658-91e3-20c1f783d32e" />

Now select change, click the Member Of tab and type mydomain.com and click ok, apply, and then ok. Then enter in the admin credentials. 

- Step 4: Setup Group Policy for non-administrative users

<img width="915" alt="Screenshot 2025-04-22 at 2 39 45 PM" src="https://github.com/user-attachments/assets/817afb25-8fe1-4904-a5c5-4e2910e4eb92" />

Log into the domain VM. In the search bar type group and click Group Policy Management. 

<img width="907" alt="Screenshot 2025-04-22 at 2 44 13 PM" src="https://github.com/user-attachments/assets/ff690412-7cdd-40c8-9173-67c7a7e75355" />

Right click on the group policy we just created and select edit. Click Computer Configuration > Preferences > Control Panel Settings > Local Users and Groups.

<img width="760" alt="Screenshot 2025-04-22 at 2 45 43 PM" src="https://github.com/user-attachments/assets/73188c4f-4fe0-40a5-944d-82acba9002a4" />

Right click and select New and Local Group.

<img width="697" alt="Screenshot 2025-04-22 at 2 49 30 PM" src="https://github.com/user-attachments/assets/a96a62ab-2812-4cf1-8f9e-5d5fc12ed6f0" />

Make sure the action is Update. For the Group Name select Remote Desktop Users (built-in) and give it a description. I named the group policy, RDP access for Domain Users and select ok.

<img width="631" alt="Screenshot 2025-04-22 at 2 54 33 PM" src="https://github.com/user-attachments/assets/312a99bc-e20c-4fab-9cf1-e2a913453e9b" />

Click Add and the ... next to the name bar. Select ok, ok, apply, and then ok.

<img width="700" alt="Screenshot 2025-04-22 at 2 55 08 PM" src="https://github.com/user-attachments/assets/058bb22c-148d-4998-8b59-e9c43a15d385" />

Type Domain users and Check Names.

<img width="582" alt="Screenshot 2025-04-22 at 3 12 41 PM" src="https://github.com/user-attachments/assets/0ac9a7f3-ce4a-4bc9-ae86-360a83f451cf" />

Login to the Client VM as the admin and get into Windows Powershell. Run gpupdate /force to update the group policy.

- Step 5: Create a user and login to the Client VM

<img width="758" alt="Screenshot 2025-04-22 at 2 16 58 PM" src="https://github.com/user-attachments/assets/8f59e3f8-a182-4fd7-b660-c27e96c7eab5" />

Login to the Domain VM as the admin user. Go to Active Directory Users and Computers and create a user within our  _EMPLOYEES organizational unit and set a password. 

<img width="433" alt="Screenshot 2025-04-22 at 2 11 13 PM" src="https://github.com/user-attachments/assets/67068b50-f4d1-47fa-96f0-5a3cd78d280d" />

Now try to log into the Client VM with their user log on name.

<img width="1440" alt="Screenshot 2025-04-22 at 2 18 15 PM" src="https://github.com/user-attachments/assets/99c037d0-771a-4610-9326-5111063b11f1" />

And it should work!

<br />
