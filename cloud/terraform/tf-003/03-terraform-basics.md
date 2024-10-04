# 3 - Understand Terraform basics
(Documentation and tutorial links from
https://developer.hashicorp.com/terraform/tutorials/certification-003/associate-study-003)

## 3a - Install and version Terraform providers & 3c - Write Terraform configuration using multiple providers
### Links
- Documentation:
  - https://developer.hashicorp.com/terraform/language/providers/configuration
  - https://developer.hashicorp.com/terraform/language/settings#specifying-provider-requirements
  - https://developer.hashicorp.com/terraform/language/files/dependency-lock
- Tutorial:
  - https://developer.hashicorp.com/terraform/tutorials/configuration-language/versions
  - https://developer.hashicorp.com/terraform/tutorials/configuration-language/provider-versioning
  - https://developer.hashicorp.com/terraform/tutorials/configuration-language/resource

### My notes
When setting up a provider you need to do 2 things:
- Require a provider (in the root module's `terraform` block, under `required_providers`)
- Configure the provider (a `provider "aws" {}` block)

**Imported modules get their provider config from the root module** (so you don't have to define an
`aws` provider if you're making a module that's designed to be imported)

You can lock the version of the providers through a dependency lock file, which records the version
constraint you define in the `required_providers`, the actual installed version, and then hashes of
that installed version to verify against when downloading.
- **Always named** `.terraform.lock.hcl`
- The lock file should be commited to version control
- If you do a `terraform init` with a new provider, the lock file is updated; otherwise if all the
  providers in the `.terraform` directory match the versions and hashes in the lock file, `terraform
  init` does nothing.
  - You can force an update of the installed provider versions (and the lock file) via `terraform
    init -upgrade`. (This will only upgrade within the specified version constraints, of course)

#### Provider requirements
Each provider in the `required_providers` block has two keys: `source` (where you're getting the
provider from; e.g. `hashicorp/aws` unless you're really fancy and have your own source for
providers); and a [version
constraint](https://developer.hashicorp.com/terraform/language/expressions/version-constraints#version-constraint-syntax).

Note that the "local name" for the provider doesn't have to match with the source. An example from
the Terraform docs is:
```hcl
terraform {
  required_providers {
    mycloud = {
      source  = "hashicorp/aws"
      version = "5.70.0"
    }
  }
}
```

This makes it possible to use the same source provider with different version constraints, if that's
required (e.g. by a child module):

```hcl
terraform {
  required_providers {
    mycloud = {
      source  = "hashicorp/aws"
      version = "5.70.0"
    }
    mycloud_old = {
      source  = "hashicorp/aws"
      version = "4.67.0"
    }
  }
}
```

However, it's recommended to use the same local name as the "preferred local name" of the provider
(i.e. `aws` for `hashicorp/aws`); because otherwise you have to define the local name in a
`provider` meta-argument for every resource/data source/module:

```hcl
resource "aws_s3_bucket" "sample" {
  provider = mycloud

  bucket = "sample"
}
```

So basically, **only re-define the local name for lesser-used resources/modules** instead of the main
provider you want to use.

#### Provider configuration
This is where you can set provider-specific configuration (e.g. for AWS: `region`, `access_key`,
`profile`...); as well as an alias.

An alias is useful because you can define the **same** provider (with the same version) multiple
times with a **different** configuration:

```hcl
provider "aws" {
  region = "eu-west-1"
}

provider "aws" {
  alias = "aws-london"
  region = "eu-west-2"
}
```

To use an alias for a data source, a resource or module, you have to reference it in the `provider`
meta-argument (for the above example, `provider = aws.aws-london` - notice it's not a string!).

## 3b - Describe plugin-based architecture and 3d - Describe how Terraform finds and fetches providers
### Links
- Documentation:
  - https://developer.hashicorp.com/terraform/language/providers
  - https://developer.hashicorp.com/terraform/plugin/how-terraform-works
- Tutorial:
  - https://developer.hashicorp.com/terraform/tutorials/community-providers
  - https://developer.hashicorp.com/terraform/tutorials/cli/init

### My notes
Terraform is a "plugin-based architecture" because providers are plugins, and they're the only thing
that do anything remotely tangible; remember that it's not just cloud or on-prem things that are
providers, but also stuff like `random` and `archive`.

Resources and data sources come from providers.

When installing providers, you can create a plugin cache on your local machine.

You can find providers on the [Terraform Registry](https://registry.terraform.io/browse/providers).

## Curiosities
If you set an alias for **every** configuration of a provider, then the **default configuration for
that provider will be an implied empty config** (which will usually error out). So if you use an
alias on every configuration of `aws`, you'll have to set a `provider` for every `aws_` resource or
data source - this is likely not what you want (unless you want to make sure people updating/adding
new resources are aware of which provider configuration they're going to be using for the resource).

Two parts of Terraform: Core and Plugins
- Core is the `terraform` binary. Parses the `.tf` files, builds the resource graph, manages the
  state, plans execution, and talks to plugins over RPC.
- Plugins are binaries. There are built in ones ("Provisioners" - bash) and those discovered
  dynamically ("Providers").
  - Providers initialise API libraries, authenticate with the service, define resources and data
    sources, etc.
  - Provisioners take actions on resources after they're created or destroyed. They're apparently a
    ["last
    resort"](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax#provisioners-are-a-last-resort)
