# AzFW-IPGroup-Update

# Overview
This repository contains the ARM templates for deploying an Azure Logic App to automatically update an IP Group when a malicious flow is detected by NSG Flow Logs.
# Prerequisites 
* Existing Azure Firewall utlizing IP groups
* Existing Azure IP Group configured to be used by Azure Firewall
* Azure Network Security Groups (NSG) with Flow logs enabled

# Setup


### Deploy Logic App
&nbsp;
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FLogic_App%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true"/> 
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FLogic_App%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true"/>
</a>
&nbsp;

1. Start the deployment of the Azure Logic App to your Azure subscription by clicking the __Deploy to Azure__ button above. 
1. For the deployment you will need to provide the following parameters -

<center>

| Parameter  | Description |
| ------------- | ------------- |
| Resource Group  | Content Cell  |
| Region  | Content Cell  |
| Logic App Name  | Content Cell  |
| Azure Sub ID  | Content Cell  |
| IP Group RG Name  | Content Cell  |
| IP Group Name   | Content Cell  |

</center>

3. Verify that all provided settings are correct and then click __Create__ to initiate the deployment.
1. When the deploment completes you will have a Logic App created in the Resource Group specified 
1. Select the Logic App resource in that was deployed and navigate to the __Logic App Designer__ section
1. In the designer click on the fist step __Something__ to expand so that you can see the HTTP endpoint 
1. Copy and save the __HTTP Endpoint__ value, it will be used for the deployment configuration of the __Action Group__ so that the __Alert Rule__ can trigger the __Logic App__ and send information about the malicious flow 


### Create Alert Rule
&nbsp;
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FAlert_Rule%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true"/> 
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2FAlert_Rule%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/visualizebutton.svg?sanitize=true"/>
</a>
&nbsp;

1. Start the deployment of the Alert Rule and Action Group to your Azure subscription by clicking the __Deploy to Azure__ button above
1. For the deployment you will need to provide the following parameters -

<center>

| Parameter  | Description |
| ------------- | ------------- |
| Action Group Name  | Content Cell  |
| Logic App Endpoint  | Content Cell  |
| Log Analytics Workspace Name  | Content Cell  |
| Log Analytics Workspace Resource Group  | Content Cell  |

</center>

3. Verify that all provided settings are correct and then click Create to initiate the deployment.


# Refrence Links
* [Azure Monitor Alert Schema](https://docs.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-common-schema-definitions)
* [IP Groups API GET](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/get)
* [IP Groups API PUT - Create or Update](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/create-or-update)