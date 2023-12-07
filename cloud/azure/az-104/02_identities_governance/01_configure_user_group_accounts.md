Link to module: [Configure user and group
accounts](https://learn.microsoft.com/en-us/training/modules/configure-user-group-accounts/)

# User accounts
Three types of user account in Entra ID:
- Cloud identity - Defined only in Entra ID; includes administrators
- Directory synchronised identity - Defined in an AD server; synchronised via Entra Connect.
- Guest user - defined outside of Azure, such as Facebook or Xbox Live. "The source for guest user
  accounts is Invited user" - wtf does that mean

There are two ways to add a new user - either by creating one directly in Azure, or to invite them
(which will send them an email with a link).

User accounts require both a display name ("Deniz Genc") and a user account name (`denizgenc` or
`dg@example.com`). This and other information (job title, picture...) and settings (admin
privileges, etc) are stored in the profile.

Bulk creation of user accounts is also supported, by uploading a CSV with various fields (Azure
portal -> Azure Active Directory -> Users -> All users -> Bulk operations). There are templates for
these CSVs, available from Azure.
- Doing bulk operations generates a results file which lists any errors encountered during the
  process, along with a resason for the error (e.g. user account name collision).

# Group accounts
There are two different types of group accounts:
- Security groups help define permission boundaries for resources, etc.
- Microsoft 365 groups are more for collaboration, since it gives members access to a mailgroup,
  shared drive, etc.

When a group is created, you can configure different membership regimes:
- Assigned: Users must specifically be made a member of a group.
- Dynamic user: Define rules for membership of the group, based on the attributes of user account
  profiles.  For example, an admin group will automatically add all admins in the directory. When a
  user account's attributes change, they may be added to or removed from a group, depending on the
  rules.
  - Requires AD Premium P1 or P2 licence
  - An example of a rule: Property = `jobTitle`; Operator =  Equals; Value = "Cloud Administrator"
- Dynamic device (security groups only): Same as the above, but for device attributes - I assume
  this is to ensure that devices are enrolled in an MDM programme, or are accessing resources from a
  specified network (e.g. a work VPN).
  - Requires AD Premium P1 or P2 licence

## Security groups
Are these supposed to be like IAM roles?

Only Entra administrators can create security groups. (Does that mean "global administrators" and
"user administrators", or only the global ones?)

## Microsoft 365 groups
Normal users might be able to create these? ("Normal users and Microsoft Entra admins can both use
Microsoft 365 groups." -> what does "use" mean in this case?)

# Administrative units
Given a large enough organisation, you might want to delegate administrative tasks for certain
sections of the organisation to "admins" without giving them free reign across the whole thing. You
can enforce these boundaries with administrative units.

For example, an organisation might have several business units who have their own internal network
with their own resources (servers, printers), who need to manage access. Let's say such a unit is
called "the science team". You can do the following:
- Create an administrative unit for the science team.
- Create a role that has admin permissions for only Entra users in the science team administrative
  unit
- Add all the members of the science team, and the resources that are part of the team, to the
  administrative unit
- Whoever is responsible for the IT team can have the administrator role we created earlier assigned
  to them.
