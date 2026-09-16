# Group Policy

## Introduction

With the `techlab365.local` Active Directory environment operational and the Windows 11 workstation successfully joined to the domain, the next stage of the laboratory focuses on **Group Policy**.

Active Directory provides the directory structure used to manage users, computers and groups. Group Policy builds on that structure by allowing administrators to centrally configure Windows settings for those objects.

In a business environment, this avoids configuring every workstation manually. Instead, an administrator can create a policy once and apply it to the appropriate users or computers through the Active Directory hierarchy.

This chapter therefore moves from **managing Active Directory objects** to **centrally managing Windows configuration**.

For this laboratory, **Group Policy Management** was used to create a dedicated GPO named `TechLab365-Workstation-Policy`. Both **Computer Configuration** and **User Configuration** were configured so that the difference between computer-side and user-side policy processing could be demonstrated.

The chapter also contains a genuine troubleshooting scenario. The Windows 11 computer was initially left in the default `Computers` container. The custom policy was associated with the TechLab365 OU structure, so the computer was outside the intended scope. The computer was moved into the custom `TechLab365\Computers` OU and the policy was then successfully applied.

The final verification was performed on Windows 11 using `gpupdate` and `gpresult`, followed by direct verification of the actual Windows behaviour.

---

# Objectives

After completing this chapter, I will be able to:

- Understand the purpose of Group Policy.
- Explain what a Group Policy Object (GPO) is.
- Explain the difference between creating a GPO and linking a GPO.
- Understand the purpose of the Group Policy Management Console (GPMC).
- Understand the difference between Computer Configuration and User Configuration.
- Create a Group Policy Object.
- Navigate the Group Policy Management Editor.
- Configure a computer-side security policy.
- Configure a user-side Administrative Template policy.
- Understand how Active Directory OUs provide the scope for Group Policy.
- Explain basic Group Policy inheritance and processing.
- Refresh Group Policy on a Windows workstation.
- Use `gpresult` to identify applied policies.
- Troubleshoot a policy that is not being applied.
- Verify both policy processing and the actual client-side effect.

---

# Prerequisites

Before starting this chapter, ensure that:

- Windows Server 2025 has been installed successfully.
- `DC01` has been promoted to a Domain Controller.
- The `techlab365.local` Active Directory domain is operational.
- The custom `TechLab365` OU structure has been created.
- A `Users` OU exists under `TechLab365`.
- A `Computers` OU exists under `TechLab365`.
- The Windows 11 workstation `WIN11-01` has joined the domain.
- Sarah Wilson (`swilson`) is available as a domain user.
- Group Policy Management is available on the Domain Controller.

The Active Directory environment was created and the Windows 11 workstation was joined to the domain in the previous chapters.

---

# Lab Configuration

The Group Policy laboratory used the following configuration:

| Component | Configuration |
|---|---|
| Domain Controller | `DC01` |
| Server Operating System | Windows Server 2025 |
| Domain | `techlab365.local` |
| Windows 11 Workstation | `WIN11-01` |
| Domain User | Sarah Wilson (`swilson`) |
| User OU | `TechLab365\Users` |
| Computer OU | `TechLab365\Computers` |
| GPO | `TechLab365-Workstation-Policy` |
| Management Tool | Group Policy Management |

The existing OU structure was important because Group Policy can use OUs to define which users and computers should receive particular settings.

---

# Group Policy Overview

**Group Policy** is a Windows management technology that allows administrators to centrally configure operating system and user settings in an Active Directory environment.

Instead of logging on to every workstation and changing settings manually, an administrator can define a policy centrally.

For example, a company could use Group Policy to configure:

- Security settings.
- Windows logon behaviour.
- Desktop and Start menu settings.
- Windows components.
- Application-related settings.
- Network configuration.
- User restrictions.
- Administrative preferences.

The important concept is that Group Policy separates **policy definition** from **policy application**.

A simplified workflow is:

```text
Administrator
     ↓
Creates GPO
     ↓
Configures policy settings
     ↓
Links GPO to Active Directory location
     ↓
User or computer falls within the policy scope
     ↓
Windows processes the policy
     ↓
Settings are applied
```

This provides centralised configuration management across a domain.

---

# What Is a Group Policy Object?

A **Group Policy Object (GPO)** is the collection of policy settings that an administrator configures.

A GPO contains two main sections:

