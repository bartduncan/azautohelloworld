# azautohelloworld

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbartduncan%2Fazautohelloworld%2Fmain%2FHelloWorldRunbook%2FHelloWorldRunbookArm.json)


----

* GitHub project that makes use of the automationAccounts/modules section in an ARM template, avdaccelerator. 
* [deploy.bicep](https://github.com/Azure/avdaccelerator/blob/main/carml/1.3.0/Microsoft.Automation/automationAccounts/modules/deploy.bicep) file that defines a modules resource within the resources section of the ARM template.
* This resource is used to import a module into an Azure Automation account.

Eexcerpt from deploy.bicep file:

```
resource module 'Microsoft.Automation/automationAccounts/modules@2022-08-08' = {
    name: name
    parent: automationAccount
    location: location
    tags: tags
    properties: {
        contentLink: {
            uri: version != 'latest' ? '${uri}/${name}/${version}' : '${uri}/${name}'
            version: version != 'latest' ? version : null
        }
    }
}
```

Alt: modules resource within the resources section of ARM template. Example:

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
            "uri": "[parameters('moduleUri')]"
        }
    }
}
```

The type property is set to Microsoft.Automation/automationAccounts/modules, which specifies that the resource is a module for an Azure Automation account. The name property is set to the concatenation of the automationAccountName and moduleName parameters (slash delimited). The dependsOn property specifies that the module depends on the existence of the Automation account specified by the automationAccountName parameter. The contentLink property specifies the URI of the module content, which is provided by the moduleUri parameter.

You can then specify values for the automationAccountName, moduleName, and moduleUri parameters when deploying the ARM template. The moduleUri parameter should be set to the URI of a .zip file containing your module.

The type property is set to Microsoft.Automation/automationAccounts/modules, which specifies that the resource is a module for an Azure Automation account. The name property is set to the value of the name parameter, which specifies the name of the module. The parent property specifies that the module is a child resource of the Automation account specified by the automationAccountName parameter. The contentLink property specifies the URI of the module content, which is provided by the uri parameter.

Ref: 
* https://powershellmagazine.com/2015/11/26/deploy-custom-azure-automation-integration-modules-using-arm-templates/
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

