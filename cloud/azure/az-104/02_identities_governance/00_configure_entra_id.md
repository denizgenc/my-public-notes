Link to module: [Configure Microsoft Entra
ID](https://learn.microsoft.com/en-us/training/modules/configure-azure-active-directory/)

Topics covered in this module:
- Tenants
- Identities
- Accounts
- Azure subscriptions

An example implementation of Entra ID is where it is synced with a (local) Windows Server AD to use
the same users and groups to authenticate and authorise entities. However, while Active Directory
uses Kerberos and NTLM auth to provide access to on-prem applications, Entra ID provides SAML,
OAuth, Open ID and WS-Federation (?) to provide access to cloud resources and applications.

Entra ID can be used for web apps that are hosted on-premises, enabling MFA, conditional access,
etc. It also allows you to delegate certain administrative tasks to employees, allowing them to
self-service for stuff like password resets.

# Concepts
- **Identity** - Something that can be authenticated; not just a user but also services or servers
  (which might require certificates, etc).
- **Account** - An identity that has data associated with it.
- **Microsoft Entra/Azure AD account** - an identity/account created through Entra or a cloud
  service/app, e.g. Microsoft 365. These accounts are accessible to your Azure subscriptions.
  - Also called a "work" or "school" account
- **Azure tenant (directory)** - Represents a single organisation. You get one automatically when
  your organisation starts using a cloud service, but you can create more. Each Azure subscription
  is assigned to a tenant.

# AD DS vs Entra ID
AD DS == Active Directory Domain Services (which also includes Certificate Services [AD CS],
Lightweight Directory Services (AD LDS), Federation Services (AD FS) and Rights Management Services
(AD RMS).

Entra is similar, but it's a full identity solution whereas AD DS is mostly for directory services.
Also, since Entra is a web service, it uses HTTP(S) protocols (e.g. SAML, OAuth) for authorisation
and authentication, whereas AD DS can use Kerberos. Because of those protocols (I think), Entra can
federate with other platforms like Facebook for authorisation/identity.

In terms of operation:
- Entra users and groups are in a flat structure; AD DS requires them to be placed within
  organisational units (OUs) or group policy objects (GPOs)
- And of course, Entra is a managed service, so you don't have to worry about patching your AD DS
  server.

# Editions
There are four:
- Free
- Microsoft 365 Apps
- Premium P1
- Premium P2

All support the following features
- Directory objects (Free can have up to 500k, the others are all unlimited)
- Single Sign-on
- Core IAM
- Business-to-business collaboration (?)

Microsoft 365 Apps adds IAM for M365 apps. This allows for using your own branding on sign-on pages,
MFA, group access management, and also self-service password reset for cloud apps

P1 adds the following:
- ["Premium features"](https://www.youtube.com/watch?v=hJ9yBgTp9UQ), helpful
  - "supports advanced administration like dynamic groups, self-service group management, and cloud
    write-back capabilities"
- Hybrid identities (through Microsoft Identity Manager?)
- Advanced group access management
- Conditional access
- Self-service password reset for on-prem users.

P2 has all of the above, and:
- Identity protection (Microsoft Entra ID Protection)
- Identity governance

[Full comparison on the pricing
page](https://www.microsoft.com/en-gb/security/business/microsoft-entra-pricing)

# Entra Join
I don't really understand what this is. It seems to be a way of linking devices (running Windows?)
to a domain/Entra ID tenant so that they can be authenticated using the device itself.

There's two ways of connecting devices to Entra ID:
- Registering the device, which provisions an identity to it, which is tracked in the Entra ID
  tenant. This identity can be used to enable or disable sign in from the device.
- Joining the device, which extends registration by adding Entra ID accounts to the device. This
  allows people to sign in to the device using their work/school account instead of their personal
  one.

It's a good idea to combine this with MDM (like Intune) to control/lock down the device as well.

# Self-service password reset (SSPR)
To enable Entra SSPR:
- Requires an account with Global Administrator privileges, so it can be used to configure SSPR.
  (This account will always be able to reset their own passwords)
- Requires a security group to limit who can SSPR
  - You can select SSPR to be enabled on "All", so is a security group required in that case?
- "All user accounts in your organisation must have a valid license to use SSPR" (?)

It's recommended that there's MFA enabled for all accounts that can SSPR (e.g. text/email
notification). SSPR requires at least one "authentication method".