```text
Computer Configuration
        ↓
Settings processed for computers

User Configuration
        ↓
Settings processed for users
```

This distinction is important.

A computer policy is associated with the computer account and is normally processed when the computer starts or when Group Policy is refreshed.

A user policy is associated with the user account and is normally processed when the user signs in or when Group Policy is refreshed.

For example:

```text
WIN11-01
    ↓
Computer Configuration

Sarah Wilson
    ↓
User Configuration
```

The same GPO can contain both types of settings.

---

# GPO vs GPO Link

Creating a GPO does **not** automatically mean that it will affect every object in the domain.

There are two separate concepts:

### Group Policy Object

The GPO contains the settings.

```text
TechLab365-Workstation-Policy
        ↓
Contains configured policy settings
```

### GPO Link

The link associates the GPO with an Active Directory location.

```text
TechLab365 OU
        ↓
GPO link
        ↓
TechLab365-Workstation-Policy
```

This distinction is particularly important when troubleshooting.

A common mistake is to confirm that a setting exists inside a GPO and assume that the setting must therefore be affecting the workstation.

The setting may be correctly configured but still outside the scope of the target user or computer.

---

# Group Policy Management

The **Group Policy Management Console (GPMC)** is the main graphical administration tool used to manage domain Group Policy.

It can be opened on the Domain Controller from:

```text
Server Manager
→ Tools
→ Group Policy Management
```

The console provides access to:

- The Active Directory domain hierarchy.
- Organisational Units.
- Group Policy Objects.
- GPO links.
- GPO inheritance.
- Group Policy Management Editor.
- Group Policy Results.

The **Group Policy Management Editor** is used after selecting a particular GPO and choosing **Edit**.

This gives the administrator access to the two major configuration areas:

```text
Computer Configuration
User Configuration
```

---

# Creating the Workstation GPO

A dedicated GPO was created for the TechLab365 workstation policy.

In **Group Policy Management**:

1. Expand **Forest: `techlab365.local`**.
2. Expand **Domains**.
3. Expand **`techlab365.local`**.
4. Right-click **Group Policy Objects**.
5. Select **New**.
6. In the **Name** field, enter:

```text
TechLab365-Workstation-Policy
```

7. Leave **Source Starter GPO** set to `(none)`.
8. Click **OK**.

The new GPO then appeared under **Group Policy Objects**.

![TechLab365 Workstation Policy](screenshots/TechLab365-Workstation-Policy.png)

At this point the GPO existed, but no settings had yet been configured.

To configure it:

1. Right-click `TechLab365-Workstation-Policy`.
2. Select **Edit**.

This opens the **Group Policy Management Editor**.

---

# Understanding the Group Policy Management Editor

The Group Policy Management Editor separates settings into two main branches.

```text
TechLab365-Workstation-Policy
│
├── Computer Configuration
│
└── User Configuration
```

### Computer Configuration

Computer Configuration contains settings intended for the computer.

These settings can be processed regardless of which domain user subsequently signs in.

Examples include:

- Security options.
- Windows settings.
- Administrative Template settings.
- Startup and shutdown configuration.

### User Configuration

User Configuration contains settings intended for the user.

These settings follow the user account rather than the physical workstation.

Examples include:

- Start menu settings.
- Desktop configuration.
- Control Panel restrictions.
- User-specific Administrative Template settings.

This distinction is important for support technicians.

If a user reports that a policy is affecting them, the technician should first determine whether the setting belongs to **User Configuration** or **Computer Configuration**.

---

# Configuring Computer Configuration

The computer-side configuration used in this laboratory was an **Interactive Logon** message.

This is a useful teaching example because the policy is processed by the computer and produces an obvious result on the Windows sign-in screen.

The settings are located at:

```text
Computer Configuration
→ Policies
→ Windows Settings
→ Security Settings
→ Local Policies
→ Security Options
```

Two related settings were configured:

```text
Interactive logon: Message title for users attempting to log on

Interactive logon: Message text for users attempting to log on
```

The first setting controls the title and the second controls the message displayed below it.

---

## Configuring the Interactive Logon Message Title

In **Group Policy Management Editor**:

1. Expand **Computer Configuration**.
2. Expand **Policies**.
3. Expand **Windows Settings**.
4. Expand **Security Settings**.
5. Expand **Local Policies**.
6. Select **Security Options**.
7. Locate:

```text
Interactive logon: Message title for users attempting to log on
```

