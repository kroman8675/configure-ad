<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial shows the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Active Directory Domain Services
- Group Policy

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory
- Step 2: Create a Domain Admin User with the Domain
- Step 3: Join the Client to Domain
- Step 4: Setup Group Policy for non-administrative users

<h2>Deployment and Configuration Steps</h2>

- Step 1: Install Active Directory

<img width="539" alt="Screenshot 2025-04-21 at 3 54 16 PM" src="https://github.com/user-attachments/assets/6db2805e-0851-48bb-bb43-014f4b4ec053" />

The first thing to do is to get logged into the Domain (Windows Server) virtual machine. Next, go to Server Manager and click add roles and features.

![image](https://github.com/user-attachments/assets/9668cb37-c0ee-41a2-92a9-b3d5ae3e6b2d)

Click next until you get to Server roles. Then check Active Directory Domain Services and click Add Features. Then click next until you can select install and close the window after installation

<img width="898" alt="Screenshot 2025-04-21 at 4 09 34 PM" src="https://github.com/user-attachments/assets/1d0e3d16-b5b5-45c6-b7aa-1215edd0c2b4" />

In Server Manager click the flag icon at the top. Then click Promote this server to a Domain Controller. 

<img width="750" alt="Screenshot 2025-04-21 at 4 54 12 PM" src="https://github.com/user-attachments/assets/005984c1-b9a7-4274-97c6-bf5d18f9a0d8" />

Select the add a new forest option and make the root domain: mydomain.com. Then click next and set a DSRM password, after that click next until you can select install. After installation the VM should automatically restart. 

- Step 2: Create a Domain Admin User with the Domain

![image](https://github.com/user-attachments/assets/3c9da7dc-e81c-492c-a586-0683a26cd46a)

Log back into Domain VM as the user including the root domain: mydomain.com\Domain

<img width="959" alt="Screenshot 2025-04-21 at 5 17 32 PM" src="https://github.com/user-attachments/assets/793a40a1-86eb-4a88-9cb8-7be90e451386" />

Go to Active Directory Users and Computers, right click our mydomain.com, then click new and click Organizational Unit.

![image](https://github.com/user-attachments/assets/b9a1af2e-7bb4-4583-a572-3e150c985035)

Create and Organizational Unit named _EMPLOYEES and another one named _ADMINS.

<img width="655" alt="Screenshot 2025-04-21 at 5 24 41 PM" src="https://github.com/user-attachments/assets/af9ac196-b1a0-49ca-b9c7-73cf001642ed" />

Create an admin user and set their password. 

<img width="686" alt="Screenshot 2025-04-21 at 5 26 29 PM" src="https://github.com/user-attachments/assets/42fec3ed-6fa9-4e63-944c-9dc9315b1b92" />

Right click the user you just created and select Properties.

<img width="669" alt="Screenshot 2025-04-21 at 5 27 11 PM" src="https://github.com/user-attachments/assets/c608e183-62e3-4600-b769-0be2cd78a93c" />

Then click Member Of and click Add. Type Domain Admins, select Check names, and then select OK,apply,and OK again. 

<img width="430" alt="Screenshot 2025-04-21 at 5 37 20 PM" src="https://github.com/user-attachments/assets/6893aa68-ec44-4b5f-a755-9d5dcd54ed95" />

Now log out of the VM and log back in as that admin user using their user log on name. 

- Step 3: Join the Client to the Domain

![Screenshot 2025-04-21 at 8 06 53 PM](https://github.com/user-attachments/assets/41e782d5-f87b-49d9-abca-eb3483f6eac2)

On the Microsoft Azure website go into the Windows 10 client VM and select Networking settings. Then select the virtual NIC.

<img width="1295" alt="Screenshot 2025-04-21 at 8 10 52 PM" src="https://github.com/user-attachments/assets/43dfa231-f26f-4773-95b9-7e17039d8b3d" />

Click DNS Servers. Then select the custom option and type the Domain VMs private IP and click save. Restart the Client VM and get logged into it.

- Step 4: Setup Group Policy for non-administrative users

<img width="637" alt="Screenshot 2025-04-21 at 8 18 51 PM" src="https://github.com/user-attachments/assets/94103bec-a0e5-421e-9a23-3587ef6cd464" />

Go to System in Settings and scroll down and click rename this pc (advanced)

<img width="721" alt="Screenshot 2025-04-21 at 8 22 59 PM" src="https://github.com/user-attachments/assets/c41f6580-25c4-4658-91e3-20c1f783d32e" />

Now select change, click member of and type mydomain.com and click ok, apply, ok. Then enter in the admin credentials. 

- Step 3: Join the Client to the Domain

<img width="959" alt="Screenshot 2025-04-21 at 8 37 34 PM" src="https://github.com/user-attachments/assets/6ca31c1c-d4eb-4ce8-a815-3f102b3755c5" />

Login to the client as the admin user. Type group policy in the search bar and click Edit Group Policy.

<img width="969" alt="Screenshot 2025-04-21 at 8 40 27 PM" src="https://github.com/user-attachments/assets/06d1f7a1-02f3-4486-8b67-5b483742e821" />

Click Windows settings > Local Policies > User Rights Assignment > Allow on Locally

<img width="954" alt="Screenshot 2025-04-21 at 8 49 01 PM" src="https://github.com/user-attachments/assets/ec7b50e6-9d2b-4fa4-a071-bf00aa622e7b" />

Hit add user of group, type in Domain Users, select check names, click ok, apply, and ok.

<br />
