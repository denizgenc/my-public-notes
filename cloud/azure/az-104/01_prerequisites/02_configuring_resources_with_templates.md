Link: [Configure resources with Azure Resource Manager templates
](https://learn.microsoft.com/en-us/training/modules/configure-resources-arm-templates/)

Benefits of templates according to MS:
- Improve consistency
- Help express complex deployments
- Reduce manual tasks
- Templates are code, so can be tracked in version control
- Promote reuse
- Are linkable ("You can link Resource Manager templates together to make the templates themselves
  modular. You can write small templates that each define a piece of a solution, and then combine
  them to create a complete system.")
- Simplify orchestration (deploy multiple resources with just one command)

# The ARM template schema
It's a JSON object with the three following required keys:
- `"$schema"` - a link to a JSON schema file that describes the version of the language. Currently
  this is set to the value
  `"http://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#"`.
- `"contentVersion"` - required but can take any value (including an empty string). This is used to
  make sure the right version of the template is being applied - to prevent downgrades (presumably)
- `"resources"` - a list of values that represent resources to deploy

There are additional optional keys:
- `"parameters"` - like variables in Terraform. A JSON object which represents values that can be
  passed into other parts of the template. You are limited to 256 parameters in any one template.
- `"variables"` - more like constants, or alternatively locals in Terraform. A JSON object which
  holds values that "are used as JSON fragments", presumably to make the template less verbose if
  certain fragments are repeated.
- `"functions"` - a list. "User-defined functions that are available within the template". How does
  that work?
- `"outputs"` - a JSON object, like outputs in Terraform.

# Template parameters
Here are the available properties for a parameter:
```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
        "description": "<description-of-the parameter>"
        }
    }
}
```

# Bicep templates
Azure specific Terraform, from Microsoft. Advantages over templates:
- Simpler syntax
- Modules
- Automatic dependency management (apparently you have to mark resources as dependent on each other
  in templates?)
  - you may still have to manually manage dependencies in some situations, like in Terraform.
- Type validation and IntelliSense

# QuickStart templates
Online catalogue of community-provided templates for certain solutions (e.g. deploying a django app)
