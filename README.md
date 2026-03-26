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

<h3>1. Configure Basic Group Policy</h3>

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

<h4>Apply a Desktop Restriction Policy</h4>

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

Within Microsoft Azure, deploy a Windows Server Virtual Machine that will serve as the Domain Controller.

Configuration:
- Virtual Machine Name: DC1
- Image: Windows Server 2025
- Virtual Network: Named "VNet10" Same network that will be used by the client machine

<img width="900" height="1000" alt="Created Domain Controller VM" src="https://github.com/user-attachments/assets/2e95058d-039e-450f-80d1-d68e1015592e" />

<hr />

<br />

<h3>2. Restrict Access to Control Panel</h3>

Security hardening
User environment control

Configuration:
- Virtual Machine Name: Client1
- Image: Windows 10 Enterprise
- Virtual Network: Same VNet as the Domain Controller (VNet10)

<img width="1920" height="1080" alt="Created Client VM" src="https://github.com/user-attachments/assets/f1cfc3f4-1d6a-4dad-88f2-5dd9b40c8fe1" />

<hr />

<br />

<h3>3. Map a Network Drive Automatically</h3>

Configured a Group Policy Object to automatically map a shared company drive for all domain users at login.

 - 1. Shared Folder Created on Server
      <img width="365" height="424" alt="1  shared folder created " src="https://github.com/user-attachments/assets/084e9429-c6cb-41a9-9e18-914fb2979f25" />

- 2. Folder Sharing and Permissions
     <img width="392" height="416" alt="2  Permissions" src="https://github.com/user-attachments/assets/aae9e12f-cebf-4b83-af18-65d5d67a6c5e" />

- 3. Drive Map GPO configuration
     <img width="686" height="428" alt="5  New mapped drive" src="https://github.com/user-attachments/assets/5c894b0c-946b-4d13-9444-7ccc502823a2" />

- 4. Universal Naming Convention (UNC) Path Configuration
     <img width="302" height="365" alt="6  Create mapped drive" src="https://github.com/user-attachments/assets/f98d3a7f-360f-4739-9ef6-17f8ec06f39f" />

- 5. Link GPO to the OU (_EMPLOYEES)
     <img width="440" height="413" alt="6 Link an existing GPO" src="https://github.com/user-attachments/assets/678fedc8-1f5e-4be5-abd6-b341d16bd99a" />

  
  

<img width="1920" height="1080" alt="Configuring Static IP Address" src="https://github.com/user-attachments/assets/129a2b34-0409-4c4b-b4f2-437b7fc79510" />

<hr />

<br />

<h3>4. Connect to the Domain Controller Using Remote Desktop</h3>

Log into the Domain Controller (DC1) using Remote Desktop Protocol (RDP).

To verify connectivity between machines, temporarily disable the firewall and allow ICMP traffic for testing.

Run:
wf.msc

This opens the Windows Defender Firewall management console.

<img width="1920" height="1080" alt="Connecting via Remote Desktop" src="https://github.com/user-attachments/assets/e8894acc-6bb7-4411-bba3-00da07647754" />

<img width="1272" height="984" alt="Disabling Firewall for Testing" src="https://github.com/user-attachments/assets/0fb3ecf4-b5db-47c4-98de-aa5da97eb6a3" />

<hr />

<br />

<h3>5. Configure Client DNS Settings</h3>

Set the DNS server of the client virtual machine (Client1) to point to the private IP address of the Domain Controller (DC1).

This allows the client machine to locate and authenticate with the domain once Active Directory is installed.

<img width="1076" height="996" alt="Changing DNS Settings on Client VM" src="https://github.com/user-attachments/assets/64087d13-0377-48f8-8104-4e5e3fd8c581" />

<hr />

<br />

<h3>6. Test Network Connectivity</h3>

Open PowerShell on Client1 and test connectivity to the Domain Controller using the ping command.

Use:
ping 10.0.0.4

Then verify DNS configuration using:
ipconfig /all

This confirms that the client machine is using the Domain Controller as its DNS server.

<img width="951" height="967" alt="Successful Ping Test" src="https://github.com/user-attachments/assets/e72a658e-4c85-41d4-ab91-57dc85b4d0e8" />

<img width="1080" height="925" alt="DNS Configuration Confirmation" src="https://github.com/user-attachments/assets/80e8d3d8-04b1-43d2-91d9-876d1890156b" />

<hr />

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
