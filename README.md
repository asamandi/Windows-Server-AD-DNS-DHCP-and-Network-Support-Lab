## Windows Server AD, DNS, DHCP, and Network Support Lab

### Overview
- This lab demonstrates a small office network environment using Windows Server and a Windows client.
- The goal of this lab is to practice Network Technician tasks such as IP configuration, DNS resolution, DHCP services, Active Directory domain connectivity, shared folder access, Group Policy verification, and basic troubleshooting.
- This lab shows hands-on experience with Windows Server network services and client/server communication in a domain environment.

### Lab Topology

DC1
- Windows Server
- Role: Domain Controller, DNS Server, DHCP Server

PC1
- Windows Client
- Role: Domain-joined workstation

Domain Name:
- amir.local

Network:
- 192.168.10.0/24

Virtual Network:
- VirtualBox Internal Network: AD-Lab

-----------------------------------------------------------
### IP Addressing

DC1

Role:
- Domain Controller, DNS Server, DHCP Server

IP Address:
- 192.168.10.10

DNS Server:
- 192.168.10.10

PC1

Role:
- Domain Client Workstation

IP Address:
- 192.168.10.20

DNS Server:
- 192.168.10.10

--------------------------------------------------------
### Lab Objectives
- Configure Windows Server with a static IP address
- Install Active Directory Domain Services
- Promote Windows Server to a Domain Controller
- Create the domain amir.local
- Configure DNS for internal name resolution
- Configure DHCP for client IP assignment
- Create Organizational Units, users, and security groups
- Join a Windows client to the domain
- Move the client computer object to the Workstations OU
- Configure shared folder access using group-based permissions
- Create and link a basic Group Policy Object
- Verify connectivity, DNS, domain login, shared folder access, and Group Policy application
- Document troubleshooting steps and verification commands

------------------------------------------------------
### Technologies Used
- Windows Server
- Windows Client
- Active Directory Domain Services
- DNS Server
- DHCP Server
- Group Policy Management
- Shared Folder Permissions
- NTFS Permissions
- Command Prompt
- Oracle VirtualBox

------------------------------------------------------
### Skills Demonstrated
- Static IP configuration
- Client/server network connectivity
- DNS configuration and troubleshooting
- DHCP scope configuration
- Active Directory domain setup
- Domain client join
- User and group management
- Organizational Unit management
- Shared folder access control
- Group Policy creation and verification
- Windows network troubleshooting
- Command-line verification using ping, ipconfig, nslookup, gpupdate, and gpresult

-------------------------------------------------------
### Active Directory Structure

Domain:
- amir.local

Organizational Units:
- IT
- HR
- Sales
- Workstations
- Disabled Users

--------------------------------------------------------
### Users Created

IT OU:
- ali.it
- sara.admin

HR OU:
- john.hr

Sales OU:
- mike.sales

Lab Password Used:
- P@ssw0rd123!

Note:
- This password was used only for lab testing.

---------------------------------------------------------
### Security Groups Created

IT-Admins
Members:
- ali.it
- sara.admin

Purpose:
- IT department security group.

HR-Users
Members:
- john.hr

Purpose:
- HR department security group.

Sales-Users
Members:
- mike.sales

Purpose:
- Sales department security group.

Shared-Folder-Access
Members:
- ali.it
- john.hr
- mike.sales

Purpose:
- Controls access to the shared folder.

-------------------------------------------------------------
### DNS Configuration

DNS Server:
- DC1

DNS Server IP:
- 192.168.10.10

Forward Lookup Zone:
- amir.local

Purpose:
- DNS allows domain clients to locate the domain controller and resolve internal hostnames such as DC1 and amir.local.

Network Support Note:
- If a client cannot join the domain, DNS should be one of the first things checked.

----------------------------------------------------------
### DHCP Configuration

DHCP Server:
- DC1

Scope Name:
- Office-LAN-Scope

Network:
- 192.168.10.0/24

DHCP Range:
- 192.168.10.100 - 192.168.10.200

Default Gateway:
- 192.168.10.1

DNS Server:
- 192.168.10.10

DNS Domain:
- amir.local

Purpose:
- The DHCP scope was configured to provide automatic IP addressing for client computers in the lab network.

Network Support Note:
- If a client receives an APIPA address such as 169.254.x.x, DHCP is not working correctly.

--------------------------------------------------------------
### Shared Folder Configuration

Server:
- DC1

Folder Path:
- C:\CompanyShare

-------------------------------------------------------------
### Shared Folder Configuration

