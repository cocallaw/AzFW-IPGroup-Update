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


* __Resource Group__ - Name of new or existing Resource Group that the logic app will be deployed into
* __Region__ - If deploying into an existing Resource Group the region will autopopulate, if deploying to a new Resource Group you will need to manually specify an Azure region
* __Logic App Name__ - Name of the Azure Logic App Resource being deployed
* __Azure Sub ID__ - Azure Subscription GUID that will contain the Logic App and IP Group resources, Parameter will automatically pupulate with `[subscription().subscriptionId]` which will pull the Subscription GUILD that you are deploying into 
* __IP Group Resource Group Name__ - Name of Resource Group that contains the IP Group resource that will be updated
* __IP Group Name__ - Name of the IP Group resource that will be updated by the Logic App 


3. Verify that all provided settings are correct and then click __Create__ to initiate the deployment.
1. When the deploment completes you will have a Logic App created in the Resource Group specified 
1. Select the Logic App resource that was deployed and navigate to the __Logic App Designer__ section under __Development Tools__
1. In the designer click on the fist step __When a HTTP request is recieved__ to expand it 
<p align="center">
  <img width="750" src="https://github.com/cocallaw/AzFW-IPGroup-Update/raw/main/Images/LAHTTPPostURL.png" alt="Logic App HTTP Post URL">
</p>

7. Copy and save the __HTTP POST UTL__ value, it will be used for the deployment configuration of the __Action Group__ so that the __Alert Rule__ can trigger the __Logic App__ and send information about the malicious flow 

&nbsp;
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


* __Action Group Name__ - Name of the Action Group resource that will be created 
* __Logic App Endpoint__ - The value of the __HTTP POST URL__ identified during the seployment of the Logic App
* __Log Analytics Workspace Name__ - Name of the Log Analytics Workspace that the Kusto queury will run against to identify malicious flows in NSG Flow Logs
* __Log Analytics Workspace Resource Group__ - Name of the Resource Group containing the Log Analytics Workspace being used to perform Kusto queries


3. Verify that all provided settings are correct and then click __Create__ to initiate the deployment.
1. When the deployment completes both the __Alert Rule__ and __Action Group__ will be deployed

The __Alert Rule__ is now actively monitoring the NSG Flow logs and will trigger the Logic App with the information about Malicious Flows to update the IP Group. 

To view the Alert Rule and Action Group and adjust any settings you can navigate to the [Alerts Management Summary Blade](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AlertsManagementSummaryBlade) and selecting __Alert Rules__

<p align="center">
  <img width="500" src="https://github.com/cocallaw/AzFW-IPGroup-Update/raw/main/Images/AlertRules.png" alt="Alert Rule Button in the Azure Portal">
</p>

# Refrence Links
* [Azure Monitor Alert Schema](https://docs.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-common-schema-definitions){:target="_blank" rel="noopener"}
* [IP Groups API GET](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/get){:target="_blank" rel="noopener"}
* [IP Groups API PUT - Create or Update](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/create-or-update){:target="_blank" rel="noopener"}