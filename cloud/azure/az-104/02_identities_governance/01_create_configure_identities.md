Link to module: [Create, configure and manage
identities](https://learn.microsoft.com/en-gb/training/modules/create-configure-manage-identities/)

# Intro
One part of cloud security is creating access boundaries around resources. This access is controlled
centrally by doing the following:
1. Creating identities for users
2. Ensuring users have access through these identities to fulfil their aims in a secure manner

We can accomplish this using Microsoft Entra ID.

## Learning objectives
- Learn how to create, congfigure and manage users
- Learn how to create, congfigure and manage groups
- Manage licenses
- Explore custom security attributes and automatic provisioning

# Creating and managing users
Every user who needs access to Azure resources needs an Azure user account.

User accounts are tied to a specific directory, and only one directory can be edited at a time via
the Entra ID dashboard in Azure.

3 types of users
- **Cloud identities** - exist only in Entra ID (or can be sourced from "External Microsoft Entra
  directory" if defined in another instance, but requires access to resources controlled by this
  directory).
- **Directory-synchronised identities** - exist in an on-prem AD instance. Synchronising via Entra
  Connect brings them into Azure. The source is "Windows Server AD".
- **Guest users** - exist outside of Azure; e.g. other cloud providers via SAML, Microsoft accounts.
  The source is "Invited user".

# Creating and managing groups
Groups make it easier to add/remove permissions than doing it for each user individually.

Groups define a **security boundary**.

Two different groups in Entra ID:
- **Security groups** - most common. For defining who/what can access Azure resources.
- **Microsoft 365 groups** - defines who can access shared 365 tools (e.g. calendars, Sharepoint
  sites). You can invite guests to these groups.

There's three different types of group membership:
- **Assigned** - these users are added manually
- **Dynamic User** - added and removed automatically based certain attributes (e.g. department).
- **Dynamic Device** - same as the above, but for devices.
  - Microsoft 365 Groups do not support dynamic devices.
  - Dynamic users/devices require an Entra ID P1 license to implement.

# Configure and manage device registration
Devices have identities in Entra ID. This can be combined with Intune to ensure the device is
compliant.

## Microsoft Entra registered devices
Using **Microsoft Entra registered** (devices), you can get people to use their own devices without
requiring them to log in using an organisation account.
- Available on Windows 10+, MacOS 10.15+, Android, and Linux (Ubuntu LTS 20.04+, RHEL 8/9)
- Usually a Microsoft Entra account is signed on to for additional access to org resources

## Microsoft Entra joined devices
For devices that want to be cloud first/cloud only. This is where a user needs to sign in to an
organisational account to use their device -> **not BYOD**, org owns the device.
- Available on Windows 10+, Windows Server 2019+, or MacOS 13+
- Primarily intended for organisations that don't have an on-prem Active Directory.

## Hybrid Microsoft Entra joined devices
This is where the organisation has **on-premises AD**, and want to benefit from Entra ID.
- Devices that are joined to on-prem AD are registered to the organisation's Entra directory
- Organisation owns the device
- Windows 10+, Windows Server 2016, 2019, and 2022
- Device management can be handled by Group Policies set by the Active Directory, or hybrid with
  Microsoft Intune
- Use this when you have old apps that rely on AD for authentication, or want to keep using Group
  Policies

# Manage licenses
Some services cost money and require a license per seat. Managing these licenses on a per-user basis
is difficult, so you can use Entra ID to do **group-based licensing**, where licenses are assigned
(and removed) from users based on group membership.
- You need to have Entra ID Premium P1 or greater, or Office 365 Enterprise E3 or greater to use
  this feature (lol)
- This feature also depends on you _already having licenses available_ to assign to users in groups,
  i.e. it won't automatically procure new licenses if the group exceeds the amount you've already
  paid for.
- These are based on security groups
- Since users can be members of multiple groups, they might have the same product licensed to them
  "multiple times" - however Entra ID detects this and the license is consumed only once.

# Create custom security attributes
A custom security attribute is a key-value pair that can be assigned to Entra objects - basically
tags. This can be used for finer access control and management of Entra objects.
- These can support multiple values as well

# Automatic user creation
## SCIM (System for Cross-domain Identity Management)
todo
