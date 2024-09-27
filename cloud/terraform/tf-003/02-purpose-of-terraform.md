# 2 - Understand the purpose of Terraform (vs other IaC)
(Documentation and tutorial links from
https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-study-003)

## 2a - Explain multi-cloud and provider-agnostic benefits
### Links
- Documentation:
  - https://developer.hashicorp.com/terraform/intro/use-cases#multi-cloud-deployment

### My notes
They say multi-cloud enables fault-tolerance; but it adds complexity.
- "Terraform lets you use the same workflow to manage multiple providers" - ok?
- "... and handle cross-cloud dependencies". Interesting. How?

There's a tutorial they link to called [Deploy Federated Multi-Cloud Kubernetes
Clusters](https://developer.hashicorp.com/terraform/tutorials/networking/multicloud-kubernetes).
It's basically about deploying a cluster to AWS EKS and to Azure AKS. Things to note:
- The [example repo](https://github.com/hashicorp/learn-terraform-multicloud-kubernetes) has
  separate `eks` and `aks` folders. They mention that they could all be in the same folder, but it's
  better to keep logically related components together.
- There's a thing called Consul that allows two Kubernetes deployments to be tied together. What's
  important is that **they're using the `terraform_remote_state` data source** to get outputs from
  the EKS and AKS deployments and use them in the Consul Terraform.
- Also, in the Consul config, the `kubernetes` provider is used twice - one for the EKS deployment
  and another for the AKS. **Provider aliasing** is used to distinguish between them.
- Terraform's **dependency graph** figures out the order to deploy everything, even when there are
  cross-cloud dependencies (in the case of Consul)

## 2b - Explain the benefits of state
### Links
- Documentation:
  - https://developer.hashicorp.com/terraform/language/v1.1.x/state/purpose
- Tutorial:
  - https://developer.hashicorp.com/terraform/tutorials/state/state-cli

### My notes
Why state?
- Mapping to the real world
- Metadata
  - If you delete a resource from your config, without state Terraform wouldn't know to delete it
    next time you `plan` or `apply`.
  - Also, it wouldn't know _how_ to delete it, given complex dependencies between resources. So it
    also stores the dependencies between resources as well as metadata about resources themselves.
- Performance
  - The resource attributes (e.g. name) are usually pulled from the provider - "sync"
  - However this can be slow, especially with large architectures and API rate limiting. Running
    with `-refresh=false` prevents syncing, and state metadata is treated as a record of truth
- Syncing/working in teams
  - You need to store state remotely for this

**Don't download the state file to inspect it.** Instead:
- `terraform show` outputs all the data in the state in a more readable format (i.e. not JSON)
  - it just pops it into stdout tho, watch out
- `terraform state list` gets a list of resource names and local identifiers from the state file
  (discarding all the additional info like attributes, etc)

**`-replace="resource_type.name"`** is the canonical (post v0.15.2) way to reprovision resources
when their configuration is the same. Just pass the flag when running `plan` or `apply`.
- This replaces `terraform taint`, and is apparently less error-prone

**`terraform refresh`** syncs your state with the current real world configuration

#### Advanced state management
`terraform state mv` and the [`moved` block
(v1.1+)](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring)
- Can be used to rename resources, e.g. if you change the name of a resource in the configuration
  and want to avoid destroying and recreating.
- (`state mv` can also move resources from the current state to a local state file, but don't do
  that)

`terraform state rm` and the [`removed` block
(v1.7+)](https://developer.hashicorp.com/terraform/language/resources/syntax#removing-resources)
- Tells Terraform to stop tracking a resource without destroying it
- `removed` block can remove multiple resources (and even modules) at the same time

`terraform import` and the [`import` block
(v1.5+)](https://developer.hashicorp.com/terraform/language/import)
- Brings a resource that Terraform did not provision into its state file
- `import` block can import multiple things at the same time, and review the import during
  plan/apply.

## Curiosities
When importing existing resources into Terraform, you need to make sure that **each distinct
object is imported to only one resource instance**.
- I didn't even think of trying to import the same resource twice. Apparently you can do it.

Terraform state schema notes:
- `resources`: array of objects that represents every named resource or data source you've deployed.
  For each object:
  - `mode`: Whether the object represents a resource type (`managed`), or a data source (`data`).
  - `type`: the resource type, e.g. `aws_s3_bucket`
  - `name`: the name of the resource, e.g. for `data.aws_iam_role.foobar` this would be `foobar`
  - `instances`: array of objects that relate to tracked real-world things. I assume this is an
    array because you can deploy multiple resources/data sources with the same name, using the
    `count` or `for_each` meta-arguments. Each of these will have:
    - `attributes`: another object that relates to the attributes defined through the Terraform
      config, except they will have their values evaluated (i.e. if `name = "hello-${var.your_var}"`
      in the config, here it will be `"name": "hello-world"` or whatever)
    - `dependencies`: an array of strings of other resources/data sources this specific instance
      depends on, in the regular Terraform syntax: `data.aws_ami.ubuntu`, `aws_iam_role.foobar` etc
