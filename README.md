# azautohelloworld

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbartduncan%2Fazautohelloworld%2Fmain%2FHelloWorldRunbook%2FHelloWorldRunbookArm.json)

[![Deploy to Azure 2](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbartduncan%2Fazautohelloworld%2Fmain%2FHelloWorldRunbook%2FHelloWorldRunbookArm2.json)

----

* GitHub project that makes use of the automationAccounts/modules section in an ARM template, avdaccelerator. 
* Modules resource within the resources section of ARM template: 

```
{
    "type": "Microsoft.Automation/automationAccounts/modules",
    "apiVersion": "2015-10-31",
    "name": "[concat(parameters('automationAccountName'), '/', parameters('moduleName'))]",
    "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts', parameters('automationAccountName'))]"
    ],
    "properties": {
        "contentLink": {
            "uri": "https://github.com/rchaganti/armseries/raw/master/MyModule.zip"
        }
    }
}
```

The type property is set to Microsoft.Automation/automationAccounts/modules, which specifies that the resource is a module for an Azure Automation account. The name property is set to the concatenation of the automationAccountName and moduleName parameters (slash delimited). The dependsOn property specifies that the module depends on the existence of the Automation account specified by the automationAccountName parameter. The contentLink property specifies the URI of the module content, which is provided by the moduleUri parameter.

Ref: 
* https://powershellmagazine.com/2015/11/26/deploy-custom-azure-automation-integration-modules-using-arm-templates/
* https://azure.microsoft.com/de-de/blog/authoring-integration-modules-for-azure-automation/
* https://learn.microsoft.com/en-us/azure/templates/microsoft.automation/automationaccounts/modules?pivots=deployment-language-bicep
* https://learn.microsoft.com/en-us/azure/templates/microsoft.automation/2022-08-08/automationaccounts/modules
* https://learn.microsoft.com/en-us/azure/automation/quickstart-create-automation-account-template
* https://github.com/hashicorp/terraform-provider-azurerm/issues/8286
* https://www.powershellgallery.com/api/v2/package

----

# Imperative Module Import

```
New-AzAutomationModule -AutomationAccountName "Contoso17" -Name "ContosoModule" -ContentLinkUri "http://contosostorage.blob.core.windows.net/modules/ContosoModule.zip" -ResourceGroupName "ResourceGroup01"
```

Or in portal: 
1. Navigate to Automation account, select Modules under Shared Resources.
2. Click on Add a module, upload the .zip file containing your module.

----

# PS module publishing

PowerShell Gallery: https://stackoverflow.com/questions/43745716/azure-arm-template-and-powershell-module

