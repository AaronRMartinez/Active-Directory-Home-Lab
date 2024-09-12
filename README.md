# Active Directory and Group Policy Home Lab

## Description

**Utilized Oracle VM VirtualBox with both Windows Server and Windows 10 ISO files to install and configure Active Directory (AD), practicing common Group Policy configurations and administrative tasks.**

This home lab was inspired by several Youtube videos and the online course **"Active Directory & Group Policy Lab** offered by Udemy.

<a href="https://github.com/AaronRMartinez/Active-Directory-and-Group-Policy-Home-Lab/blob/main/ActiveDirectoryandGroupPolicyLab.pdf">Active Directory & Group Policy Certification</a>

In this lab, I used Oracle VM VirtualBox to run virtual machines for both Windows Server and Windows 10 operating systems. A domain controller was established using the Windows Server ISO, and the domain was created afterward. The domain controller was then configured with NAT, routing, and DHCP. With the domain controller set up, numerous common AD procedures were exercised. Methods involving Group Policy, including software deployment, domain password and account lockout policies, password policy enforcement, and Windows Firewall configuration, were also applied. PowerShell scripts were also executed to accomplish administrative tasks such as creating user accounts or moving disabled user accounts to a designated organizational unit (OU).

## Tools

**Oracle VM VirtualBox:** Used to virtualize both a Window Server operating system and a Windows 10 operating system as a guest machine

## Languages Used

**Powershell:** Script used to list AD users, configure roaming user profiles, create AD accounts, and move disabled users to a designated Organizational Unit

## Overview

Downloaded and installed Oracle VM VirtualBox with the intention of running virtual machines for both the Windows Server and Windows 10 operating systems. Both operating system files were downloaded to configure their respective virtual machines. With one of the virtual machines acting as the domain controller, the system was configured to have two network adapters. One is used to connect to the external Internet, while the other connects to VirtualBox's private network for internal clients. Windows Server was then installed on one of the virtual machines to act as the domain controller. With IP addressing being assigned for the internal network. After establishing the domain controller, Active Directory was installed, and the domain was created. Network address translation (NAT) and routing was configured to allow the internal clients to access the Internet through the domain controller. A Dynamic Host Configuration Protocol (DHCP) was then set up on the domain controller. Allowing the internal clients on the private network to be assigned an IP address.

With the domain controller deployed with Active Directory established, numerous AD procedures and Group Policies were performed. Such as:

AD Procedures
  - Creating Users
  - Searching Objects
  - Resetting User Passwords
  - Disabling and Deleting User Accounts
  - Setting Up a Logon Banner
  - Configuring Roaming Profiles for User Accounts
  - Creating a System State Backup
  - Restoring an Active Directory Backup

Group Policy Procedures
  - Creating and Linking Group Policy Objects (GPOs)
  - Editing GPOs
  - Deploying Software
  - Mapping Network Share Drives
  - Configuring Domain Password and Account Lockout Policies
  - Deploying Password Policies
  - Configuring Windows Firewall
  - Configuring Windows Registry Settings

Any Group Policy troubleshooting was conducted by either utilizing Resultant Set of Policy (RSOP.msc) or the Command Prompt. Executing commands such as **gpresult /r**.

In order to automate laborious AD administrative tasks, PowerShell script execution was enabled. Once implemented, several PowerShell scripts were utilized to conduct common AD tasks. Such as:

**Creating User Accounts**

```
﻿# Import AD Module
Import-Module ActiveDirectory

# Grab variables from user
$firstname = Read-Host -Prompt "Please enter the first name"
$lastname = Read-Host -Prompt "Please enter the last name"

# Create the AD User
New-ADUser `
    -Name "$firstname $lastname" `
    -GivenName $firstname `
    -Surname $lastname `
    -UserPrincipalName "$firstname.$lastname" `
    -AccountPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force) `
    -Path "OU=Domain Users,OU=TEST mydomain,DC=mydomain,DC=com" `
    -ChangePasswordAtLogon 1
```

**Configuring Roaming Profile Paths**

```
﻿# Importing AD module
Import-Module ActiveDirectory

# Get all members of the roaming profile group
Get-ADGroupMember 'Roaming Profile Users' |
    # Loop through each user
    ForEach-Object {
        # Do this for each member
        Set-ADUser -Identity $_.SamAccountName -ProfilePath ("\\DC\Profiles$\" + $_.SamAccountName)
    }
```

**Moving Disabled Users to a Designated OU for Disabled Users**

```
 Import the AD Module
Import-Module ActiveDirectory

# List all disabled AD users
Search-ADAccount -AccountDisabled | Select-Object Name, DistinguishedName

# Move all disabled AD users to disabled users OU
Search-ADAccount -AccountDisabled | Where {$_.DistinguishedName -notlike "*OU=Disabled Users*"} | Move-ADObject -TargetPath "OU=Disabled Users,OU=TEST mydomain,DC=mydomain,DC=com"

# Disable all users in the disabled users OU
Get-ADUser -Filter {Enabled -eq $True} -SearchBase "OU=Disabled Users,OU=TEST mydomain,DC=mydomain,DC=com" | Disable-ADAccount
```
