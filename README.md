# Group Policy Management & Shared Resources

---

## Objective
In this lab, I configured Group Policy to manage the user desktop environment and improve security within a domain environment. I implemented desktop restrictions to create a standardized workspace and prevent access to certain system features.

Additionally, I created a shared folder on the domain controller and deployed a Group Policy Object (GPO) to automatically map a network drive when users log in, allowing centralized access to company resources.

---

## Technologies / Environments Used

- Microsoft Azure
  - Virtual Machines
  - Virtual Network
- Windows Server 2025 (Domain Controller)
- Windows 10 (Client Machine)
- Active Directory Domain Services (AD DS)
- Group Policy Management Console (GPMC)
- Server Message Block (SMB) File Sharing

---

## Key Group Policy Configurations

## 1. Account Lockout Policy

Created a Group Policy Object (GPO) to enforce password attempt limits and account lockout settings.

### Actions Performed
- Created a new GPO using Group Policy Management Console
- Configured password threshold settings
- Applied the policy to domain users
- Tested the policy by attempting multiple failed logins
- Verified that the account locked after the threshold was reached
- Unlocked the account and reset the password in Active Directory

<img width="1223" height="834" alt="19using GPMC to create GPO for Password Threshold" src="https://github.com/user-attachments/assets/fdd75e56-b07b-43cf-8abb-f619f4c3bd56" />

After configuring the policy, I attempted to log in as the user **bot.vug** with an incorrect password multiple times until the account was locked.

![Unlocking user account](https://github.com/user-attachments/assets/2ac6be79-4175-45c7-a1d2-8642f4e9a1d0)

---

## 2. Desktop Restriction Policy

To simulate a managed corporate environment, I created a policy to remove certain system icons from the desktop for standard users.

### Policy Configuration Path
User Configuration → Administrative Templates → Desktop

### Policies Enabled
- Remove Recycle Bin icon
- Remove Computer icon
- Remove Properties option

![Desktop restriction settings](https://github.com/user-attachments/assets/6b9ac319-26ee-489b-8e4e-3dc8b815393b)

The policy was linked to the **_EMPLOYEES Organizational Unit (OU)**.

![Link policy to OU](https://github.com/user-attachments/assets/9dc1ba05-1e47-4f93-af73-ae37bc03c171)

### Result
When logging into the client machine as a standard domain user, the restricted desktop icons were no longer visible.

---

## 3. Restrict Access to Control Panel

To further improve security, I implemented a Group Policy that prevents users from accessing the Control Panel.

### Steps

Created a new GPO named:

**Restrict Control Panel Access**

![Naming GPO](https://github.com/user-attachments/assets/eaed2285-1d87-45ea-a15f-7f447af27018)

Enabled the policy under:

User Configuration → Administrative Templates → Control Panel  
**Prohibit access to Control Panel and PC settings**

![Policy enabled](https://github.com/user-attachments/assets/53c248a7-f0c3-4a69-a9aa-ce5de618e91a)

Linked the policy to the **_EMPLOYEES OU**

![Link policy to OU](https://github.com/user-attachments/assets/78c9d2b7-15c8-4242-b172-b5b4755da477)

---

## 4. Automatically Map a Network Drive



I created a shared company folder on the domain controller and used Group Policy Preferences to automatically map a network drive for users when they log in.



### Shared Folder Created on Server



![Shared folder created](https://github.com/user-attachments/assets/084e9429-c6cb-41a9-9e18-914fb2979f25)



### Folder Sharing and Permissions


![Permissions configured](https://github.com/user-attachments/assets/aae9e12f-cebf-4b83-af18-65d5d67a6c5e)

### Drive Map GPO Configuration


![Mapped drive configuration](https://github.com/user-attachments/assets/5c894b0c-946b-4d13-9444-7ccc502823a2)



### UNC Path Configuration

UNC path used:

Configured the drive using the server's shared folder path.

\\DCMachine\CompanyFiles


<img width="302" height="365" alt="UNC path configuration" src="https://github.com/user-attachments/assets/f98d3a7f-360f-4739-9ef6-17f8ec06f39f" />


Link GPO to the _EMPLOYEES OU


<img width="440" height="413" alt="Link GPO" src="https://github.com/user-attachments/assets/678fedc8-1f5e-4be5-abd6-b341d16bd99a" />


Result on Client Machine (Most Important Test)
When users log in, the network drive automatically appears in File Explorer as:

  Z: Company Files

This confirms the Group Policy successfully deployed and the shared resource is accessible to domain users.



## What I Learned
Difference between Share Permissions and NTFS Permissions
How Group Policy Objects are linked and applied to Organizational Units
How to test and verify policy deployment on client machines
The importance of centralized management in enterprise IT environments

