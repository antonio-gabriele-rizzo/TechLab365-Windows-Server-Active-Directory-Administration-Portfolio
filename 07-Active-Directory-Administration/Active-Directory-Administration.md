# Active Directory Administration

## Introduction

With the `techlab365.local` Active Directory domain successfully deployed in the previous chapter, the next stage of the laboratory focuses on practical **Active Directory administration**.

The objective of this chapter is to simulate common administrative tasks performed by IT Support and Service Desk teams, including managing users, groups, passwords, account status and account security policies.

For this laboratory, **Active Directory Users and Computers (ADUC)** and **Group Policy Management** were used to administer the domain and simulate realistic support scenarios.

By the end of this chapter, the Active Directory environment contains a structured organisational hierarchy, departmental security groups and user accounts, together with an Account Lockout Policy designed to simulate a common locked-account scenario.

---

# Objectives

After completing this chapter, I will be able to:

- Review the Active Directory domain structure.
- Create and manage Organisational Units.
- Create Active Directory user accounts.
- Create and manage security groups.
- Assign users to departmental groups.
- Review user group membership.
- Reset a user password.
- Disable a user account.
- Configure an Account Lockout Policy.
- Verify domain account lockout settings.
- Understand a common locked-account Service Desk scenario.

---

# Prerequisites

Before starting this chapter, ensure that:

- Windows Server 2025 has been installed successfully.
- The server has been promoted to a Domain Controller.
- The `techlab365.local` Active Directory domain has been created.
- DNS is operational.
- Active Directory Users and Computers is available.
- Group Policy Management is available.

The Active Directory infrastructure was deployed and verified in the previous chapter.

---

# Lab Configuration

The Active Directory administration environment used the following configuration:

| Component | Configuration |
|---|---|
| Server Name | `DC01` |
| Server Operating System | Windows Server 2025 |
| Domain | `techlab365.local` |
| Domain Controller | `DC01.techlab365.local` |
| Administration Tools | Active Directory Users and Computers, Group Policy Management |

---

# Active Directory Administration Overview

Active Directory provides centralised management of identities and resources within a Windows domain environment.

Common administrative tasks include managing:

- User accounts
- Computer accounts
- Security groups
- Organisational Units
- Authentication
- Passwords
- Account status
- Security policies

For First Line IT Support and Service Desk roles, common tasks often include creating users, resetting passwords, managing group membership, disabling accounts and troubleshooting account access issues.

---

# Reviewing the Active Directory Domain Structure

The `techlab365.local` domain was reviewed using **Active Directory Users and Computers**.

The default Active Directory structure contains containers such as:

- Builtin
- Computers
- Domain Controllers
- Foreign Security Principals
- Managed Service Accounts
- Users

![Active Directory Domain Structure](screenshots/aduc-domain-structure.png)

Although these default containers are created automatically, Organisational Units can be used to create a more logical administrative structure.

---

# Creating an Organisational Unit Structure

A custom Organisational Unit structure was created for the TechLab365 environment.

The structure included dedicated areas for:

- Users
- Computers
- Servers

![Organisational Unit Structure](screenshots/aduc-organizational-unit-structure.png)

Organisational Units help administrators organise Active Directory objects and can also be used to:

- Apply Group Policy.
- Delegate administrative permissions.
- Separate departments or business functions.
- Organise users and computers logically.

---

# Creating Departmental Security Groups

Security groups were created to represent different departments within the organisation.

The groups included:

- IT Support
- HR
- Finance

![New Group Details](screenshots/aduc-new-group-details.png)

Security groups allow permissions and access to be managed based on job role rather than assigning permissions individually to every user.

---

# Creating Active Directory User Accounts

Several user accounts were created to simulate employees within the organisation.

![New User Details](screenshots/aduc-new-user-details.png)

After creation, the user accounts were visible in Active Directory Users and Computers.

![Active Directory Users Created](screenshots/aduc-users-created.png)

Creating and managing user accounts is one of the most common Active Directory tasks performed by IT Support teams.

---

# Managing Departmental Group Membership

Users were assigned to the appropriate departmental security groups.

## IT Support Group

![IT Support Group Members](screenshots/aduc-it-support-members.png)

## HR Group

![HR Group Members](screenshots/aduc-hr-team-members.png)

## Finance Group

![Finance Group Members](screenshots/aduc-finance-team-members.png)

Group membership was also reviewed from the user account perspective.

![User Group Membership](screenshots/aduc-user-group-membership.png)

Using security groups simplifies access management and provides a more scalable approach than assigning permissions directly to individual users.

---

# Password Reset Scenario

A common Service Desk request involves a user who has forgotten their password.

To simulate this scenario, the password for **Michael Brown** was reset using Active Directory Users and Computers.

The password reset process was initiated from the user account context menu.

![Password Reset Menu](screenshots/aduc-reset-password-menu.png)

The new password and account options were then configured.

![Password Reset Details](screenshots/aduc-reset-password-details.png)

Active Directory confirmed that the password had been successfully changed.

![Password Reset Confirmation](screenshots/aduc-password-reset-confirmation.png)

When performing password resets in a real organisation, identity verification and company security procedures should be followed before changing user credentials.

---

# Disabling a User Account

Another common Active Directory administration task is disabling a user account.

Accounts may need to be disabled when:

- An employee leaves the organisation.
- Access needs to be temporarily suspended.
- A security investigation is required.

