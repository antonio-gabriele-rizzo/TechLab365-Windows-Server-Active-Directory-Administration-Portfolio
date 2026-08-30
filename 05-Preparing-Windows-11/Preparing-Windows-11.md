# Preparing Windows 11

## Overview

This chapter documents the preparation of the Windows 11 client virtual machine that will be used throughout the Active Directory home lab.

The objective was not simply to install Windows 11. The client needed to be configured and verified so that it could operate correctly in a dual-network lab environment:

- **Internet access** through the VirtualBox NAT network.
- **Internal lab connectivity** through the dedicated `AD-Lab` network.
- A clear and identifiable workstation name.
- Correct IP addressing on both network adapters.
- Successful communication with the future domain controller.
- A documented troubleshooting process for connectivity issues encountered during the configuration.

This approach reflects a real IT support and infrastructure workflow: configure the system, verify each component, identify faults, troubleshoot methodically, and document the final working solution.

---

## Lab Configuration

The Windows 11 virtual machine was configured with two network adapters.

| Component | Configuration |
|---|---|
| Operating System | Windows 11 Pro 24H2 |
| Computer Name | `WIN11-01` |
| RAM | 8 GB |
| Processor | Intel Core i7-7700K |
| Virtual Disk | 80 GB |
| Network Adapter 1 | NAT – Internet connectivity |
| Network Adapter 2 | Internal Network – `AD-Lab` |
| Client IP Address | `10.10.10.20` |
| Internal Subnet | `10.10.10.0/24` |
| Domain Controller IP | `10.10.10.10` |

The two-adapter configuration deliberately separates external connectivity from internal Active Directory lab communication.

---

# 1. Verify the Windows 11 Installation

After completing the Windows 11 installation, the first step was to confirm that the virtual machine was running correctly and that the expected system resources had been assigned.

The Windows **System > About** page was used to verify:

- Device name.
- Windows edition.
- Windows version.
- Installed RAM.
- Processor.
- 64-bit operating system.

The workstation was renamed to:

```text
WIN11-01
```

Using a meaningful hostname is important in an Active Directory environment because computers need to be easily identifiable by administrators.

> **Screenshot:** Windows 11 system information and device name.

---

# 2. Configure the Network Architecture

The Windows 11 client uses two separate network adapters.

## Adapter 1 – Internet Connectivity

The first adapter is connected to VirtualBox NAT.

This adapter provides:

- Internet access.
- DHCP addressing from VirtualBox.
- A default gateway.
- External DNS resolution.

The client received the following IPv4 configuration:

```text
IPv4 Address:    10.0.2.15
Subnet Mask:     255.255.255.0
Default Gateway: 10.0.2.2
DHCP Server:     10.0.2.2
```

This adapter is used only for external connectivity.

---

## Adapter 2 – Internal Active Directory Lab

The second adapter is connected to the VirtualBox internal network named:

```text
AD-Lab
```

A static IP address was configured:

```text
IPv4 Address: 10.10.10.20
Subnet Mask:  255.255.255.0
```

No default gateway was configured on the internal adapter.

This is intentional.

The internal network is used for communication between lab machines and does not require a route to the internet. Keeping the default gateway only on the NAT adapter prevents Windows from attempting to use the internal network for external traffic.

> **Screenshot:** `ipconfig /all` showing both network adapters and their configuration.

---

# 3. Verify Internet Connectivity

Before testing the internal lab network, internet connectivity was verified independently.

The following tests were performed:

```cmd
ping 8.8.8.8
```

This confirmed that the workstation could communicate with an external IP address.

DNS resolution was then tested:

```cmd
nslookup google.com
```

The hostname resolved successfully.

Finally, hostname connectivity was tested:

```cmd
ping google.com
```

All tests completed successfully.

### Result

The NAT adapter was correctly configured and provided:

- Internet connectivity.
- IP connectivity.
- DNS name resolution.

This confirmed that the external network configuration was working correctly before troubleshooting the internal lab connection.

---

# 4. Verify Internal Network Connectivity

The Windows 11 client was configured with the internal address:

```text
10.10.10.20
```

The domain controller was configured with:

```text
10.10.10.10
```

The first test confirmed that the client could communicate on the internal network:

```cmd
ping 10.10.10.20
```

The client responded successfully.

The next test targeted the domain controller:

```cmd
ping 10.10.10.10
```

Initially, this test failed with:

```text
Request timed out.
```

Because both virtual machines were configured on the same internal subnet and the server was reachable from its own network interface, this indicated that the problem was not necessarily VirtualBox networking itself.

