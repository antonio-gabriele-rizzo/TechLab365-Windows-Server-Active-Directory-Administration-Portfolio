# Chapter 05 – Network Configuration and Connectivity

## Overview

This chapter documents the network configuration used to connect the Windows 11 client and Windows Server within the lab.

The environment uses two network adapters on each virtual machine:

- **NAT adapter** – provides internet connectivity.
- **Internal network adapter (AD-Lab)** – provides isolated communication between the Windows 11 client and the Domain Controller.

---

## Network Topology

| Device | Role | Internal IP Address |
|---|---|---|
| DC01 | Windows Server / Domain Controller | `10.10.10.10` |
| WIN11-01 | Windows 11 Client | `10.10.10.20` |

```text
AD-Lab
│
├── DC01
│   └── 10.10.10.10
│
└── WIN11-01
    └── 10.10.10.20
```

Subnet: `10.10.10.0/24`

---

# 1. Windows 11 Network Configuration

The Windows 11 virtual machine uses two network adapters.

## Adapter 1 – Internet Access

NAT provides internet connectivity through DHCP:

- IP address: `10.0.2.15`
- Subnet mask: `255.255.255.0`
- Default gateway: `10.0.2.2`
- DHCP server: `10.0.2.2`

## Adapter 2 – Internal Lab Network

The second adapter connects Windows 11 to the isolated AD-Lab network:

- IP address: `10.10.10.20`
- Subnet mask: `255.255.255.0`
- Default gateway: Not configured

No default gateway is required because this adapter is used only for internal lab communication.

The configuration was verified using:

```cmd
ipconfig /all
```

![Windows 11 network configuration](windows-11-network-configuration.png)

---

# 2. Windows Server Network Configuration

The Windows Server virtual machine also uses two network adapters.

## Adapter 1 – Internet Access

NAT provides internet connectivity through DHCP:

- IP address: `10.0.2.15`
- Default gateway: `10.0.2.2`

## Adapter 2 – Internal Lab Network

The internal adapter connects the server to AD-Lab:

- IP address: `10.10.10.10`
- Subnet mask: `255.255.255.0`
- Default gateway: Not configured

---

# 3. Internet Connectivity Verification

Windows 11 internet connectivity was tested using:

```cmd
ping 8.8.8.8
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

DNS resolution was tested with:

```cmd
nslookup google.com
```

The hostname resolved successfully.

Internet connectivity using DNS was then verified:

```cmd
ping google.com
```

The test succeeded, confirming both internet connectivity and DNS resolution.

---

# 4. Internal Network Connectivity

The Windows 11 client was configured with the static address:

```text
10.10.10.20
```

The Windows Server internal adapter was configured with:

```text
10.10.10.10
```

The Windows Server successfully communicated with the Windows 11 client.

The reverse test from Windows 11 initially failed.

---

# 5. Troubleshooting – Domain Controller Ping Failure

Initially, Windows 11 could not ping the Domain Controller:

```cmd
ping 10.10.10.10
```

Result:

```text
Request timed out.
```

The network configuration was checked and communication from Windows Server to Windows 11 was working.

Further investigation identified that the Windows Firewall on the server was blocking inbound ICMP Echo Requests.

The firewall rules were checked using PowerShell:

```powershell
Get-NetFirewallRule -DisplayName "*Echo Request*" | Select-Object DisplayName, Enabled
```

The required ICMPv4 inbound rule was disabled.

## Solution

The inbound ICMP Echo Request rule was enabled:

```powershell
Enable-NetFirewallRule -DisplayName "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)"
```

The configuration was verified:

```powershell
Get-NetFirewallRule -DisplayName "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" | Select-Object DisplayName, Enabled
```

The rule was successfully enabled and the server responded to ping requests.

![Firewall troubleshooting and successful ping](network-connectivity-troubleshooting.png)

---

# 6. Final Connectivity Verification

After enabling the ICMP firewall rule, Windows 11 successfully communicated with the Domain Controller.

From Windows 11:

```cmd
ping 10.10.10.10
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

![Successful ping from Windows 11 to the Domain Controller](ping-domain-controller.png)

Final communication:

```text
WIN11-01 (10.10.10.20)
        ↕
DC01 (10.10.10.10)
```

---

# Summary

The network configuration for the lab is complete:

- Internet connectivity through NAT.
- Working DNS resolution.
- Dedicated isolated AD-Lab network.
- Static internal IP addresses.
- Successful communication between Windows 11 and Windows Server.
- ICMP firewall troubleshooting documented and resolved.

The network foundation is ready for the next lab stage.
