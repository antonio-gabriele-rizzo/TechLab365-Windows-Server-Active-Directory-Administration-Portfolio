<p align="center">
  <img src="logo/techlab365-logo.png" alt="TechLab365" width="250">
</p>

<p align="center">
  <img src="logo/windows-server-active-directory-administration-title.png" alt="Windows Server & Active Directory Administration Portfolio">
</p>

---

# Windows Server & Active Directory Administration Portfolio



This repository documents my hands-on Windows Server and Active Directory Administration laboratory, developed entirely from scratch within a virtualised environment using Oracle VirtualBox, Windows Server and Windows 11.

Unlike the previous repositories in my **TechLab365 Microsoft Cloud Administration Portfolio**, which focused on administering Microsoft cloud services, this project begins with the underlying on-premises infrastructure that continues to support enterprise environments around the world.

Rather than assuming that the infrastructure already exists, every stage of the deployment is documented, beginning with the installation and configuration of Oracle VirtualBox, followed by Windows Server, Windows 11, Active Directory Domain Services (AD DS), domain administration and the core Windows Server services required to operate an enterprise network.

Every chapter combines detailed explanations, annotated screenshots and practical exercises, demonstrating the complete implementation of a Windows domain environment while following industry best practices.

This repository represents the natural continuation of the TechLab365 learning journey, extending from Microsoft cloud administration into traditional Windows infrastructure and demonstrating how on-premises identity services complement modern Microsoft cloud technologies.

---

# Project Objectives

Throughout this project, I will build a complete Windows Server infrastructure while developing practical experience with enterprise domain administration.

The primary objectives of this repository are to:

- Configure a virtual laboratory using Oracle VirtualBox.
- Install Windows Server.
- Configure Windows Server for enterprise use.
- Install Windows 11.
- Configure Windows 11 as a domain client.
- Deploy Active Directory Domain Services (AD DS).
- Create and administer an Active Directory forest and domain.
- Join Windows 11 to the domain.
- Configure and administer Group Policy.
- Deploy and manage DNS services.
- Deploy and manage DHCP services.
- Configure File Server roles and NTFS permissions.
- Troubleshoot common Active Directory administration issues.
- Produce professional technical documentation suitable for a GitHub portfolio.

---

# Skills Demonstrated

Throughout this project I will demonstrate practical experience with:

- Windows Server Administration
- Active Directory Domain Services (AD DS)
- Oracle VirtualBox
- Virtual Infrastructure Deployment
- Windows Networking
- Domain Administration
- User and Group Management
- Organisational Units (OUs)
- Group Policy Administration
- DNS Administration
- DHCP Administration
- Windows 11 Domain Administration
- File Server Administration
- Active Directory Troubleshooting
- Technical Documentation
- GitHub Portfolio Development

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| Oracle VirtualBox | Virtualisation platform |
| Windows Server 2022 | Server operating system |
| Windows 11 Pro | Domain client |
| Active Directory Domain Services | Identity management |
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
| Hypervisor | Oracle VirtualBox |
| Server Operating System | Windows Server 2022 Standard (Desktop Experience) |
| Client Operating System | Windows 11 Pro |
| Directory Service | Active Directory Domain Services |
| Network Type | Private Virtual Network |
| Documentation | Markdown |
| Version Control | GitHub |

---

# Repository Structure

