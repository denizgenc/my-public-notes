Link: [Understand Microsoft Entra ID
](https://learn.microsoft.com/en-us/training/modules/understand-azure-active-directory/) #

# Intro
Entra is cloud IAM service. Manages identities and access policies.

Learning objectives:
- Describe Microsoft Entra ID.
- Compare Microsoft Entra ID to Active Directory Domain Services (AD DS).
- Describe how Microsoft Entra ID is used as a directory for cloud apps.
- Describe Microsoft Entra ID P1 and P2.
- Describe Microsoft Entra Domain Services.

# Examine Entra ID
Entra ID is compared to Active Directory Directory Services (AD DS, usually just called "Active
Directory"). AD DS is a **directory service** (it allows users to get information on other users,
and also handles password based auth), which runs on Windows server - this server is called a
"domain controller".

Entra ID, on the other hand, is serverless and managed by Microsoft. It also has some features that
AD doesn't have, e.g. MFA, self-service password reset, and "identifying irregular sign-in
activity". It is primarily an **identity service**, though has some directory features.

Entra ID is part of the free tier of Azure. New Azure subscriptions will usually create a new Entra
tenant named "Default Directory".
- More advanced features are part of paid versions of Entra ID, which come in various tiers: Basic,
  Premium, P1, P2, etc. These are sometimes included as part of Microsoft 365 subscriptions.

## Tenants
Unlike AD, Entra is **multi-tenant** by design; isolation between directory instances.
- "Tenant" typically represents an entire organisation; but it technically represents an individual
  Entra instance.
- Creating additional tenants can be useful if you want to test functionality without affecting the
  existing deployment.

**An Azure subscription can only be associated with one Entra tenant at any given time.**
- You can associate the same tenant with multiple subscriptions.

Each tenant is assigned a default DNS name. This name is a unique prefix followed by
`onmicrosoft.com`.
- You can add one or more custom domains to tenants.

## Schema
Entra's schema contains fewer "object types" than AD. It doesn't have a "computer" class, though it
does have the "device" class.
- Joining devices to Entra is very different to joining computers to AD.
- You can extend the Entra schema, and reverse those extensions.

Because Entra doesn't have the concept of a "computer", you can't use Entra ID to do traditional
device management for computers (e.g. using Group Policy Objects {GPOs}).

There's more comparisons between Entra and AD described which doesn't really make much sense to me.

# Compare Entra ID and AD DS
AD DS is one part of the AD suite of technologies, which also includes Certificate Services (AD CS),
Lightweight Directory Services (AD LDS), Federation Services (AD FS) and Rights Management Services
(AD RMS).

AD DS has the following properties:
- It is a true directory service, which follows the hierarchical
  [X.500 standard](https://en.wikipedia.org/wiki/X.500)
- It uses DNS to locate resources
- Can be queried and managed using Lightweight Directory Access Protocol (LDAP) calls
- Uses Kerberos for authentication
- Uses OUs and GPOs for management of devices
- Includes computer objects
- Uses trusts between domains for delegated management.

AD DS can be deployed on a VM in Azure, so it's not like Entra ID obsoletes it.

Entra ID is slightly different:
- It is primarily an identity solution, and it uses HTTP(S) as the underlying protocol
  - Users and groups are created in a flat structure (no OUs or GPOs)
- Multi-tenant
- Uses SAML, OIDC or WS-Federation for authentication, and OAuth for authorisation.
  - Has federation services (so can use Facebook logins with Entra ID)

# Entra ID as a directory service for cloud apps
"Cloud Apps" here refers to M365 (formerly Office), or Intune.

Each of these cloud services will create their own Entra tenant; apparently this is more convenient.

Since Entra ID can federate (via SAML?), you can access cloud apps using SSO with logins from
Facebook, Google, etc.

# P1 and P2 plans
P1 and P2 provide more features compared to the Free and Office 365 editions.

P1 features:
- Allows users the ability to create and manage groups.
- Advanced security reports and alerts -> ML-powered reports, data based on access logs
- Multi-factor authentication (MFA)
- Microsoft Identity Manager (MIM) licensing. This can bridge existing on-prem directory services
  (AD DS, LDAP, Oracle) with Entra ID
- SLA of 99.9% (this is also available in the paid Basic and Premium tiers, mentioned above)
- Self-service password reset ("with writeback")
- Conditional access based on device, group or location
- Microsoft Entra Connect Health - seems to be a dashboard for reviewing usage patterns and
  configuration settings.
- Microsoft Entra Domain Services (see following section)

P2 has, in addition to the above P1 features, the following:
- Microsoft Entra ID Protection - for monitoring and protecting user accounts. User risk policies,
  sign-in policies, etc
- Microsoft Entra Privileged Identity Management - allows for fine grained security levels to be
  created for privileged users. Permanent and temporary admins, etc.

# Entra Domain Services
In an organisation, line-of-business (LOB) apps are deployed on devices that are domain members.
These orgs use AD DS-based credentials for authentication, and Group Policy manages them.

If these applications are moved into Azure, how do you manage authentication?
- Create a VPN to connect on-prem devices to Azure cloud?
  - Authentication traffic crosses VPN
- Deploy replica domain controllers as VMs in Azure?
  - Replicationi traffic crosses VPN, authentication traffic stays within cloud

Both of the above solutions incur additional costs and administrative overhead. Instead, use
Microsoft Entra Domain Services
- Provides Group Policy management, domain joining, Kerberos auth to Entra tenant
- Fully compatible with on-prem AD DS, so no need to deploy replica DCs in Azure

Can still use this even if you don't have AD DS deployed locally -> it basically acts as a domain
controller for your organisation.
- This means that sysadmins don't need to monitor and manage DC servers, or deal with replication.

However, there are limitations compared to full-fat AD DS:
- Only the base computer AD object is supported
- Not possible to extend the schema for the Microsoft Entra Domain Services domain
- The OU structure is flat and you can't have nested OUs
- Built in Group Policy Object (GPO), and it exists for computer and user accounts
- Not possible to target OUs with built-in GPOs

This service charges per hour based on the size of the directory
