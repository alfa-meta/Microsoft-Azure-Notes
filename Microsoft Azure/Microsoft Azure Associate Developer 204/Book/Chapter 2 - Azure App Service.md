
ISO Compliance - International Organisation for Standardisation, which ensures best practices for security management.

SOC Compliance - Service Organisation Controls, which are reports that verify if a service's controls, like security and data handling, are effective.

PCI Compliance - Payment Card Industry standards that ensure secure handling of credit card information.


To Create an App Service, you will need to create a resource group first.

Resource group is a logical container that holds related resources for an Azure solution.

## Main levels of Scope in Azure

Tenant - dedicated instance of Microsoft Entra ID.
	Can contain one or more subscriptions.
	Highest level of organisation in Azure
	Manages users, groups, and applications across all subscriptions.

Management group - help you organise your subscriptions.
	Second-highest level of scope.
	You can apply policies and access controls to a management group - they will be inherited by all subscriptions and resources within that group.

Subscription - each subscription can have multiple resource groups and is associated with billing.
	Subscriptions separate resources for different projects, environments and teams,
	Act as a security boundary for resources.

Resource group - container for resources.
	Allows you to deploy all resources at the same time.

Resource - can be a service, virtual machine, storage account, or web app.
	Resources inherit permissions and policies from their parent resource group, subscription and management group.
	Smallest scope


`az group create --name SampleRg --location east us` - Creates a resource group called SampleRg in US East 


## What is an App Service Plan

App Service Plan - is a container for your web apps and defines the resources and features available to them.
	Supports Free, Shared, Basic, Standard, Premium, and Isolated tiers


`az appservice plan create --name --resource-group` 
	Default tier is Basic tier.

`az appservice plan create --name SamplePlan --resource-group SampleRg --sku P1V2`

The following creates a web app
`az webapp create --resource-gorup SampleRg --plan SamplePlan --name sampleApp`

ZIP Deployment
`az webapp deploy --resource-group SampleRg --name sampleApp --src-path ./path/to/file.zip --type zip --async true`


### PowerShell Commands


`Connect-AzAccount` - allows you to connect to Azure via PowerShell

`New-AzResourceGroup -Name SampleRg -Location EastUS` - creates a new resource group in East US

Creating an App Service Plan:
`New-AzAppServicePlan -Name SamplePlan -ResourceGroupName SampleRg -Tier Premium`

Creating an App Service:
`New-AzWebApp -Name sampleApp -ResourceGroupName SampleRg -AppServicePlan SamplePlan`

You can verify the creation of your app service by navigating to the Azure Portal or using the following:
`Get-AzWebApp -ResourceGroupName SampleRg -Name sampleApp`

## Configuring and Scaling App Services

Adding App Settings in Azure CLI:
```Azure CLI
az webapp config appsettings set
	--resource-group <ResourceGroupName>
	--name <AppName>
	--settings <SettingName>=<SettingValue>
```

Adding App Settings in Azure PowerShell
```PowerShell
settings = @{"MySettings" = "MyValue"}
Set-AzWebApp
	-ResourceGroupName <ResourceGroupName>
	-Name <AppName>
	-AppSettings $settings
```

### Deployment slots

Deployment slots - provide isolated environments for testing new versions of your application.

### Connection strings

Connection strings - secure values use to connect your app to databases or other services

Azure CLI:
```Azure CLI
az webapp config connection-string set
	--resource-group <ResourceGroupName>
	--name <AppName>
	--settings MyConnectionString="Server=myServer;Database=myDB;UserId=myUser;Password=myPassword;"	--connection-string-type SQLAzure
```



PowerShell:
```PowerShell
$connectionStrings = @{"MyConnectionString" = @{Value="Server=myServer;Database=Us;myDB;User Id=myUser;Password=myPassword;";
	Type="SQLAzure"}}
	
Set-AzWebApp
	-ResourceGroupName <ResourceGroupName>
	-Name <AppName>
	-ConnectionStrings $connectionStrings
```


## Scaling

Vertical Scaling - involves upgrading the service plan to a higher tier with more resources, such as CPU, memory, and additional features like custom domains and Secure Sockets Layer (SSL) support.


```Azure CLI
az appservice plan update
	--name <AppServicePlanName>
	--resource-group <ResourceGroupName>
	--sku <SkuToScaleTo>
```

```PowerShell
Set-AzWebAppServicePlan
	-ResourceGroupName <ResourceGroupName>
	-Name <AppServicePlanName>
	-Tier <SkuToScaleTo>
	-WorkerSize Large
```