```text
TechLab365-Windows-Server-Active-Directory-Administration-Portfolio
│
├── README.md
├── LICENSE
│
├── logo
│   ├── techlab365-logo.png
│   └── windows-server-active-directory-administration-title.png
│
├── 01-Installing-Oracle-VirtualBox
│   ├── README.md
│   └── screenshots
│
├── 02-Installing-Windows-Server
│   ├── README.md
│   └── screenshots
│
├── 03-Configuring-Windows-Server
│   ├── README.md
│   └── screenshots
│
├── 04-Installing-Windows-11
│   ├── README.md
│   └── screenshots
│
├── 05-Configuring-Windows-11
│   ├── README.md
│   └── screenshots
│
├── 06-Installing-Active-Directory-Domain-Services
│   ├── README.md
│   └── screenshots
│
├── 07-Active-Directory-Administration
│   ├── README.md
│   └── screenshots
│
├── 08-Joining-Windows-11-to-the-Domain
│   ├── README.md
│   └── screenshots
│
├── 09-Group-Policy
│   ├── README.md
│   └── screenshots
│
├── 10-DNS-Administration
│   ├── README.md
│   └── screenshots
│
├── 11-DHCP-Administration
│   ├── README.md
│   └── screenshots
│
├── 12-File-Server-and-NTFS-Permissions
│   ├── README.md
│   └── screenshots
│
└── 13-Troubleshooting-Scenarios
    ├── README.md
    └── screenshots

```

---

# Chapter Overview

## 01 – Installing Oracle VirtualBox

The repository begins by preparing the virtual laboratory environment using Oracle VirtualBox. This chapter demonstrates how to install VirtualBox, configure the Extension Pack, create the virtual networking environment and verify that the platform is ready to host Windows Server and Windows 11 virtual machines.

### Topics covered

- Oracle VirtualBox overview
- Installing Oracle VirtualBox
- Installing the Extension Pack
- Virtual networking
- Host-only networking
- NAT networking
- Virtual machine requirements
- Preparing the laboratory

### Skills developed

- Virtualisation fundamentals
- Oracle VirtualBox administration
- Virtual networking
- Laboratory preparation

---

## 02 – Installing Windows Server

This chapter demonstrates the installation of Windows Server, beginning with the creation of the virtual machine and continuing through the complete operating system installation. The server is prepared to become the future Domain Controller for the Active Directory environment.

### Topics covered

- Creating the virtual machine
- Windows Server installation
- Desktop Experience
- Initial configuration
- Administrator account
- Server Manager
- Installation verification

### Skills developed

- Windows Server deployment
- Virtual machine administration
- Operating system installation
- Server preparation

---

## 03 – Configuring Windows Server

Before deploying Active Directory, the server must be configured according to best practices. This chapter demonstrates the initial configuration tasks required to prepare Windows Server for its role within the domain environment.

### Topics covered

- Computer renaming
- Static IP configuration
- DNS configuration
- Windows Update
- Time zone configuration
- Remote Desktop
- Server validation

### Skills developed

- Windows Server administration
- TCP/IP configuration
- Server hardening
- Infrastructure preparation

---

## 04 – Installing Windows 11

The Windows 11 client is installed within Oracle VirtualBox to simulate a workstation in the enterprise environment. This chapter demonstrates the complete installation process and prepares the client operating system for domain integration.

### Topics covered

- Creating the virtual machine
- Windows 11 installation
- Initial setup
- Local administrator account
- Virtual hardware
- Installation verification

### Skills developed

- Windows 11 deployment
- Virtual workstation administration
- Client operating system installation
- Laboratory preparation

---

## 05 – Configuring Windows 11

The Windows 11 client is configured before joining the Active Directory domain. This chapter demonstrates the initial administrative tasks required to prepare the workstation for enterprise deployment.

### Topics covered

- Computer renaming
- Static IP configuration
- DNS configuration
- Windows Update
- Remote Desktop
- Connectivity testing
- Client validation

### Skills developed

- Windows 11 administration
- Client networking
- Enterprise workstation preparation
- Endpoint configuration

---

## 06 – Installing Active Directory Domain Services

With the infrastructure in place, this chapter demonstrates the deployment of Active Directory Domain Services (AD DS). Windows Server is promoted to a Domain Controller while simultaneously installing DNS and creating the first Active Directory forest and domain.

### Topics covered

- Active Directory Domain Services
- Installing AD DS
- Domain Controller promotion
- Creating a new forest
- Creating a new domain
- DNS integration
- Verifying the installation

### Skills developed

- Active Directory deployment
- Domain Controller administration
- Forest and domain creation
- Enterprise identity management

