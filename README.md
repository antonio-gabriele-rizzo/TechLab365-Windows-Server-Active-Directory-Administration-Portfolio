<p align="center">
  <img src="logo/techlab365-logo.png" alt="TechLab365" width="250">
</p>

<p align="center">
  <img src="logo/windows-server-active-directory-administration-title.png" alt="Windows Server & Active Directory Administration Portfolio">
</p>

---

# Project Overview

This repository documents my hands-on Windows Server and Active Directory Administration laboratory, developed from scratch within a virtualised environment using Oracle VirtualBox, Windows Server 2025 and Windows 11 Pro.

Unlike the previous repositories in my **TechLab365 Microsoft Cloud Administration Portfolio**, which focused on Microsoft cloud services, this project introduces the underlying on-premises infrastructure used in traditional enterprise environments.

Rather than assuming that the infrastructure already exists, every stage of the deployment is documented, beginning with the installation of Oracle VirtualBox, followed by Windows Server 2025, Windows 11 Pro, Active Directory Domain Services (AD DS), domain administration and the core Windows Server services required to operate an enterprise network.

Every chapter combines technical explanations, practical configuration and screenshots of the laboratory environment. The documentation is designed to demonstrate genuine hands-on experience while also providing a technical reference that can be used to reproduce the procedures in the future.

This repository represents the next stage of the TechLab365 learning journey, extending from Microsoft 365, Microsoft Entra ID and Microsoft Intune administration into traditional Windows Server and Active Directory infrastructure.

---

# Project Objectives

The primary objectives of this repository are to:

- Configure a virtual laboratory using Oracle VirtualBox.
- Install and configure Windows Server 2025.
- Install and configure Windows 11 Pro.
- Deploy Active Directory Domain Services (AD DS).
- Create and administer an Active Directory forest and domain.
- Create and manage Users, Groups and Organisational Units (OUs).
- Manage computer objects within Active Directory.
- Configure password and account management.
- Join Windows 11 Pro to the Active Directory domain.
- Configure and administer Group Policy.
- Deploy and manage DNS services.
- Deploy and manage DHCP services.
- Configure File Server roles and NTFS permissions.
- Troubleshoot common Windows Server and Active Directory issues.
- Produce professional technical documentation suitable for a GitHub portfolio.
- Create a reusable technical reference for future learning and revision.

---

# Skills Demonstrated

Throughout this project I will demonstrate practical experience with:

- Windows Server 2025 Administration
- Active Directory Domain Services (AD DS)
- Oracle VirtualBox
- Virtual Infrastructure Deployment
- Windows Networking
- Domain Administration
- Active Directory Users and Computers
- User Management
- Security Group Management
- Organisational Unit (OU) Management
- Computer Object Management
- Password Management
- Account Disable/Enable
- Group Membership Management
- Domain Joining
- Domain Authentication
- Group Policy Administration
- DNS Administration
- DHCP Administration
- File Server Administration
- NTFS Permissions
- Active Directory Troubleshooting
- Technical Documentation
- GitHub Portfolio Development

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Oracle VirtualBox | Virtualisation platform |
| Windows Server 2025 | Server operating system |
| Windows 11 Pro | Domain client |
| Active Directory Domain Services | Identity and domain management |
| DNS Server | Name resolution |
| DHCP Server | Automatic IP addressing |
| Group Policy Management | Centralised administration |
| File Services | Network file sharing |
| GitHub | Version control |
| Markdown | Technical documentation |

---

# Lab Environment

| Component | Details |
|-----------|---------|
| Hypervisor | Oracle VirtualBox 7.2.16 |
| Server Operating System | Windows Server 2025 Standard Evaluation (Desktop Experience) |
| Client Operating System | Windows 11 Pro |
| Directory Service | Active Directory Domain Services |
| Network Type | Private Virtual Network |
| Documentation | Markdown |
| Version Control | GitHub |

The laboratory consists of:

```text
VirtualBox
│
├── Windows Server 2025
│   └── Domain Controller
│
└── Windows 11 Pro
    └── Domain Client
TechLab365-Windows-Server-Active-Directory-Administration-Portfolio
│
├── README.md
│
├── logo
│   ├── techlab365-logo.png
│   └── windows-server-active-directory-administration-title.png
│
├── Installing-Oracle-VirtualBox
│   ├── Installing-Oracle-VirtualBox.md
│   └── screenshots
│
├── Installing-Windows-Server
│   ├── Installing-Windows-Server.md
│   └── screenshots
│
├── Preparing-Windows-Server
│   ├── Preparing-Windows-Server.md
│   └── screenshots
│
├── Installing-Windows-11
│   ├── Installing-Windows-11.md
│   └── screenshots
│
├── Preparing-Windows-11
│   ├── Preparing-Windows-11.md
│   └── screenshots
│
├── Installing-Active-Directory-Domain-Services
│   ├── Installing-Active-Directory-Domain-Services.md
│   └── screenshots
│
├── Active-Directory-Administration
│   ├── Active-Directory-Administration.md
│   └── screenshots
│
├── Joining-Windows-11-to-the-Domain
│   ├── Joining-Windows-11-to-the-Domain.md
│   └── screenshots
│
├── Group-Policy
│   ├── Group-Policy.md
│   └── screenshots
│
├── DNS-Administration
│   ├── DNS-Administration.md
│   └── screenshots
│
├── DHCP-Administration
│   ├── DHCP-Administration.md
│   └── screenshots
│
├── File-Server-and-NTFS-Permissions
│   ├── File-Server-and-NTFS-Permissions.md
│   └── screenshots
│
└── Troubleshooting-Scenarios
    ├── Troubleshooting-Scenarios.md
    └── screenshots
