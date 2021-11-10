# AzFW-IPGroup-Update

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true"/> 
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fcocallaw%2FAzFW-IPGroup-Update%2Fmain%2FTemplates%2Fazuredeploy.json" target="_blank">
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
1. In the Azure Portal navigate to the [Alert Rule](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AlertsManagementSummaryBlade) blade.
1. Select __Create__ to start setting up a new alert rule that will trigger the logic app.
1. For the __Scope__ of the alert rule select the Log Analytics workspace that contains the NSG flow logs you want to monitor for malicous flows.
1. The __Condition__ that will be used is __Custom log search__.
1. Copy and paste the query below or from [Kusto_Search_Query.txt](https://raw.githubusercontent.com/cocallaw/AzFW-IPGroup-Update/main/Kusto_Search_Query.txt) into the Search query box for the condition.
```
AzureNetworkAnalyticsIPDetails_CL
| where SubType_s == 'FlowLog' and FlowType_s == 'MaliciousFlow'
| where TimeGenerated < ago(1h)
| distinct
    IP_s,
    PublicIPDetails_s,
    Location_s,
    FlowIntervalStartTime_t,
    ThreatType_s,
    ThreatDescription_s,
    DNSDomain_s

```
You may recieve a warning message about the query, stating that no results were found. You can ignore the warning message. This will occur if there are no malicous flows were detected in the last hour.
 
![Search Query Error](/Images/SQError.png)
 
6. Under __Alert Logic__  set the operator to be __Greater than__ and the __Threshold Value__ to be __0__. 
1. The __Frequency of Evaluation__ should be set to a value that makes sense for your environment. When evaluation occurs the query will run and look for any results in the last hour becasue of the TimeGenerated value in the query. It is recommended to set the frequency to a value between 5 and 30 minutes. 

![Alert Logic Settings](/Images/AlertLogic.png)

8. Click __Next: Actions__ to continue to the next step.
1. On the Actions page, select __Create action group__ to create a new action group that will trigger the logic app.   
1. Provide a Resource Group and Name for the new action group.
1. Specify if you would like any Email/SMS/Push/Voice notification to be sent when the action group is triggered on the __Notifications__ tab.
1. On the __Actions__ tab, select Webhook as the __Action Type__ and provide the a __Name__.
1. For the URI of the webhook, provide the HTTP POST URL that was saved from the _When a HTTP resquest is recived_ step of the Logic App you created earlier.
1. Make sure the that _Enable the common alert schema_ is set to No  
1. Save the action, apply any Tags and then Review and Create the Action Group. 
1. Once created the Action Group will be listed under the Alert Rule. Review the __Details__ tab settings and then create the Alert Rule.

# Refrence Links
* [Azure Monitor Alert Schema](https://docs.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-common-schema-definitions)
* [IP Groups API GET](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/get)
* [IP Groups API PUT - Create or Update](https://docs.microsoft.com/en-us/rest/api/virtualnetwork/ip-groups/create-or-update)