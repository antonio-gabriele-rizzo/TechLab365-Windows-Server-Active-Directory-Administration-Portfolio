# Installing Active Directory Domain Services

## Overview

This chapter documents the installation and configuration of **Active Directory Domain Services (AD DS)** on Windows Server and the creation of a new Active Directory domain.

During this lab, Windows Server was promoted to a Domain Controller for:

```text
techlab365.local
```

The Domain Controller hostname is:

```text
DC01
```

DNS Server was installed alongside AD DS because Active Directory relies on DNS for service discovery and domain communication.

This chapter also includes post-installation verification using Server Manager, Active Directory Users and Computers, DNS Manager, PowerShell and Active Directory diagnostic tools.

---

# Lab Environment

| Component | Configuration |
|---|---|
| Server Name | DC01 |
| Server Role | Domain Controller |
| Active Directory Domain | techlab365.local |
| Forest | techlab365.local |
| DNS Server | Installed |
| SYSVOL Replication | DFS Replication |
| Active Directory Database | NTDS |
| Domain Functional Mode | Windows Server 2025 |
| Forest Functional Mode | Windows Server 2025 |

---

# Learning Objectives

By completing this chapter, I practised how to:

- Install the Active Directory Domain Services role
- Promote a Windows Server to a Domain Controller
- Create a new Active Directory forest
- Create a new Active Directory domain
- Install and configure DNS during Domain Controller promotion
- Verify the successful installation of Active Directory
- Explore Active Directory Users and Computers
- Explore DNS Manager
- Verify domain and forest information using PowerShell
- Verify critical Domain Controller services
- Run Active Directory diagnostic checks using `dcdiag`
- Investigate diagnostic warnings after Domain Controller promotion

---

# 1. Installing the Active Directory Domain Services Role

The first step was to install the **Active Directory Domain Services (AD DS)** role using Server Manager.

From Server Manager:

```text
Manage
   ↓
Add Roles and Features
```

The installation wizard was used to add:

- Active Directory Domain Services
- DNS Server

Installing DNS alongside Active Directory is important because domain-joined computers use DNS to locate services such as Domain Controllers, LDAP services, Kerberos authentication services and Global Catalog servers.

### Screenshot

![Add Roles and Features](images/add-roles-features.png)

---

# 2. Checking Installation Prerequisites

Before promoting the server to a Domain Controller, the Active Directory Domain Services Configuration Wizard performs prerequisite checks.

The prerequisite validation completed successfully, allowing the Domain Controller promotion process to continue.

### Screenshot

![Prerequisites Check](images/prerequisites-check.png)

---

# 3. Configuring the Domain Controller

After installing the AD DS role, the server was promoted to a Domain Controller.

The deployment configuration was set to create:

```text
A new forest
```

The new root domain name configured was:

```text
techlab365.local
```

### Domain Configuration

| Setting | Value |
|---|---|
| Deployment Operation | Add a new forest |
| Root Domain Name | techlab365.local |
| Domain Controller | DC01 |
| DNS Server | Installed |
| Global Catalog | Enabled |

Creating a new forest means that this Domain Controller becomes the first Domain Controller within the new Active Directory environment.

---

# 4. Reviewing the Domain Controller Configuration

Before installation, the configuration wizard provided a summary of the selected settings.

This review stage is important because promoting a server to a Domain Controller significantly changes its role within the infrastructure.

The server will become responsible for:

- Active Directory authentication
- Directory services
- DNS
- Group Policy processing
- Domain resource management

### Screenshot

![Review Domain Controller Configuration](images/review-domain-controller-configuration.png)

---

# 5. Completing the Active Directory Installation

After completing the configuration wizard, Windows installed the required Active Directory components and promoted the server to a Domain Controller.

The installation required a server restart.

After the restart, the server became the Domain Controller for:

```text
techlab365.local
```

### Screenshot

![AD DS Installation Complete](images/ad-ds-installation-complete.png)

---

# 6. Verifying the Domain Controller in Server Manager

After restarting the server, Server Manager showed the newly installed management sections.

The following roles were visible:

- AD DS
- DNS
- File and Storage Services

The presence of the AD DS and DNS sections confirms that the required server roles were successfully installed.

### Screenshot

![Domain Controller Server Manager](images/domain-controller-server-manager.png)

---

# 7. Verifying Active Directory Users and Computers

The Active Directory Users and Computers management console was opened to verify that the domain had been created successfully.

The new domain:

```text
techlab365.local
```

