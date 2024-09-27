# 1 - Understand Infrastructure as Code (IaC) concepts
(Documentation and tutorial links from
https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-study-003)

## 1a - Explain what IaC is
### Links
- Documentation:
  - https://developer.hashicorp.com/terraform/intro
  - https://www.hashicorp.com/blog/infrastructure-as-code-in-a-private-or-public-cloud
- Tutorial:
  - https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code

### My notes
They define the **core workflow** as:
- **Write:** writing the .tf files
- **Plan:** Creating an execution plan for either applying/modifying or destroying
- **Apply:** Execute the plan

Headings from [blog
post](https://www.hashicorp.com/blog/infrastructure-as-code-in-a-private-or-public-cloud) are
important:
- **IaC makes infra more reliable**
  - Increased replicability and reliability when making changes. No futzing around web consoles or
    SSHing into servers and following a playbook; no room for error.
  - Allows for testing of infra definitions like other code (?)
  - Check in IaC to a VCS -> see how infra evolves over time; potentially roll back.
- **IaC makes infra more manageable**
  - Only makes the changes you need, doesn't touch resources whose configurations are the same.
- **IaC makes sense**
  - Managing infra is hard, IaC reduces toil and mitigates risk.

The blog post makes the point that even though cloud providers ("virtual compute") exposed APIs for
creating and configuring resources, **you'd still have to script them**. Imagine an AWFUL bunch of
Bash scripts with `curl` in them to create your infra, lol.
- So the point of IaC is not just using code to provision infra, but a **readable and maintainable
  language to do so**.
- Plus the whole point of Terraform is that it's declarative - you don't care about the API requests
  you need to make (and in which order they need to be made) to deploy, you just write down what
  your desired state is.

Common metaphor in both blog post and video is "Day 0" -> no infra provisioned; "Day 1" -> some
infra provisioned, more needs to be added and changes needed for existing.

## 1b - Describe advantages of IaC patterns
### Links
- Documentation:
  - https://www.hashicorp.com/resources/what-is-infrastructure-as-code
  - https://www.hashicorp.com/blog/infrastructure-as-code-in-a-private-or-public-cloud
- Tutorial:
  - https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code

### My notes
Nothing that isn't covered in 1a


## Curiosities
Here I put stuff that I found interesting, but might not be something I actually need to know for
the test.

Video: https://youtu.be/h970ZBgKINg - Intro to Terraform with CTO Armon Dadgar
- mentions the first core command is `terraform refresh` which basically checks the state with
  what's deployed, which is interesting.
  - ... I just realised that `refresh` is probably a plumbing concept, i.e. it automatically runs
    when `terraform plan` is run.
- He talks about Terraform HCP, which is a cloud service that keeps your state, but also makes sure
  that `terraform apply`s only get run sequentially, so teams don't have to run into state lock
  issues. Also maintains secrets, and allows for private module registries to be set up.

Terraform works using graphs :)