For this scenario, the account belonging to **Sarah Wilson** was disabled.

![Disable User Account](screenshots/aduc-disable-user-account.png)

Active Directory then confirmed that the account had been disabled.

![User Account Disabled Confirmation](screenshots/aduc-user-disabled-confirmation.png)

Disabling an account prevents authentication while retaining the Active Directory object and its associated information.

---

# Configuring the Account Lockout Policy

A common IT Support scenario occurs when a user repeatedly enters an incorrect password and becomes locked out.

The Account Lockout Policy was configured through **Group Policy Management**.

The policy location is:

```text
Computer Configuration
→ Policies
→ Windows Settings
→ Security Settings
→ Account Policies
→ Account Lockout Policy
```

The following settings were configured:

| Policy | Configuration |
|---|---|
| Account lockout threshold | 5 invalid logon attempts |
| Account lockout duration | 30 minutes |
| Reset account lockout counter after | 30 minutes |
| Allow Administrator account lockout | Enabled |

![Account Lockout Policy Configured](screenshots/account-lockout-policy-configured.png)

The policy was configured to simulate a realistic balance between security and usability within the laboratory environment.

---

# Verifying the Account Lockout Policy

After configuring the policy, the settings were reviewed to confirm that the configuration had been successfully applied.

The final configuration confirmed:

- 5 invalid logon attempts.
- 30-minute account lockout duration.
- 30-minute reset counter period.
- Administrator account lockout enabled.

![Account Lockout Policy Check](screenshots/account-lockout-policy-check.png)

This policy helps protect against repeated invalid authentication attempts and provides a realistic scenario for Service Desk account troubleshooting.

---

# Locked Account Service Desk Scenario

A typical Service Desk request could be:

> “I forgot my password and entered the wrong password too many times. Now I cannot log in.”

A structured troubleshooting process would include:

1. Verify the user's identity according to company procedures.
2. Check whether the account is locked.
3. Determine whether a password reset is required.
4. Unlock the account if necessary.
5. Confirm that the account is enabled.
6. Ask the user to attempt to sign in again.

The Windows 11 client has not yet joined the domain, so the complete client-side lockout scenario will be demonstrated later after the workstation has been joined to `techlab365.local`.

---

# Verification

The Active Directory administration tasks were considered successful after confirming that:

- The `techlab365.local` domain structure was accessible.
- A custom Organisational Unit structure was created.
- Departmental security groups were created.
- User accounts were created successfully.
- Users were assigned to the appropriate groups.
- User group membership was verified.
- A user password was successfully reset.
- Password reset confirmation was received.
- A user account was successfully disabled.
- The Account Lockout Policy was configured.
- The lockout threshold was configured for 5 invalid logon attempts.
- The lockout duration was configured for 30 minutes.
- The lockout counter reset period was configured for 30 minutes.

The Active Directory environment is now prepared for the next stage of the laboratory.

---

# Key Learnings

During this chapter, I learned that:

- Active Directory Users and Computers is used for day-to-day administration of users, groups and Organisational Units.
- Organisational Units provide a logical structure for organising Active Directory objects.
- Security groups simplify access management.
- Group membership can be used to manage access based on job role.
- Password resets are a common First Line IT Support responsibility.
- User accounts can be disabled without deleting the Active Directory object.
- Group Policy can be used to configure domain-wide account security settings.
- Account Lockout Policies help protect against repeated invalid authentication attempts.
- Locked-account issues should be handled using a structured troubleshooting process.

---

# Skills Demonstrated

During this chapter, I demonstrated practical experience in:

- Active Directory Users and Computers.
- Active Directory user administration.
- User account creation.
- Password resets.
- User account disabling.
- Security group creation.
- Group membership management.
- Organisational Unit administration.
- Group Policy Management.
- Account Lockout Policy configuration.
- Domain account security configuration.
- Basic account access troubleshooting.
- First Line IT Support scenarios.
- Service Desk administration tasks.
- Producing technical documentation using GitHub and Markdown.

---

# Interview Tip

For a First Line or IT Support role, Active Directory administration is frequently listed as a required skill.

A useful way to describe this laboratory experience in an interview is:

> “I built and administered an Active Directory environment in my Windows Server 2025 home lab. I created an organisational structure, user accounts and departmental security groups, managed group membership, performed password resets and disabled user accounts. I also configured a domain Account Lockout Policy using Group Policy Management to simulate a common Service Desk scenario involving repeated incorrect password attempts.”

This demonstrates practical familiarity with common identity and access management tasks rather than only theoretical knowledge.

---

# Chapter Summary

In this chapter, the `techlab365.local` Active Directory environment was used to perform common administrative tasks.

The domain structure was reviewed and a custom Organisational Unit structure was created. User accounts and departmental security groups were created, and users were assigned to the appropriate groups.

Common Service Desk scenarios were also simulated. A password reset was performed for Michael Brown and Sarah Wilson's account was disabled to demonstrate account lifecycle management.

Finally, an Account Lockout Policy was configured through Group Policy Management. The policy was configured to lock accounts after five invalid logon attempts for thirty minutes.

The Active Directory environment is now ready for the next stage of the laboratory: configuring and joining the Windows 11 client to the `techlab365.local` domain.

---

# Next Chapter

Continue directly to:

**[Chapter 8 – Joining Windows 11 to the Domain](../08-Joining-Windows-11-to-the-Domain/Joining-Windows-11-to-the-Domain.md)**