8. Right-click the setting.
9. Select **Properties**.
10. Select **Define this policy setting**.
11. Enter:

```text
TechLab365 IT Environment
```

12. Click **Apply**.
13. Click **OK**.

![Interactive Logon Message Title](screenshots/gpo-interactive-logon-message-title.png)

The configured value becomes the title displayed on the Windows sign-in screen.

---

## Configuring the Interactive Logon Message Text

The second setting controls the main message.

In **Security Options**:

1. Locate:

```text
Interactive logon: Message text for users attempting to log on
```

2. Right-click the setting.
3. Select **Properties**.
4. Select **Define this policy setting**.
5. Enter:

```text
This computer is part of the TechLab365 domain. Use only authorised accounts.
```

6. Click **Apply**.
7. Click **OK**.

![Interactive Logon Message Text](screenshots/gpo-interactive-logon-message-text.png)

The two settings work together:

```text
TechLab365 IT Environment

This computer is part of the TechLab365 domain.
Use only authorised accounts.
```

This demonstrates an important Group Policy concept: a visible Windows feature can be controlled centrally through a computer-side GPO rather than configured manually on each workstation.

---

# Configuring User Configuration

The second part of the GPO demonstrates a user-side setting.

The selected policy was:

```text
Remove Run menu from Start Menu
```

The setting is located at:

```text
User Configuration
→ Policies
→ Administrative Templates
→ Start Menu and Taskbar
→ Remove Run menu from Start Menu
```

This policy was selected because it produces a simple, visible change in the Windows user interface.

To configure it:

1. Expand **User Configuration**.
2. Expand **Policies**.
3. Expand **Administrative Templates**.
4. Select **Start Menu and Taskbar**.
5. Locate **Remove Run menu from Start Menu**.
6. Right-click the setting.
7. Select **Edit**.
8. Select **Enabled**.
9. Click **Apply**.
10. Click **OK**.

![Remove Run Menu Policy](screenshots/gpo-remove-run-menu-enabled.png)

The setting is now stored inside the GPO.

However, configuration alone does not prove that the policy will affect the intended user. The GPO must be within the user's policy scope and Windows must successfully process it.

---

# Understanding Policy Scope

The **scope** of a GPO determines which objects can receive its settings.

In Active Directory, GPOs can be linked at different levels of the hierarchy.

A simplified structure is:

```text
Forest
  ↓
Domain
  ↓
Organisational Unit
  ↓
Child Organisational Unit
  ↓
User / Computer
```

When a GPO is linked to an OU, objects in that OU can receive the policy. Child OUs can also inherit policies from parent OUs unless inheritance or filtering changes the result.

For example:

```text
techlab365.local
└── TechLab365
    ├── Users
    │    └── Sarah Wilson
    │
    └── Computers
         └── WIN11-01
```

A GPO associated with the `TechLab365` OU can therefore provide a common policy to the child `Users` and `Computers` OUs.

This is one reason why the OU structure created in the earlier Active Directory chapter is important: it provides the structure through which Group Policy can be organised.

---

# Linking the GPO

After creating and configuring the GPO, it was linked into the TechLab365 OU structure.

In **Group Policy Management**:

1. Expand **`techlab365.local`**.
2. Locate the **TechLab365** OU.
3. Right-click the OU.
4. Select **Link an Existing GPO**.
5. Select:

```text
TechLab365-Workstation-Policy
```

6. Click **OK**.

The GPO is now linked to the Active Directory hierarchy.

The important distinction is:

```text
GPO created
      ↓
Settings configured
      ↓
GPO linked
      ↓
Policy can enter the processing scope
```

A GPO that exists only under **Group Policy Objects** but is not linked into the appropriate scope will not automatically affect the intended objects.

---

# Group Policy Processing

Once a GPO is within scope, Windows must process the policy.

The simplified processing sequence is:

```text
Active Directory location identified
            ↓
Applicable GPOs determined
            ↓
Computer policy processed
            ↓
User signs in
            ↓
User policy processed
            ↓
Configured settings applied
```

The exact processing behaviour depends on the type of policy, the object, the hierarchy and other Group Policy conditions.

For troubleshooting purposes, two commands are particularly useful:

```text
gpupdate
```

and:

```text
gpresult
```

`gpupdate` requests a Group Policy refresh.

`gpresult` reports the resulting Group Policy information for the selected user or computer.