Server Information
- Server: DC1
- Folder Path: C:\CompanyShare
- Network Path: \DC1\CompanyShare
- Access Group: Shared-Folder-Access

Share Permissions
- Read
- Change
- NTFS Permissions

Modify
- Read & Execute

List Folder Contents
- Read
- Write

Purpose
- This shared folder was configured to test domain user access to network resources using Active Directory security groups.
- The goal was to verify that domain users can access a shared network resource from a domain-joined client computer.

Network Support Notes
If a user cannot access the shared folder, the following items should be checked:
- Network connectivity
- DNS resolution
- Share permissions
- NTFS permissions
- Security group membership
- Domain login status

--------------------------------------------------------------------
### Group Policy Configuration

GPO Information
- GPO Name: Workstation Security Policy

Linked OUs:
- Workstations

IT
Configured Settings
- Prohibit access to Control Panel and PC settings: Enabled
- Minimum password length: 8
- Password complexity: Enabled

Purpose
- This Group Policy was configured to practice basic workstation and user policy management in a Windows domain environment.
- The policy was used to verify that domain settings can be applied and confirmed from a client computer.

Important Note
- The GPO was linked to the Workstations OU for computer-based settings.
- The GPO was also linked to the IT OU for user-based settings.
- User Configuration settings apply to user accounts.
- Computer Configuration settings apply to computer accounts.

---------------------------------------------------------
### Verification Commands

DC1 Verification
- hostname
- ipconfig /all
- nslookup amir.local
- net user /domain
- net group /domain

PC1 Verification
- ipconfig /all
- ping 192.168.10.10
- ping DC1
- nslookup amir.local
- whoami
- gpupdate /force
- gpresult /r

net use
- \DC1\CompanyShare

-------------------------------------------------------------------------
### Connectivity Verification

Ping Test

Command:
- ping 192.168.10.10

Expected Result:
- Reply from 192.168.10.10

Purpose:
- This confirms that PC1 can reach DC1 over the lab network.

DNS Resolution Test

Command:
- nslookup amir.local

Expected Result:
- Server: dc1.amir.local
- Address: 192.168.10.10

Purpose:
- This confirms that PC1 is using DC1 as the DNS server and can resolve the Active Directory domain.

Domain User Test

Command:
- whoami
- Expected Result:
- amir\ali.it

Purpose:
- This confirms that the user is logged in with a domain account.

----------------------------------------------------------------
### Troubleshooting Scenarios

### Problem 1: PC1 Could Not Reach DC1

Issue:
- PC1 could not ping DC1.

Root Cause:
- PC1 was initially connected to the wrong VirtualBox network adapter type.

Fix:
- Changed PC1 from Bridged Adapter to Internal Network.
- Used the same internal network name on both DC1 and PC1.

Correct Network Name:
- AD-Lab

Verification:
- PC1 successfully pinged DC1 at 192.168.10.10.

-------------------------------------------------
### Problem 2: PC1 Received APIPA Address

Issue:
- PC1 received a 169.254.x.x address.

Root Cause:
- PC1 did not receive an IP address from the DHCP server.

Fix:
- Verified that DC1 and PC1 were connected to the same Internal Network.
- Checked DC1 static IP settings.
- Tested connectivity using a temporary static IP address on PC1.

Verification:
- PC1 successfully reached DC1 after correcting the network configuration.

---------------------------------------
### Problem 3: nslookup amir.local Failed

Issue:
- PC1 could not resolve amir.local.

Root Cause:
- PC1 was using IPv6 DNS servers before the lab DNS server.

Fix:
- Set PC1 IPv4 DNS manually to 192.168.10.10.
- Disabled IPv6 temporarily for the lab.

Verification:
- nslookup amir.local used DC1 as the DNS server.

----------------------------------------------------
### Problem 4: Group Policy Was Not Applying

Issue:
- The Group Policy Object did not appear under Applied Group Policy Objects.

Root Cause:
- The GPO contained User Configuration settings, but it was linked only to the Workstations OU.
- The user ali.it was located in the IT OU.

Fix:
- Linked the Workstation Security Policy GPO to the IT OU.

Verification Commands:
- gpupdate /force
- gpresult /r

Verification Result:
- The Workstation Security Policy appeared under Applied Group Policy Objects.

----------------------------------------------------------
### Final Result

This lab proves hands-on experience with Windows Server network services in a small office domain environment.

The lab demonstrates the ability to configure and troubleshoot:
- IP addressing
- DNS
- DHCP
- Active Directory
- Domain client connectivity
- Shared network resources
- Group Policy
- User access issues
- Basic Windows network troubleshooting
