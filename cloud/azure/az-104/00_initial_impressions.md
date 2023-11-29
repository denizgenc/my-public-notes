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
  - Azure Blobs == S3
  - Azure Files == EFS (+ support for SMB)
  - Azure Disks == EBS
  - Azure Queues == SQS
  - Azure Tables == Uhh, what's that nosql thing AWS offers?