These commands are especially useful because they allow a support technician to move from:

```text
"I configured the GPO"
```

to:

```text
"The workstation actually received the GPO"
```

---

# Understanding Group Policy Inheritance

Group Policy follows the Active Directory hierarchy.

For example:

```text
techlab365.local
        ↓
    TechLab365
        ↓
   ┌────┴────┐
 Users    Computers
   ↓          ↓
Sarah      WIN11-01
```

A policy linked at a higher level can be inherited by objects below it.

This means that an administrator does not necessarily need to create a separate GPO for every OU.

For example, a common workstation policy can be linked to a parent OU and inherited by a child `Computers` OU.

More specific policies can also affect the final configuration when settings conflict.

This is why Group Policy troubleshooting should consider the complete hierarchy rather than looking only at the setting inside one GPO.

---

# A Real Group Policy Troubleshooting Scenario

The laboratory produced a genuine Group Policy troubleshooting scenario.

After configuring the GPO, the policy was refreshed on Windows 11 and the resulting computer policy was checked.

The GPO did not initially appear in the applied computer policies.

This meant that the next step was not to randomly change the policy settings.

Instead, the Active Directory location of the computer account was checked.

---

## Initial Computer Policy Check

On Windows 11, open **PowerShell as an administrator** and run:

```powershell
gpupdate /force
```

The command completed the computer and user policy update.

The computer-side policy result was then checked with:

```powershell
gpresult /r /scope computer
```

The report showed that `WIN11-01` was initially located in the default container:

```text
CN=WIN11-01,CN=Computers,DC=techlab365,DC=local
```

The custom OU created in the earlier Active Directory chapter was:

```text
OU=Computers,OU=TechLab365,DC=techlab365,DC=local
```

These are different Active Directory locations.

The important troubleshooting finding was therefore that the workstation was still in the default `Computers` container instead of the custom `TechLab365\Computers` OU.

![GPO Troubleshooting - Wrong Computer Container](screenshots/gpo-troubleshooting-wrong-computer-container.png)

This is a realistic Service Desk and junior administrator troubleshooting principle:

> Before changing a working policy, verify that the affected object is actually within the intended policy scope.

---

# Moving WIN11-01 to the Correct OU

The computer object was moved using **Active Directory Users and Computers**.

1. Open **Active Directory Users and Computers**.
2. Expand `techlab365.local`.
3. Locate the default **Computers** container.
4. Right-click `WIN11-01`.
5. Select **Move**.
6. Expand **TechLab365** in the destination tree.
7. Select **Computers**.
8. Click **OK**.

The computer was now located in:

```text
CN=WIN11-01,OU=Computers,OU=TechLab365,DC=techlab365,DC=local
```

This placed the workstation inside the custom OU structure used for the laboratory's workstation policies.

---

# Refreshing Computer Policy

After correcting the computer's Active Directory location, Group Policy was refreshed again.

Open **PowerShell as an administrator**.

### Command 1

```powershell
gpupdate /force
```

This requests an immediate refresh of Group Policy.

The command reported:

```text
Computer Policy update has completed successfully.
User Policy update has completed successfully.
```

A successful `gpupdate` is useful, but it does not by itself prove that a particular GPO was applied.

Therefore a second command was used.

### Command 2

```powershell
gpresult /r /scope computer
```

The report now showed the computer in the custom OU:

```text
CN=WIN11-01,OU=Computers,OU=TechLab365,DC=techlab365,DC=local
```

Under **Applied Group Policy Objects**, the report showed:

```text
TechLab365-Workstation-Policy
Default Domain Policy
```

![Computer Policy Applied](screenshots/gpo-computer-policy-applied.png)

This provided direct evidence that the custom workstation GPO was now being processed by `WIN11-01`.

---

# Verifying the Computer Policy Effect

The next step was to verify the actual effect of the computer-side policy.

The configured policy was the Interactive Logon message.

The workstation was signed out so that the Windows sign-in screen could be examined.

The configured message appeared:

```text
TechLab365 IT Environment

This computer is part of the TechLab365 domain. Use only authorised accounts.
```

![Windows 11 Interactive Logon Message](screenshots/win11-interactive-logon-message.png)

This is stronger evidence than simply seeing the setting configured in GPMC.

The verification sequence was:

```text
Policy configured
      ↓
GPO within scope
      ↓
gpupdate /force
      ↓
gpresult confirms GPO
      ↓
Windows displays configured result
```

