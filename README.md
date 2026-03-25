<h1>Preparing Active Directory Infrastructure in Microsoft Azure</h1>
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



<h2>Step-by-Step Walkthrough</h2>

<h3>1. Removed system icons from the desktop using Group Policy</h3>

Within Microsoft Azure, deploy a Windows Server Virtual Machine that will serve as the Domain Controller.

Configuration:
- Virtual Machine Name: DC1
- Image: Windows Server 2025
- Virtual Network: Named "VNet10" Same network that will be used by the client machine

<img width="900" height="1000" alt="Created Domain Controller VM" src="https://github.com/user-attachments/assets/2e95058d-039e-450f-80d1-d68e1015592e" />

<hr />

<br />

<h3>2. Create the Client Virtual Machine</h3>

Create another Windows Virtual Machine that will act as a client computer in the domain.

Configuration:
- Virtual Machine Name: Client1
- Image: Windows 10 Enterprise
- Virtual Network: Same VNet as the Domain Controller (VNet10)

<img width="1920" height="1080" alt="Created Client VM" src="https://github.com/user-attachments/assets/f1cfc3f4-1d6a-4dad-88f2-5dd9b40c8fe1" />

<hr />

<br />

<h3>3. Configure a Static Private IP Address for the Domain Controller</h3>

Set the Domain Controller's Network Interface Card (NIC) to use a static private IP address.  
This ensures the IP address does not change and allows the client machine to reliably use the Domain Controller as its DNS server.

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
During this lab I learned how important DNS configuration is when setting up Active Directory in Azure. Matching the IP Address to the same network is critical for ensuring direct, secure, and reliable communication for Active Directory services. Otherwise, network traffic may be blocked, misrouted or fail to resolve.

<h2>⏭️Next Steps</h2>

In the next phase of this lab I'll:
- Install Active Directory Domain Services (AD DS)
- Promote the server to a Domain Controller
- Join Client1 to the domain
- Create test user accounts