was visible within the console.

The default Active Directory containers were also created automatically, including:

- Builtin
- Computers
- Domain Controllers
- ForeignSecurityPrincipals
- Managed Service Accounts
- Users

### Screenshot

![Active Directory Domain](images/active-directory-domain.png)

| Container | Purpose |
|---|---|
| Users | Stores user accounts and default security groups |
| Computers | Stores computer accounts |
| Domain Controllers | Stores Domain Controller computer accounts |
| Builtin | Contains built-in security groups |
| Managed Service Accounts | Stores managed service accounts |

---

# 8. Verifying DNS Installation

DNS Manager was opened to confirm that DNS was installed and functioning on the Domain Controller.

The DNS server:

```text
DC01
```

was visible in the DNS Manager console.

### Screenshot

![DNS Options](images/dns-options.png)

DNS is essential within an Active Directory environment because Windows clients and servers use DNS to locate domain services.

---

# 9. Verifying Forward Lookup Zones

Within DNS Manager, the Forward Lookup Zones section was expanded.

The following Active Directory-integrated DNS zones were visible:

```text
_msdcs.techlab365.local
techlab365.local
```

### Screenshot

![DNS Forward Lookup Zones](images/dns-forward-lookup-zones.png)

The `_msdcs` zone contains DNS records used by Active Directory services and Domain Controllers.

---

# 10. Verifying DNS Records

The `techlab365.local` DNS zone was opened to examine the automatically created records.

The zone contained records including:

- Start of Authority (SOA)
- Name Server (NS)
- Host (A)
- IPv6 Host (AAAA)

The Domain Controller `DC01` was also registered within DNS.

### Screenshot

![DNS Zone Records](images/dns-zone-records.png)

---

# 11. Verifying the Active Directory Domain and Forest

PowerShell was used to verify the Active Directory domain configuration.

The following commands were executed:

```powershell
Get-ADDomain
```

```powershell
Get-ADForest
```

The results confirmed:

- The Active Directory domain exists
- The DNS root is `techlab365.local`
- DC01 is the PDC Emulator
- The Active Directory forest was successfully created
- DC01 holds the required forest-level roles

### Screenshot

![Verify Domain and Forest](images/verify-domain-and-forest.png)

---

# 12. Verifying Critical Domain Controller Services

The following PowerShell command was used:

```powershell
Get-Service NTDS,DNS,Netlogon | Select-Object Name, Status, StartType
```

The results confirmed:

```text
Name       Status   StartType
----       ------   ---------
DNS        Running  Automatic
Netlogon   Running  Automatic
NTDS       Running  Automatic
```

### Screenshot

![Verify Domain Controller Services](images/verify-domain-controller-services.png)

| Service | Purpose |
|---|---|
| NTDS | Provides Active Directory Domain Services |
| DNS | Provides Domain Name System services |
| Netlogon | Supports authentication and Domain Controller location |

---

# 13. Running Active Directory Diagnostics

The `dcdiag` command-line utility was used to perform diagnostic tests against the Domain Controller.

```powershell
dcdiag
```

The majority of critical Active Directory tests passed successfully, including:

```text
DC01 passed test Connectivity
DC01 passed test Advertising
DC01 passed test SysVolCheck
DC01 passed test KccEvent
DC01 passed test NetLogons
DC01 passed test Replications
DC01 passed test Services
DC01 passed test VerifyReferences
techlab365.local passed test LocatorCheck
techlab365.local passed test Intersite
```

This confirmed that the newly installed Domain Controller was functioning correctly.

---

# 14. Investigating DFS Replication Warnings

The initial `dcdiag` output reported a failure relating to the DFS Replication Event Log.

```text
DC01 failed test DFSREvent
```

To investigate further:

```powershell
dcdiag /test:DFSREvent /v
```

The output showed recent DFS Replication events generated shortly after the Domain Controller installation.

Because this was a newly promoted Domain Controller, the diagnostic results were reviewed alongside additional verification checks rather than assuming the events represented an ongoing failure.

---

# 15. Verifying the DFS Replication Service

The DFS Replication service was checked using:

```powershell
Get-Service DFSR | Select-Object Name, Status, StartType
```

The output confirmed:

```text
Name  Status   StartType
----  ------   ---------
DFSR  Running  Automatic
```

The DFS Replication migration state was also checked:

```powershell
dfsrmig /getglobalstate
```

Result:

```text
Current DFSR global state: 'Eliminated'
Succeeded.
```

