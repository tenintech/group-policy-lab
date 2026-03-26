<h1>Group Policy Management & Shared Resources</h1>
<h2>Objective</h2>
In this lab, I configured Group Policy to control parts of the user desktop environment and improve security. Desktop icons were removed to create a more standardized workspace for users. I also created a shared folder on the domain controller and deployed a Group Policy that automatically maps a network drive when users log in, allowing centralized access to company files.


<h2>Technologies/Environments Used</h2>
  
  - Microsoft Azure(Virtual Machines, Vitual Network)
    
       - Windows Server 2025 (Domain Controller)
       - Windows 10 (Client Machine)
         
  - Active Directory Domain Services (AD DS)
  
  - Group Policy Management Console (GPMC)

  - Server Message Block (SMB) File Sharing
  

<br />



<h2>Group Policy Configurations</h2>

<h3>1. Account Lockout Policy</h3>

Open Group Policy Management.

Create a new Group Policy Object (GPO) to apply basic domain settings.

Example configuration:
- Password policy
- Account lockout policy
- Desktop restrictions

Link the policy to the appropriate Organizational Unit.

Verify the policy applies to domain users.

<img width="1223" height="834" alt="19using GPMC to create GPO for Password Threshold" src="https://github.com/user-attachments/assets/d99b1fe3-fadc-4506-8a83-f57fc6c54e9b" />

After setting the password threshold I attempted to login as user "bot.vug" with the wrong password 10 times and was locked out of the account. 
Unlock the account
Reset the password


<img width="1235" height="902" alt="23Unlocking user account" src="https://github.com/user-attachments/assets/2ac6be79-4175-45c7-a1d2-8642f4e9a1d0" />

<h4>Desktop Restriction Policy</h4>

To simulate a managed work environment, a desktop restriction policy was created to prevent standard users from accessing certain icons on the desktop and system settings.

Steps:
- Open Group Policy Management
- Right Click Group Policy Objects 
- Create New GPO
      - Name: Desktop Restrictions Policy
- Navigate to:

User Configuration → Administrative Templates → Desktop
<img width="713" height="406" alt="EnablingDesktopRestrictions" src="https://github.com/user-attachments/assets/6b9ac319-26ee-489b-8e4e-3dc8b815393b" />


Enable the policy:
Remove Computer Icon, Properties and Recycle Bin Icon

Apply the policy to the _EMPLOYEES Organizational Unit.

<img width="655" height="409" alt="linkpolicyto OU" src="https://github.com/user-attachments/assets/9dc1ba05-1e47-4f93-af73-ae37bc03c171" />

Test the policy:
- Log into Client1 as a standard user
- Observe the absence of icons and properties menu.

Result:
Icons no longer visible
<h3>1. Removed system icons from the desktop using Group Policy</h3>




<hr />

<br />

<h3>2. Restrict Access to Control Panel</h3>

Security hardening
User environment control


<hr />

<br />

<h3>3. Map a Network Drive Automatically</h3>

Configured a Group Policy Object to automatically map a shared company drive for all domain users at login.

1. Shared Folder Created on Server


    <img width="365" height="424" alt="1  shared folder created " src="https://github.com/user-attachments/assets/084e9429-c6cb-41a9-9e18-914fb2979f25" />
<br />


3. Folder Sharing and Permissions


    <img width="392" height="416" alt="2  Permissions" src="https://github.com/user-attachments/assets/aae9e12f-cebf-4b83-af18-65d5d67a6c5e" />
<br />


4. Drive Map GPO configuration


   <img width="686" height="428" alt="5  New mapped drive" src="https://github.com/user-attachments/assets/5c894b0c-946b-4d13-9444-7ccc502823a2" />
<br />


5. Universal Naming Convention (UNC) Path Configuration


   <img width="302" height="365" alt="6  Create mapped drive" src="https://github.com/user-attachments/assets/f98d3a7f-360f-4739-9ef6-17f8ec06f39f" />
<br />


6. Link GPO to the OU (_EMPLOYEES)


   <img width="440" height="413" alt="6 Link an existing GPO" src="https://github.com/user-attachments/assets/678fedc8-1f5e-4be5-abd6-b341d16bd99a" />

  
  


<hr />

<br />



## What I Learned
How organizations provide shared file access to users using Group Policy
The difference between share permissions and NTFS permissions
How Group Policy Objects are linked to Organizational Units (OUs)
How to verify and troubleshoot policy deployment on client machines
The importance of centralized management in Active Directory environments

<h2>⏭️Next Steps</h2>

In the next phase of this lab I'll:
- Install Active Directory Domain Services (AD DS)
- Promote the server to a Domain Controller
- Join Client1 to the domain
- Create test user accounts