This demonstrates both **policy processing** and **actual client-side effect**.

---

# Verifying User Configuration

The user-side policy was verified while logged into Windows 11 as **Sarah Wilson**.

The current identity was first confirmed using:

```cmd
whoami
```

The result was:

```text
techlab365\swilson
```

This is important because `gpresult /scope user` reports information for the user context being examined.

The user policy was then refreshed.

### Command 1

```cmd
gpupdate /force
```

### Command 2

```cmd
gpresult /r /scope user
```

The report showed Sarah's Active Directory location:

```text
CN=Sarah Wilson,OU=Users,OU=TechLab365,DC=techlab365,DC=local
```

Under **Applied Group Policy Objects**, the report showed:

```text
TechLab365-Workstation-Policy
```

![User Policy Applied](screenshots/gpo-user-policy-applied.png)

This confirmed that the GPO was being processed in Sarah Wilson's user context.

---

# Troubleshooting User Policy Verification

During the user-policy verification process, an initial result did not provide the expected information for Sarah Wilson.

The important troubleshooting question was:

```text
Which user account is actually being examined?
```

`gpresult /scope user` reports the user context of the account currently being examined. Therefore, running the command while logged into another account does not produce Sarah Wilson's user policy result.

The correct procedure was:

1. Sign in as Sarah Wilson.
2. Confirm the identity with:

```cmd
whoami
```

3. Refresh Group Policy:

```cmd
gpupdate /force
```

4. Generate the user policy result:

```cmd
gpresult /r /scope user
```

The initial troubleshooting result was retained as evidence because it demonstrates that Group Policy troubleshooting requires the correct user context.

![User Policy Not Applied](screenshots/gpo-user-policy-not-applied.png)

The successful result for Sarah Wilson subsequently confirmed the expected GPO.

This is a useful support lesson because a policy may appear to be missing when the technician is actually checking the wrong security context.

---

# Verifying the User Policy Effect

The configured User Configuration policy was:

```text
Remove Run menu from Start Menu
```

After the user policy was processed, the **Run** option was no longer available from the Start menu.

![Run Menu Removed](screenshots/gpo-user-policy-effect-run-removed.png)

This provided direct evidence that the user-side policy was not only configured and reported as applied, but was also producing the expected change on Windows 11.

---

# Understanding the Complete Verification Process

A Group Policy administrator should ideally verify a policy at several levels.

### Level 1 - Configuration

Check the setting inside the GPO.

```text
GPMC
    ↓
GPO
    ↓
Policy setting
```

This answers:

> Is the policy configured?

### Level 2 - Scope

Check the Active Directory location and GPO link.

```text
GPO
    ↓
OU / domain
    ↓
Target object
```

This answers:

> Should this user or computer receive the policy?

### Level 3 - Processing

Use:

```text
gpupdate /force
```

This answers:

> Has Windows requested a policy refresh?

### Level 4 - Result

Use:

```text
gpresult
```

This answers:

> Did Windows actually process the GPO?

### Level 5 - Client Effect

Check Windows itself.

This answers:

> Did the configured setting actually change the expected behaviour?

The laboratory demonstrated all of these stages.

---

# Group Policy Troubleshooting Method

When a GPO does not appear to work, troubleshooting should be systematic.

A useful sequence is:

```text
1. Confirm the GPO exists
          ↓
2. Confirm the setting is configured
          ↓
3. Confirm the GPO is linked
          ↓
4. Check the target object's OU
          ↓
5. Refresh Group Policy
          ↓
6. Run gpresult
          ↓
7. Check for filtering / inheritance issues
          ↓
8. Verify the actual Windows effect
```

---

## Step 1 - Confirm the GPO Exists

Open **Group Policy Management** and check:

```text
Domain
→ Group Policy Objects
```

Confirm that:

```text
TechLab365-Workstation-Policy
```

exists.

If it does not exist, there is no GPO to apply.

---

## Step 2 - Confirm the Setting Is Configured

Edit the GPO and check the relevant branch:

```text
Computer Configuration
```

or:

```text
User Configuration
```

Confirm that the required setting is not:

```text
Not Configured
```

A GPO can exist while containing no effective settings.

---

## Step 3 - Confirm the GPO Is Linked

In Group Policy Management, select the relevant domain or OU and check the linked GPOs.