---

## 07 – Active Directory Administration

Once the domain has been created, administrators can begin managing the Active Directory environment. This chapter introduces the primary administrative tools used to organise users, groups, computers and organisational units while demonstrating the day-to-day management tasks performed within a Windows domain.

### Topics covered

- Active Directory Users and Computers
- Organisational Units (OUs)
- Creating users
- Creating Security Groups
- Managing group membership
- Computer objects
- Active Directory administration

### Skills developed

- Active Directory administration
- User lifecycle management
- Security Group administration
- Organisational Unit management

---

## 08 – Joining Windows 11 to the Domain

With Active Directory operational, the Windows 11 client is joined to the domain. This chapter demonstrates the complete domain join process, verifies communication with the Domain Controller and confirms that users can authenticate using domain credentials.

### Topics covered

- Domain Name System (DNS) verification
- Domain connectivity
- Joining a Windows client to the domain
- Domain authentication
- Domain user logon
- Verifying computer objects
- Domain communication

### Skills developed

- Domain administration
- Client deployment
- Active Directory integration
- Enterprise authentication

---

## 09 – Group Policy

Group Policy enables administrators to centrally manage user and computer settings across the entire domain. This chapter demonstrates how to create, configure and apply Group Policy Objects (GPOs) while exploring how policies are processed and verified on domain-joined computers.

### Topics covered

- Group Policy overview
- Group Policy Management Console (GPMC)
- Creating Group Policy Objects
- User Configuration
- Computer Configuration
- Policy inheritance
- Group Policy processing
- Policy verification

### Skills developed

- Group Policy administration
- Centralised configuration management
- Enterprise policy deployment
- Windows administration

---

## 10 – DNS Administration

Active Directory depends heavily on the Domain Name System (DNS). This chapter explores DNS administration by reviewing zones, resource records and name resolution while demonstrating how DNS supports domain authentication and network communication.

### Topics covered

- DNS overview
- Forward Lookup Zones
- Reverse Lookup Zones
- Host (A) records
- Pointer (PTR) records
- Name resolution
- DNS troubleshooting

### Skills developed

- DNS administration
- Name resolution
- Windows networking
- Infrastructure services

---

## 11 – DHCP Administration

Dynamic Host Configuration Protocol (DHCP) automates the assignment of IP addresses and network configuration to client devices. This chapter demonstrates how to install, configure and administer a DHCP server within the Active Directory environment.

### Topics covered

- DHCP overview
- Installing DHCP
- Creating DHCP scopes
- Scope options
- Reservations
- Lease management
- DHCP authorisation

### Skills developed

- DHCP administration
- Network configuration
- IP address management
- Windows Server administration

---

## 12 – File Server and NTFS Permissions

File Services allow administrators to provide secure shared storage across the enterprise. This chapter demonstrates how to create shared folders, configure NTFS permissions and combine Security Groups with access control to implement best-practice file management.

### Topics covered

- File Server overview
- Creating shared folders
- NTFS permissions
- Share permissions
- Security Groups
- Access control
- Permission verification

### Skills developed

- File Server administration
- NTFS permission management
- Shared folder administration
- Enterprise file security

---

## 13 – Troubleshooting Scenarios

The repository concludes with a collection of practical troubleshooting scenarios based on common Windows Server and Active Directory administration issues. Rather than using simulated examples, this chapter documents real administrative problems and demonstrates a structured methodology for identifying, diagnosing and resolving infrastructure issues.

### Topics covered

- Active Directory troubleshooting
- DNS troubleshooting
- Domain join issues
- Authentication problems
- Group Policy troubleshooting
- Network diagnostics
- Administrative best practices

### Skills developed

- Infrastructure troubleshooting
- Root cause analysis
- Windows Server diagnostics
- Active Directory support

---

# Learning Outcomes

By completing this repository, I will gain practical experience building and administering a complete Windows Server infrastructure from the ground up while developing the skills required to manage an enterprise Active Directory environment.

