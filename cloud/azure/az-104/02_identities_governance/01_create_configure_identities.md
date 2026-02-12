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

# TODO: rest of units