A GPO that exists under **Group Policy Objects** but has no relevant link is not automatically applied simply because it exists.

---

## Step 4 - Check the Target Object's Location

For a computer policy:

1. Open **Active Directory Users and Computers**.
2. Locate the computer account.
3. Check its OU or container.

For a user policy:

1. Locate the user account.
2. Check its OU or container.

This was the critical step in the laboratory troubleshooting scenario.

`WIN11-01` was initially in:

```text
CN=Computers
```

instead of:

```text
OU=Computers,OU=TechLab365
```

---

## Step 5 - Refresh Group Policy

On the client:

```powershell
gpupdate /force
```

A successful refresh indicates that Windows completed the requested Group Policy update.

It does not by itself prove that the required GPO was applied.

---

## Step 6 - Use gpresult

For computer policy:

```powershell
gpresult /r /scope computer
```

For user policy:

```powershell
gpresult /r /scope user
```

Look under:

```text
Applied Group Policy Objects
```

This is one of the most useful first-line checks when investigating whether a GPO reached a Windows workstation.

---

## Step 7 - Check Scope and Processing Conditions

If the GPO is still not applied, investigate:

- Active Directory object location.
- GPO link.
- GPO inheritance.
- Security filtering.
- Whether the setting belongs to User or Computer Configuration.
- Whether the correct user is being examined.
- Whether the computer can communicate with the Domain Controller.

The laboratory demonstrated two particularly useful checks:

```text
Wrong computer OU
```

and:

```text
Wrong user context
```

Both can make a correctly configured GPO appear not to work.

---

## Step 8 - Verify the Actual Windows Effect

Finally, check the workstation itself.

For this laboratory:

```text
Computer policy
→ Interactive Logon message
```

and:

```text
User policy
→ Run menu removed
```

The actual Windows effect is the final confirmation that the policy has achieved its intended result.

---

# Service Desk Perspective

Group Policy knowledge is particularly useful in First Line and Service Desk environments because support technicians frequently encounter settings that are centrally controlled.

For example, a user may report:

> "The Run option has disappeared from my Start menu."

A technician should not immediately assume that Windows is damaged.

A structured investigation could be:

```text
Identify the user
      ↓
Confirm the computer
      ↓
Check whether the device is domain joined
      ↓
Check Group Policy processing
      ↓
Run gpresult
      ↓
Identify the applied GPO
      ↓
Check the configured setting
```

Similarly, if a workstation is missing a required computer policy, checking the computer's Active Directory OU can quickly identify a scope problem.

The troubleshooting performed in this laboratory therefore reflects a realistic support workflow rather than simply demonstrating how to click through GPMC.

---

# Verification

The Group Policy administration tasks were considered successful after confirming that:

- The `TechLab365-Workstation-Policy` GPO was created.
- The GPO was visible under **Group Policy Objects**.
- Computer Configuration was configured with an Interactive Logon message title.
- Computer Configuration was configured with an Interactive Logon message text.
- User Configuration was configured with **Remove Run menu from Start Menu**.
- The GPO was linked into the TechLab365 OU structure.
- `WIN11-01` was identified in the wrong default `Computers` container during troubleshooting.
- `WIN11-01` was moved into the custom `TechLab365\Computers` OU.
- `gpupdate /force` completed successfully.
- `gpresult /r /scope computer` showed `TechLab365-Workstation-Policy` as applied.
- The computer-side Interactive Logon message appeared on Windows 11.
- `whoami` confirmed the Sarah Wilson domain identity as `techlab365\swilson`.
- `gpresult /r /scope user` showed `TechLab365-Workstation-Policy` as applied for Sarah Wilson.
- The Run option was removed from the Windows 11 Start menu.

The verification therefore covered configuration, scope, policy processing and the resulting Windows behaviour.

---

# Key Learnings

During this chapter, I learned that:

- Group Policy provides centralised configuration management in an Active Directory environment.
- A GPO is a collection of policy settings.
- Creating a GPO does not automatically make it apply to every object.
- A GPO must be within the appropriate policy scope.
- GPO links connect policy objects to the Active Directory hierarchy.
- Organisational Units provide an important structure for Group Policy deployment.
- Computer Configuration contains settings intended for computers.
- User Configuration contains settings intended for users.
- The same GPO can contain both computer and user settings.
- Group Policy follows the Active Directory hierarchy and can be inherited by child OUs.
- The location of an Active Directory object can determine whether a GPO is within scope.
- The default `Computers` container and a custom `Computers` OU are different locations.
- `gpupdate /force` requests an immediate Group Policy refresh.
- A successful `gpupdate` does not prove that a particular GPO was applied.
- `gpresult` can be used to examine the resulting Group Policy state.
- `gpresult /scope computer` investigates the computer policy context.
- `gpresult /scope user` investigates the current user's policy context.
- The correct user account must be used when investigating user-side policy.
- Policy configuration should be checked separately from policy application.
- Actual Windows behaviour should be checked after confirming the GPO is applied.
- Group Policy troubleshooting should follow evidence rather than assumptions.
- Checking Active Directory object location is an important first troubleshooting step.
- A correctly configured GPO can appear not to work when the target object is outside its intended scope.
- Group Policy is relevant to everyday Service Desk troubleshooting because many Windows settings are centrally controlled.

---

# Skills Demonstrated

During this chapter, I demonstrated practical experience in:

- Group Policy Management.
- Group Policy Management Console (GPMC).
- Group Policy Object creation.
- GPO configuration.
- GPO linking.
- Computer Configuration.
- User Configuration.
- Administrative Templates.
- Windows Security Options.
- Interactive Logon policy configuration.
- Active Directory OU administration.
- Computer object management.
- Moving computer accounts between Active Directory containers and OUs.
- Group Policy processing.
- `gpupdate`.
- `gpresult`.
- Computer-side policy verification.
- User-side policy verification.
- Client-side policy verification.
- Group Policy troubleshooting.
- Active Directory scope troubleshooting.
- User-context troubleshooting.
- Windows 11 domain administration.
- First Line IT Support troubleshooting methodology.
- Technical documentation using GitHub and Markdown.

---

# Interview Tip

For a First Line or Service Desk role, a useful way to describe this practical experience is:

> "I created and administered a Group Policy Object in my Windows Server 2025 Active Directory lab. I configured both Computer Configuration and User Configuration, linked the GPO into the Active Directory OU structure, and used gpupdate and gpresult to verify policy processing. I also troubleshot a real issue where the Windows 11 computer was still in the default Computers container instead of the custom OU, moved the computer to the correct OU and then verified the policy effects on the client."

This demonstrates practical Group Policy administration together with a structured troubleshooting approach.

---

# Chapter Summary

In this chapter, Group Policy was introduced as the mechanism used to centrally manage Windows settings within the `techlab365.local` Active Directory environment.

A dedicated GPO named `TechLab365-Workstation-Policy` was created and configured using the Group Policy Management Editor. The GPO demonstrated both major configuration areas.

Under **Computer Configuration**, an Interactive Logon message was configured with the title:

```text
TechLab365 IT Environment
```

and the message:

```text
This computer is part of the TechLab365 domain. Use only authorised accounts.
```

Under **User Configuration**, the **Remove Run menu from Start Menu** policy was enabled to demonstrate a user-side Windows configuration.

The chapter also demonstrated the relationship between Active Directory OUs and Group Policy scope. During the initial verification, `WIN11-01` was found in the default `Computers` container rather than the custom `TechLab365\Computers` OU. As a result, the workstation was outside the intended policy scope.

The computer object was moved to the correct OU and Group Policy was refreshed using:

```text
gpupdate /force
```

The resulting policy state was then checked using:

```text
gpresult /r /scope computer
```

The custom GPO appeared under **Applied Group Policy Objects**, confirming that the computer was receiving the policy.

The computer-side result was then verified directly on Windows 11 when the configured Interactive Logon message appeared.

The user-side policy was subsequently verified while logged in as Sarah Wilson. The domain identity was confirmed with `whoami`, and `gpresult /r /scope user` confirmed that the GPO was applied in the correct user context. The removal of the Run option provided the final visible verification.

The chapter therefore demonstrated the complete Group Policy administration and troubleshooting workflow:

```text
Understand
    ↓
Create GPO
    ↓
Configure settings
    ↓
Link GPO
    ↓
Check Active Directory scope
    ↓
Refresh policy
    ↓
Check gpresult
    ↓
Verify Windows effect
    ↓
Troubleshoot if necessary
```

The Active Directory environment is now ready for the next stage of the laboratory: DNS administration.

---

# Next Chapter

Continue directly to:

**[Chapter 10 – DNS Administration](../10-DNS-Administration/DNS-Administration.md)**
