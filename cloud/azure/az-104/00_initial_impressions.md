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
- VM scale sets = AWS EC2 autoscaling group (?).
  - There are also VM "availability sets", for reliability instead of performance/efficiency. You
    can group an availability set using update domains, which allows you to stagger updates across
    the set; or fault domains, which ensures that VMs are located in up to 3 different locations in
    a datacenter (protecting against power or networking failure).
- Azure Container Apps == AWS ECS (running containers with load balancing and autoscaling). Azure
  Container Instances are a simpler version - just run a container without load balancing etc.
- Azure Functions == AWS Lambda, but there are stateful versions of them (called Durable Functions)
  that can maintain state across invocations.
