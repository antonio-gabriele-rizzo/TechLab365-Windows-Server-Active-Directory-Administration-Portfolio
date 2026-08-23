# Installing Oracle VirtualBox

## Introduction

Oracle VirtualBox is the hypervisor used to build the TechLab365 Windows Server and Active Directory laboratory.

The laboratory requires an isolated virtual environment in which Windows Server 2025 can be installed as the future Domain Controller and Windows 11 Pro can later be configured as a domain client. VirtualBox provides the platform used to create, configure and run those virtual machines without requiring additional physical hardware.

In this chapter, Oracle VirtualBox 7.2.16 was downloaded and installed on the Windows host computer. The installation was then verified by opening VirtualBox Manager and confirming that the virtualisation platform was ready for the next stage of the laboratory.

This chapter documents the actual preparation of the host environment rather than simply describing what VirtualBox is. The configuration and verification steps provide a repeatable reference that can be followed again if the laboratory needs to be rebuilt.

---

# Objectives

After completing this chapter, I will be able to:

- Download Oracle VirtualBox 7.2.16 for Windows.
- Understand the role of VirtualBox within the TechLab365 laboratory.
- Install Oracle VirtualBox using the standard installation options.
- Verify that the installation completed successfully.
- Open and navigate Oracle VirtualBox Manager.
- Confirm that the host is ready to create the laboratory virtual machines.

---

# Prerequisites

Before starting this chapter, ensure you have:

- A Windows host computer.
- Administrator permissions on the host.
- Internet access.
- Sufficient storage space for the virtual machines that will be created later.
- A GitHub repository prepared for the laboratory documentation.

No Windows Server or Windows 11 virtual machine is required at this stage.

---

# Oracle VirtualBox

Oracle VirtualBox is a desktop virtualisation platform that allows complete operating systems to run as virtual machines on a physical host computer.

For this laboratory, VirtualBox provides the layer between the physical Windows computer and the virtual infrastructure that will be built throughout the repository.

The final laboratory will contain two principal virtual machines:

```text
VirtualBox
│
├── Windows Server 2025
│   └── Domain Controller
│
└── Windows 11 Pro
    └── Domain Client
```

Building the environment virtually allows the complete Windows Server and Active Directory infrastructure to be created, tested and rebuilt without requiring separate physical servers and workstations.

---

# Downloading Oracle VirtualBox

The Oracle VirtualBox download page was accessed and the **Windows hosts** package for **VirtualBox 7.2.16** was selected.

![VirtualBox Download Page](screenshots/virtualbox-download-page.png)

The Windows installer was downloaded to the host computer and used for the installation that follows.

> **Note**
>
> The download-page screenshot was captured using the Wayback Machine because the current VirtualBox website was not loading correctly in the browser at the time the laboratory was being documented.

The important point for reproducing the laboratory is to use the appropriate Windows installer for the VirtualBox version being used. In this laboratory, the installed version is **7.2.16**.

---

# Starting the Installation

After the installer had been downloaded, the VirtualBox installation package was opened using the Windows host operating system.

The installer presented the standard Oracle VirtualBox installation process.

![VirtualBox Installer](screenshots/virtualbox-installer-start.png)

The installation was continued using the standard options provided by the installer. No customised installation path or non-standard components were required for this laboratory.

Using the standard installation options keeps the host configuration straightforward and provides the normal VirtualBox Manager environment required for the remainder of the project.

During installation, Windows may temporarily modify network connectivity while VirtualBox networking components are installed. This is expected because VirtualBox provides virtual networking capabilities that will later be required when the Windows Server and Windows 11 virtual machines communicate with each other.

---

# Completing the Installation

After the installation process completed, the installer displayed confirmation that **Oracle VirtualBox 7.2.16 had been installed**.

![VirtualBox Installation Complete](screenshots/virtualbox-installation-complete.png)

This confirmed that the VirtualBox software and its required host components had been installed successfully.

At this point, the Windows host was ready to launch VirtualBox Manager.

---

# Opening VirtualBox Manager

After the installation completed, **Oracle VirtualBox Manager** was opened from the Windows host.

The VirtualBox Manager provides the graphical interface used to create and manage virtual machines.

![VirtualBox Manager](screenshots/virtualbox-manager.png)

At this stage, no laboratory virtual machines had been created yet. The empty Manager interface therefore represents the starting point from which the Windows Server 2025 and Windows 11 Pro virtual machines will be built.

The VirtualBox Manager will subsequently be used to configure virtual hardware, attach ISO installation media, start and stop virtual machines, create snapshots and manage the virtual laboratory environment.

---

# Preparing the Virtualisation Environment

With VirtualBox Manager successfully running, the host environment was ready for the next stage of the project.

The installation itself does not create the Windows Server or Active Directory environment. It provides the virtualisation platform on which those components will be built.

The next chapter therefore moves from **preparing the hypervisor** to **creating and installing the Windows Server 2025 virtual machine**.

---

# Verification

The VirtualBox installation was considered successful after confirming that:

- Oracle VirtualBox 7.2.16 was installed.
- The installation completed without errors.
- Oracle VirtualBox Manager opened successfully.
- The VirtualBox Manager interface was available.
- The host was ready to create virtual machines.

---

# Key Learnings

During this chapter, I learned that:

- A hypervisor provides the virtualisation layer required to run virtual machines.
- Oracle VirtualBox can be used to create an isolated laboratory environment on a Windows host.
- VirtualBox Manager is the primary interface for creating and managing virtual machines.
- Virtual networking components are installed with VirtualBox and will later support communication between the laboratory systems.
- A properly prepared virtualisation platform is the foundation for the Windows Server and Active Directory environment.

---

# Skills Demonstrated

During this chapter, I demonstrated practical experience in:

- Installing desktop virtualisation software.
- Preparing a virtualisation environment.
- Verifying software installation.
- Navigating Oracle VirtualBox Manager.
- Understanding the role of a hypervisor within a laboratory environment.
- Preparing a platform for Windows Server deployment.
- Producing structured technical documentation using GitHub and Markdown.

---

# Interview Tip

For a First Line or IT Support role, understanding virtualisation is useful because IT support and infrastructure teams frequently use virtual machines for testing, troubleshooting, development and training.

A useful way to describe this laboratory experience in an interview is:

> “I built my Windows Server and Active Directory laboratory using Oracle VirtualBox. I installed and configured the hypervisor first, then used it to create isolated Windows Server and Windows 11 virtual machines for my hands-on administration work.”

This demonstrates practical familiarity with virtualisation rather than only theoretical knowledge.

---

# Chapter Summary

In this chapter, Oracle VirtualBox 7.2.16 was downloaded and installed on the Windows host computer.

The installation was completed using the standard installation options and was verified by successfully opening VirtualBox Manager.

At the end of the chapter, the host computer had a working virtualisation platform ready to create the Windows Server 2025 and Windows 11 Pro virtual machines required for the Active Directory laboratory.

The next stage of the project is to create and install the Windows Server 2025 virtual machine.

---

# Next Chapter

Continue directly to:

**[Chapter 2 – Installing Windows Server 2025](../02-Installing-Windows-Server/Installing-Windows-Server.md)**
