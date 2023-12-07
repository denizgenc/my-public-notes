# Exam AZ-104: Microsoft Azure Administrator
Link to the exam page, with further links to various learning paths that can be used to prepare:
https://learn.microsoft.com/en-gb/credentials/certifications/exams/az-104/

This path itself has 3 other learning paths as prerequisites:
1. [Microsoft Azure Fundamentals: Describe cloud
   concepts](https://learn.microsoft.com/en-us/training/paths/microsoft-azure-fundamentals-describe-cloud-concepts/)
2. [Azure Fundamentals: Describe Azure architecture and
   services](https://learn.microsoft.com/en-us/training/paths/azure-fundamentals-describe-azure-architecture-services/)
3. [Azure Fundamentals: Describe Azure management and
   governance](https://learn.microsoft.com/en-us/training/paths/describe-azure-management-governance/)

I would have created subdirectories for those paths but I'm on the third one right now and I only
just thought to take notes. To reflect these prequisites in notes I will compile them all in
[`00_initial_impressions.md`](00_initial_impressions.md).
- Oh there's a page for services comparison on Azure's website:
  https://learn.microsoft.com/en-us/azure/architecture/aws-professional/services

  It's basically what that initial impressions file is mostly about

# Questions I have
- Are there any resource providers that aren't from Microsoft?
- What defines a child resource (for resource locking)?
  - Found the answer
    [here](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/move-support-resources):

    In most cases, a child resource can't be moved independently from its parent resource. Child
    resources have a resource type in the format of
    `<resource-provider-namespace>/<parent-resource>/<child-resource>`. For example,
    `Microsoft.ServiceBus/namespaces/queues` is a child resource of
    `Microsoft.ServiceBus/namespaces`.  When you move the parent resource, the child resource is
    automatically moved with it. If you don't see a child resource in this article, you can assume
    it is moved with the parent resource.  If the parent resource doesn't support move, the child
    resource can't be moved.

- Identities and governance
  - From
    [02_identities_governance/01_configure_user_group_accounts.md](02_identities_governance/01_configure_user_group_accounts.md):
    - Are security groups supposed to be like IAM roles?
    - Only administrators can create security groups, but which kind of admin? (I've heard of
      "global" and "user" admins...)