The key learning outcomes include:

- Deploying a virtual infrastructure using Oracle VirtualBox.
- Installing and configuring Windows Server.
- Installing and configuring Windows 11.
- Deploying Active Directory Domain Services.
- Creating and administering an Active Directory domain.
- Managing users, groups and organisational units.
- Joining Windows clients to an Active Directory domain.
- Configuring and administering Group Policy Objects.
- Deploying DNS and DHCP services.
- Managing shared folders and NTFS permissions.
- Applying structured troubleshooting techniques to Windows Server environments.
- Producing professional technical documentation suitable for a GitHub portfolio.

---

# Repository Prerequisites

To reproduce this laboratory, the following components are recommended:

- Oracle VirtualBox 7.x (or later)
- Oracle VM VirtualBox Extension Pack
- Windows Server 2022 Evaluation ISO (Desktop Experience)
- Windows 11 Pro ISO
- Minimum 16 GB RAM (32 GB recommended)
- Quad-Core processor with hardware virtualisation (Intel VT-x / AMD-V)
- At least 120 GB of available SSD storage
- Stable Internet connection
- GitHub account (for documentation and version control)

Although this laboratory is built using Windows Server 2022 and Windows 11, the concepts and administrative procedures demonstrated throughout this repository are equally applicable to newer versions of Windows Server and modern Windows client operating systems.

---

# Related Repositories

This repository forms part of the **TechLab365 Microsoft Administration Portfolio**.

The complete learning journey currently includes:

## ☁️ Microsoft Cloud Administration

- **TechLab365 – Microsoft 365 Administration Portfolio**
- **TechLab365 – Microsoft Entra ID Administration Portfolio**
- **TechLab365 – Microsoft Intune Administration Portfolio**

## 🖥️ Windows Infrastructure Administration

- **TechLab365 – Windows Server & Active Directory Administration Portfolio** *(this repository)*

The Microsoft Cloud Administration repositories demonstrate the administration of Microsoft cloud services, while this repository focuses on building and administering the underlying Windows infrastructure that continues to support enterprise environments worldwide.

Together, these repositories provide a comprehensive overview of modern Microsoft administration across both cloud and on-premises environments.

---

# Conclusion

This repository documents the complete deployment of a Windows Server infrastructure, beginning with an empty virtual environment and progressing through the implementation of Active Directory Domain Services, enterprise networking and Windows domain administration.

Unlike many Active Directory tutorials that assume an existing infrastructure, this project demonstrates every stage of the deployment process, from installing Oracle VirtualBox and Windows Server through to configuring enterprise services such as DNS, DHCP, Group Policy and File Services.

By following each chapter in sequence, readers will gain practical experience building a fully functional Windows domain while understanding how the individual infrastructure components interact to provide authentication, networking and centralised administration.

Completing this project will strengthen practical Windows Server administration skills while reinforcing the relationship between traditional on-premises infrastructure and the Microsoft cloud services explored throughout the other TechLab365 repositories.

In addition to developing technical expertise, this repository continues the TechLab365 commitment to producing structured, professional documentation using Markdown and GitHub, creating a portfolio that accurately reflects real hands-on experience with enterprise Microsoft technologies.

---

# Author

**Antonio Gabriele Rizzo**

**TechLab365 – Microsoft Administration Learning Series**

### ☁️ Microsoft Cloud Administration

- Microsoft 365 Administration
- Microsoft Entra ID Administration
- Microsoft Intune Administration

### 🖥️ Windows Infrastructure Administration

- Windows Server & Active Directory Administration

---

# License

This project is published for educational and portfolio purposes.

The documentation is based on hands-on experience gained while building a Windows Server laboratory using Oracle VirtualBox, Windows Server 2022 and Windows 11.

Microsoft, Windows Server, Windows, Active Directory, Group Policy and Microsoft Entra ID are trademarks of Microsoft Corporation.

Oracle and Oracle VirtualBox are trademarks of Oracle Corporation.
