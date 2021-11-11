# AzFW-IPGroup-Update

### Logic App
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FLogic_App%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true"/> 
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FLogic_App%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true"/>
</a>

### Alert Rule & Action Group
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FAlert_Rule%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true"/> 
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FAlert_Rule%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true"/>
</a>

# Overview
This repository contains the ARM templates for deploying an Azure Logic App to automatically update an IP Group when a malicious flow is detected by NSG Flow Logs.
# Prerequisites 
* Existing Azure Firewall utlizing IP groups
* Existing Azure IP Group configured to be used by Azure Firewall
* Azure Network Security Groups (NSG) with Flow logs enabled

# Setup
### Deploy Logic App

1. Start the deployment of the Azure Logic App to your Azure subscription by clicking the button above. 
1. For the deployment you will need to provide the following parameters -
 * Resource Group
 * Region
 * Logic App Name
 * Azure Sub ID
 * IP Group RG Name 
 * IP Group Name 
3. Verify that all provided settings are correct and then click Create to begin the deployment.
1. When the deploment completes .....
1.  
### Create Alert Rule
1. 

# Refrence Links
* [Azure Monitor Alert Schema](https://docs.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-common-schema-definitions)
* [IP Groups API GET](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/get)
* [IP Groups API PUT - Create or Update](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/create-or-update)