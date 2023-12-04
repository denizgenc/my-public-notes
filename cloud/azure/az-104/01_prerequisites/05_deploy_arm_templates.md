Link: [Deploy Azure infrastructure by using JSON ARM
templates](https://learn.microsoft.com/en-us/training/modules/create-azure-resource-manager-template-vs-code/)

Note that a lot of this module seems to be similar to the module ["Configure resources with Azure
Resource Manager templates"](02_configuring_resources_with_templates.md), so this will only have
stuff that is/seems new to me.

ARM templates are idempotent like Terraform. The Resource Manager tries to create resources in
parallel whenever possible; and it also validates the template before deploying.

Note that ARM templates aren't pure JSON because they support `//` and `/* */` comments.

# Deploying templates to Azure
Three ways:
- Deploying a local template (i.e. one saved as a JSON object on your computer)
- Deploy a linked template (a template that references others, like Terraform modules)
- Deploy in a continuous deployment pipeline

Once you have a resource group in a region you want to deploy to, you can deploy a local template
with `az deployment group create --name DEPLOYMENT_NAME --resource-group RESOURCE_GROUP_NAME
--template-file PATH_TO_TEMPLATE`

# Adding resources to the template
To find out the names and required parameters for resources in ARM templates, go to the page on
Microsoft's site for the [Azure resource
reference](https://learn.microsoft.com/en-us/azure/templates/). For example, here's the page for
[`Microsoft.Storage/storageAccounts`](https://learn.microsoft.com/en-us/azure/templates/microsoft.storage/storageaccounts?pivots=deployment-language-bicep)
resources.

Based on the above, we can construct the following schema:
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.1",
  "apiProfile": "",
  "parameters": {},
  "variables": {},
  "functions": [],
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-06-01",
      "name": "learntemplatestorage123",
      "location": "westus",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      }
    }
  ],
  "outputs": {}
}
```

There's a VS Code extension called
[Azure Resource Manager (ARM)
Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) that
is recommended for writing templates. Utilises IntelliSense and macros to output a bunch of stuff
for you automatically, and fills in various things for you.

# Notes for using template parameters
Reminder that parameters in ARM templates are like Terraform variables. Again, look at
[`02_configuring_resources_with_templates.md`](02_configuring_resources_with_templates.md) for an
introduction to them.

The parameter types you can use in templates are:
- string
- secureString
- integers
- boolean
- object
- secureObject
- array

Note that secureString is for parameters that are for sensitive strings (usernames, passwords, API
tokens, etc); secureObject is for when there's sensitive data within a JSON object that needs to be
passed in. Just like the `sensitive` flag for Terraform variables, this will suppress the output of
the parameter in CLI output.

## Example use of parameters
We can define a `storageAccountType` parameter:
```json
"parameters": {
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
      "Standard_LRS",
      "Standard_GRS",
      "Standard_ZRS",
      "Premium_LRS"
    ],
    "metadata": {
      "description": "Storage Account type"
    }
  }
}
```

To use a parameter, we use the `parameters()` function (within square brackets), passing in the
parameter name as an argument, within single quotes. As an example for the above, we would reference
the parameter with `"[parameters('storageAccountType')]"`:
```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-04-01",
    "name": "learntemplatestorage123",
    "location": "[resourceGroup().location]",
    "sku": {
      "name": "[parameters('storageAccountType')]"
    },
    "kind": "StorageV2",
    "properties": {
      "supportsHttpsTrafficOnly": true
    }
  }
]
```

# Template outputs
Again, like Terraform outputs, but more powerful in that you can specify a condition on when the
output is returned (using a Boolean expression), and also use the `"copy"` field to return more than
one value.

example
```json
"outputs": {
  "<output-name>": {
    "condition": "<boolean-value-whether-to-output-value>",
    "type": "<type-of-output-value>",
    "value": "<output-value-expression>",
    "copy": {
      "count": <number-of-iterations>,
      "input": <values-for-the-variable>
    }
  }
}
```
