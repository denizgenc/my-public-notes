Link: https://learn.microsoft.com/en-us/training/modules/use-azure-resource-manager/

Azure resource terminology:
- Resource provider - basically a generic term for the various services you can use for deploying
  resources. Examples are `Microsoft.Compute`, which provides VMs, `Microsoft.Storage`,
  `Microsoft.KeyVault`, etc
  - Resource types build upon these resource provider names - `{resource-provider}/{resource-type}`.
    So a VM is `Microsoft.Compute/virtualMachines`.
  - I wonder if there are non-Microsoft providers.

Resource group rules:
- A resource can only be in one resource group at any time
  - There are limitations on [moving
    resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources)
    from group to group. Note that you can also move a resource to a different subscription (!)
- Resource groups can't be renamed
- Resource groups can have resources of different types and from different regions
  - The resource group itself must be placed within a specific region. This basically determines
    where the metadata about the resources within the group is stored (which can be relevant for
    regulatory/compliance reasons)

Azure Resource Manager locks can be either read-only (no modifications allowed whatsoever), or
simply deletion locks; and they can be applied to a subscription, resource group, or a specific
resource. Note that locks are inherited by child resources.
