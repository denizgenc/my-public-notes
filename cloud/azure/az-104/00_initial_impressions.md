From [Describe the core architectural components of
Azure](https://learn.microsoft.com/en-us/training/modules/describe-core-architectural-components-of-azure/):
- An Azure "account" is more similar to an AWS Organization.

  **A subscription in Azure is the analogue to an account in AWS**.
- Every resource *must* be placed in a resource group. A resource may only be in one group at any
  time. Resource groups cannot be nested.

  Resource groups are helpful in identifying what resources are related (something that was rather
  ad-hoc in AWS, e.g. by using a `Project` tag). You can limit users to access only specific
  resource groups.
- You can assign subscriptions to Azure management groups, which are located within an account;
  management groups can also be nested, forming a hierachy. This makes them the analogue to AWS
  organizational units (OUs).

From [Describe Azure compute and networking
services](https://learn.microsoft.com/en-gb/training/modules/describe-azure-compute-networking-services/):
- VM scale sets == AWS EC2 autoscaling group (?).
  - There are also VM "availability sets", for reliability instead of performance/efficiency. You
    can group an availability set using update domains, which allows you to stagger updates across
    the set; or fault domains, which ensures that VMs are located in up to 3 different locations in
    a datacenter (protecting against power or networking failure).
- Azure Container Apps == AWS ECS (running containers with load balancing and autoscaling). Azure
  Container Instances are a simpler version - just run a container without load balancing etc.
- Azure Functions == AWS Lambda, but there are stateful versions of them (called Durable Functions)
  that can maintain state across invocations.
- Azure Virtual Networks == AWS VPCs
  - There's a name resolution service built into Azure
  - To make a resource in a virtual network publically accessible, either assign a public IP to it
    or put it behind a public load balancer.
  - Just like VPCs, some resources reside within virtual networks (VMs, Kubernetes Service), while
    others can be connected via service endpoints
- Network Security Groups == Security Groups (with rule priority as a "bonus"); there are also
  "network virtual appliances" which seem to be for stuff like intrustion detection/prevention
- Azure DNS == Route53 (but you can't register domains with it)

From [Describe Azure storage
services](https://learn.microsoft.com/en-gb/training/modules/describe-azure-storage-services/):
- You need to create a "storage account" which gives you a namespace for the storage; the type of
  storage account determines what sort of redundancy options will be available to you.

  Storage accounts require globally unique names, so in this sense they're like AWS S3, but storage
  accounts offer multiple storage types simultaneously:
  - Azure Blobs == S3 ; has hot, cool, cold and archive access tiers
  - Azure Files == EFS ; a system for hosting an NFS or SMB share (EFS doesn't support SMB!)
  - Azure Disks == EBS
  - Azure Queues == SQS
  - Azure Tables == DynamoDB

From [Describe Azure identity, access, and
security](https://learn.microsoft.com/en-gb/training/modules/describe-azure-identity-access-security/):
- Entra ID is Microsoft's system for managing sign-ins to their cloud apps (e.g. Office 365). It's
  also offered to Azure users as a mechanism for managing identity and authentication in their cloud
  applications.
  - Entra ID is not a replacement for on-prem AD, since it doesn't do the same things (I think?);
    however you can connect it with your AD to synchronise identities on your local domain with the
    ones used by your cloud apps, and allows you to use features that Entra ID provides (e.g. SSO
    and MFA).
- Entra Domain Services is a managed Domain Controller for Active Directory. Basically instead of
  maintaining your own DC inside an Azure VM, you can use Entra Domain Services instead, and have it
  be automatically patched etc.
- Azure RBAC (?) == AWS IAM
  - RBAC works on scopes, and they're hierarchical: Management groups > subscriptions > resource
    groups > resources. In other words, if someone is given a role over a subscription, they have
    the same permissions on all the resource groups inside that subscription.
  - RBAC has a bunch of different roles. In terms of permissions granted, they are: Owner >
    Contributor > Custom roles > Resource-specific > Reader

Assorted other notes from various modules in the [Microsoft Azure Fundamentals: Describe Azure
management and governance
](https://learn.microsoft.com/en-us/training/paths/describe-azure-management-governance/) learning
path:
- You can tag stuff in Azure too
- The way things get made is through "Azure Resource Manager", which is the system/API that you talk
  to when you use the Azure Portal or the CLI.
- ARM Templates are a JSON format for describing infrastructure that can then be deployed to Azure
  through `az deployment group create` (given the resource group you will be deploying to already
  exists). The syntax for ARM templates is a little obtuse though.
- Bicep is basically a custom language that compiles to ARM Templates (though you can deploy with
  them directly, again with `az deployment group create`). Think of it as Azure specific Terraform.
- Azure Policy is like AWS Config - it enforces certain conditions on resources (e.g. all resources
  must be tagged). Policies can even actively remediate violations by making changes to resources
  that do not comply; however users can flag the resource as an "exception".
  - Multiple policies can be grouped together into an initiative
- Resource locks prevent resources being modified/deleted. First the lock must be removed, and you
  can assign permissions to locks and resources separately.
- Azure Advisor == AWS Trusted Advisor. Does reliability, security, performance, "operational
  excellence" and costs checks against your resources.
- Azure Monitory == AWS CloudWatch. Provides visibility on metrics and logs coming from your various
  resources. You can set alerts with it.
  - Azure Log Analytics *seems* to be like CloudWatch logs? I'm not too sure though.
