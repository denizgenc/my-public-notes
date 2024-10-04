Link to module: [Configure role-based access
control](https://learn.microsoft.com/en-us/training/modules/configure-role-based-access-control/)

There's a difference between Azure RBAC and Entra roles, apparently.

# Core concepts
- Security principal: an actor (e.g. a user, group, a VM) that need access
- Role definition: A set of permissions. Azure comes with some built-in ones (e.g. Reader, Owner),
  but custom ones can be defined
- Scope: The boundary that defines what the permissions are allowed to act on. Basically the
  `Resource` field in AWS IAM policies. This can be anywhere from a specific resource to a whole
  management group.
  - Unlike IAM policies, though, this is _separate_ from a role definition, and is set when a role
    is assigned. In other words, allowed actions have been decoupled from the objects they act on.
- Assignment: When a role definition is attached to a security principal at a particular scope

# Role definitions
Role definitions ("sometimes just called a _role_") have several different properties that define
and/or narrow what the role permits the assignee to do and what it can act on. The important
properties are (see [this documentation
page](https://learn.microsoft.com/en-us/azure/role-based-access-control/role-definitions#role-definition):
- `Description`: a description of the role
- `Actions`: An array of strings that specifies the "control plane" actions permitted by the role
- `NotActions`: An array of strings that excludes things defined in `Actions`
  - Note if an identity is assigned a role that provides has an action defined in `Actions` despite
    also having one in `NotActions`, they are allowed to perform that action. `NotActions` isn't for
    denying, it's simply subtracting from the set of actions in `Actions`
- `DataActions`: An array of strings that specifies the "data plane" actions permitted by the role
- `NotDataActions`: An array of strings that excludes things defined in `DataActions`
  - See note above.
- `AssignableScopes`: An array of strings that defines which identities the role may be assigned to.
  You can scope it to anyone, a specific subscription, or even identities within a specific resource
  group.

As an example, the built in "Contributor" role in Azure RBAC is defined as:
```json
{
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Authorization/*/Delete",
    "Authorization/*/Write",
    "Authorization/elevateAccess/Action"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

From this definition, we can tell that:
- Given the `Action` and `NotAction` fields, the Contributor role can do everything except for
  Deleting, Writing, or elevating access.
- Given the `DataActions` and `NotDataActions` fields, any actions allowed by the earlier fields can
  affect the data.
- The role can be assigned for all scopes.

In general, the "Owner" role has the highest amount of access in Azure.

# Role assignments
