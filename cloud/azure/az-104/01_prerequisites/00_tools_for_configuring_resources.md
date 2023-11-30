Link: https://learn.microsoft.com/en-us/training/modules/configure-azure-resources-tools/

There are four main tools for interfacing with Azure:
- The Azure portal
- Azure Cloud Shell
- Azure PowerShell
- Azure CLI

The portal is click-ops, which as we all know is cringe and should be avoided at all cost

Cloud Shell is a web-based terminal that you can access via the portal. It allows you to choose
between a Bash or PowerShell session. Note: it requires an Azure Files share to be mounted (since it
needs a filesystem to run from), and will store a 5 GB image in that share to persist the home
directory.

Azure PowerShell is a module that can be installed to PowerShell which provides additional cmdlets
(e.g. `New-AzVm`) to PowerShell, and "provides services such as the shell window and command
parsing". I guess the point is that it integrates with PowerShell's object-based paradigm (instead
of a CLI's text based one).

Azure CLI is basically the `az` command.