A systematic troubleshooting process was therefore required.

---

# 5. Troubleshooting Internal Connectivity

## Verify the Server Network Configuration

The Windows Server virtual machine was started and its network configuration was checked using:

```cmd
ipconfig /all
```

The output confirmed that the server had two network adapters:

### Internet Adapter

```text
IPv4 Address:    10.0.2.15
Default Gateway: 10.0.2.2
```

### Internal Lab Adapter

```text
IPv4 Address: 10.10.10.10
Subnet Mask:  255.255.255.0
```

This confirmed that the server and Windows 11 client were correctly configured on the same internal subnet:

```text
DC01     → 10.10.10.10
WIN11-01 → 10.10.10.20
```

The server could also successfully ping the Windows 11 client.

This narrowed the issue down to inbound communication being blocked on the server rather than an addressing or VirtualBox network problem.

---

## Identify the Firewall Issue

PowerShell was used to check the network profiles:

```powershell
Get-NetConnectionProfile
```

The internal adapter was identified as an **Unidentified network** with a **Public** network category.

The Windows Firewall rules for ICMP echo requests were then checked:

```powershell
Get-NetFirewallRule -DisplayName "*Echo Request*" | Select-Object DisplayName, Enabled
```

The relevant inbound ICMPv4 rule was disabled.

This explained why the Windows 11 client could not initially ping the domain controller even though both machines were correctly connected to the same internal network.

---

## Enable ICMP Echo Requests

The inbound ICMPv4 echo request rule was enabled using PowerShell:

```powershell
Enable-NetFirewallRule -DisplayName "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)"
```

The configuration was verified:

```powershell
Get-NetFirewallRule -DisplayName "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" | Select-Object DisplayName, Enabled
```

The rule was confirmed as:

```text
Enabled: True
```

This allowed the server to respond to ICMP ping requests.

---

# 6. Confirm Client-to-Server Connectivity

The final connectivity test was performed from the Windows 11 client:

```cmd
ping 10.10.10.10
```

The result showed four successful replies with no packet loss.

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed successful communication between:

```text
WIN11-01 (10.10.10.20)
        ↓
Internal Network: AD-Lab
        ↓
DC01 (10.10.10.10)
```

> **Screenshot:** Successful ping from `WIN11-01` to the domain controller at `10.10.10.10`.

---

# Troubleshooting Summary

| Issue | Cause | Resolution |
|---|---|---|
| Client could access the internet | NAT adapter working correctly | No action required |
| DNS resolution working | External DNS configuration working | No action required |
| Client could not initially ping DC01 | ICMP echo requests blocked by Windows Firewall | Enabled inbound ICMPv4 echo request rule |
| Internal network communication required verification | Dual-adapter configuration could introduce routing confusion | Verified addressing and connectivity on both adapters |

---

# Key Learning Points

This configuration provided practical experience with several important infrastructure concepts:

- Configuring a Windows client in a virtualised environment.
- Using multiple network adapters for different purposes.
- Separating internet and internal lab traffic.
- Understanding the purpose of a default gateway.
- Using static IP addressing for internal infrastructure.
- Using `ipconfig /all` to investigate network configuration.
- Testing connectivity with `ping`.
- Testing DNS resolution with `nslookup`.
- Using PowerShell to inspect network profiles.
- Investigating Windows Firewall rules.
- Identifying firewall behaviour as a cause of connectivity failures.
- Applying a targeted troubleshooting solution rather than changing multiple settings unnecessarily.

---

# Final Configuration

The Windows 11 client is now fully prepared for the next stage of the lab.

| Setting | Final Configuration |
|---|---|
| Computer Name | `WIN11-01` |
| Operating System | Windows 11 Pro |
| Internet Network | NAT |
| Internet Connectivity | Working |
| Internal Network | `AD-Lab` |
| Client IP | `10.10.10.20` |
| Domain Controller IP | `10.10.10.10` |
| Client-to-Server Ping | Successful |
| DNS Resolution | Working |
| Lab Connectivity | Verified |

The workstation is now ready to be joined to the Active Directory domain in the next stage of the project.

---

## Screenshots Used

This chapter is supported by the following screenshots:

1. Windows 11 system information showing the final workstation configuration and computer name.
2. `ipconfig /all` showing the dual-network configuration.
3. Successful ping from `WIN11-01` to `10.10.10.10` after resolving the firewall issue.

> Screenshot filenames should follow the repository naming convention and should not include chapter numbers.