This confirms that the environment is using DFS Replication rather than the legacy File Replication Service.

---

# 16. Verifying SYSVOL and NETLOGON Shares

The SYSVOL and NETLOGON shares were checked using:

```powershell
Get-SmbShare -Name SYSVOL,NETLOGON | Select-Object Name, Path, Description
```

The results confirmed that both shares were available.

```text
Name     Path
----     ----
NETLOGON C:\WINDOWS\SYSVOL\sysvol	echlab365.local\SCRIPTS
SYSVOL   C:\WINDOWS\SYSVOL\sysvol
```

These shares are essential because:

- **SYSVOL** stores Group Policy files and scripts
- **NETLOGON** provides a location for domain logon scripts

Their availability confirms that essential Domain Controller functionality was operating correctly.

---

# 17. Verifying DNS Name Resolution

DNS resolution was tested using:

```powershell
Resolve-DnsName techlab365.local
```

and:

```powershell
Resolve-DnsName dc01.techlab365.local
```

Both commands successfully returned DNS records.

This confirmed that DNS was resolving:

```text
techlab365.local
```

and:

```text
dc01.techlab365.local
```

within the newly created Active Directory environment.

---

# Installation Verification Summary

| Verification | Result |
|---|---|
| AD DS Role Installed | ✅ Successful |
| Domain Controller Promotion | ✅ Successful |
| New Forest Created | ✅ Successful |
| Domain Created | ✅ techlab365.local |
| DNS Server Installed | ✅ Running |
| Active Directory Users and Computers | ✅ Accessible |
| DNS Manager | ✅ Accessible |
| Domain Information | ✅ Verified |
| Forest Information | ✅ Verified |
| NTDS Service | ✅ Running |
| DNS Service | ✅ Running |
| Netlogon Service | ✅ Running |
| DFSR Service | ✅ Running |
| SYSVOL Share | ✅ Available |
| NETLOGON Share | ✅ Available |
| DNS Name Resolution | ✅ Successful |
| Active Directory Diagnostics | ⚠️ DFSR events investigated |

---

# Key Takeaways

This lab demonstrated the complete process of transforming a Windows Server into an Active Directory Domain Controller.

The most important concepts reinforced were:

- Active Directory relies heavily on DNS
- A Domain Controller provides centralised authentication and directory services
- SYSVOL is required for Group Policy replication
- NETLOGON provides domain logon-related functionality
- Domain Controllers run multiple critical services
- PowerShell can be used to verify Active Directory configuration
- `dcdiag` provides an important diagnostic tool for checking Domain Controller health
- Event warnings should be investigated rather than ignored

---

# Skills Practised

During this chapter, I gained hands-on experience with:

- Windows Server
- Active Directory Domain Services
- Domain Controller promotion
- Active Directory forest creation
- Active Directory domain creation
- DNS Server
- DNS Forward Lookup Zones
- Active Directory-integrated DNS
- Active Directory Users and Computers
- PowerShell Active Directory module
- `Get-ADDomain`
- `Get-ADForest`
- Windows Services
- DFS Replication
- SYSVOL
- NETLOGON
- DNS troubleshooting
- `Resolve-DnsName`
- `dcdiag`

---

# Chapter Summary

In this chapter, I successfully installed Active Directory Domain Services and promoted `DC01` to the first Domain Controller in a new Active Directory forest.

The deployment created the:

```text
techlab365.local
```

domain and integrated DNS infrastructure required for Active Directory service discovery.

The installation was verified through graphical management tools, PowerShell commands, service checks, DNS resolution tests and Active Directory diagnostics. Critical services including NTDS, DNS, Netlogon and DFSR were confirmed as operational, while SYSVOL and NETLOGON shares were also verified.

The completed Domain Controller now provides the identity infrastructure required for the remaining Windows Server administration labs.

---

# Next Chapter

## 07 - Active Directory Administration

The next stage will focus on managing the Active Directory environment, including:

- Users
- Groups
- Organisational Units
- Computer accounts
- Account management
- Group membership
- Administrative delegation

---

# Chapter Progress

```text
✅ 06 - Installing Active Directory Domain Services
⬜ 07 - Active Directory Administration
⬜ 08 - Joining Windows 11 to the Domain
⬜ 09 - Group Policy
⬜ 10 - DNS Administration
⬜ 11 - DHCP Administration
⬜ 12 - File Server and NTFS Permissions
⬜ 13 - Troubleshooting Scenarios
```
