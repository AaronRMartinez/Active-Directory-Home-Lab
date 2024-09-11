# Active Directory and Group Policy Home Lab

## Description

**Utilized Oracle VM VirtualBox with both Windows Server and Windows 10 ISO files to install and configure Active Directory (AD), practicing common Group Policy configurations and administrative tasks.**

Used Oracle VM VirtualBox to run virtual machines for both Windows Server and Windows 10 operating systems. A domain controller utilizing the Windows Server ISO was established, with a domain being created subsequently.  

## Tools

**Oracle VM VirtualBox:** Used to virtualize both a Window Server operating system and a Windows 10 operating system as a guest machine

## Languages Used

**Powershell:** Script used to list AD users, configure roaming user profiles, create AD accounts, and move disabled users to a designated Organizational Unit

## Overview

Downloaded and installed Oracle VM VirtualBox with the intention of running virtual machines for both the Windows Server and Windows 10 operating systems. Both operating system files were downloaded to configure their respective virtual machines. With one of the virtual machines acting as the domain controller, the system was configured to have two network adapters. One is used to connect to the external Internet, while the other connects to VirtualBox's private network for internal clients. Windows Server was then installed on one of the virtual machines to act as the domain controller. With IP addressing being assigned for the internal network. After establishing the domain controller, Active Directory was installed, and the domain was created. 
