
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


### Creating Autoscaling  settings

Horizontal Scaling - adds more instances of your app to distribute the load.

```Azure CLI
az monitor autoscale create ## 1
	--resource-group {resource-group-new} ## 2
	--resource {resource-id} ## 3
	--min-count 2 ## 4
	--max-count 9 ## 5
	--count 4 ## 6
```

1. This command is used to establish autoscale settings for an Azure resource
2. This parameter denotes the resource group where these settings will be applied, which you replace with your actual resource group name.
3. Identifies the specific resource, such as an App Service, that the autoscale setting will govern, using its unique resource ID.
4. Ensures there will always be a minimum of two instances running, providing a baseline capacity to handle basic traffic.
5. Sets an upper limit of nine instances, preventing excessive scaling that could lead to high operational costs.
6. This parameter sets the default number of instances to start with, meaning that under typical conditions, the application will operate with four instances.



### Creating Scaling out rule

```Azure CLI
az monitor autoscale rule create
	--resource-group {resource-group-new}
	--autoscale-name {resource-name} ## 1
	--scale out 1 ## 2
	--condition "Percentage CPU > 75 avg 5m" ## 3
```

1. This parameter indicates the name of the autoscale setting that will use this rule.
2. This parameter defines the action to be taken when the condition is met, which in this case is to add one instance.
3. This parameter specifies the condition that triggers this scaling action. This rule states that if the average CPU usage exceeds 75% over a five-minute period, an additional instance will be added to handle the increased load.


Metric-based rules - these trigger scaling actions based on performance metrics.
	For instance if a certain resource, like CPU or memory, exceed a set threshold, the system can automatically scale out to handle increased demand.

Time-based rules - these allow you to scale your resources based on a predefined schedule.
	You could set up scale out during peak hours.

### Creating the scale-in rule

```Azure CLI
az monitor autoscale rule create
	--resource-group {resource-group-name}
	--autoscale-name {resource-name}
	--scale in 1
	--condition "Percentage CPU < 25 avg 5m"
```


This removes one instance of a resource if CPU percentage falls below 25% over a 5 minute period.


### Autoscaling in the Azure portal

Maximum burst - number of instances your App Service plan can scale out under load.
	Its value should be greater than or equal to current instances for the plan.

Always ready instances - number of instances that are always ready for the web app to use by default.

Note that if automatic scaling is enabled, rules-based scaling will be ignored.


## Blue-Green Deployment with Slots

Blue-Green deployment - is a technique that reduces downtime and risk by running two identical production environments, known as blue and green.

In Azure blue-green deployments can be implemented using deployment slots.

### Creating a deployment slot

```Azure CLI
az webapp deployment slot create
	--name sampleApp
	--resource-group SampleRg
	--slot staging
	
```

```Azure PowerShell
New-AzWebAppSlot
	-ResourceGroupName "SampleRg"
	-Name "sampleApp"
	-Slot "staging"
```


### Deploying your new application

```Azure CLI
az webapp deploy
	--resource-grouip SampleRg
	--name sampleApp
	--src-path "./path/to/file.zip"
	--slot "staging"
	--type zip
	--async true
```


```PowerShell
Publish-AzWebApp
	-ResourceGroupName SampleRg
	-Name sampleApp
	-Slot staging
	-ArchivePath "./path/to/file.zip"
```


### Swapping staging slot with the production slot

```AzureCLI
az webapp deployment slow swap
	--resource-group SampleRg
	--slot staging
```


```PowerShell
Swap-AzWebAppSlot
	-ResourceGroupName "SampleRg"
	-Name "sampleApp"
	-SourceSlotName "staging"
	-DestinationSlotName "production"
```


## Automating App Service Deployments

Azure Service Connection - allows you to securely connect and authenticate with Azure resources from external tools or services, such as Azure DevOps, GitHub Actions, or other CI/CD pipelines.

### Using Azure DevOps

```YAML
trigger:
	branches:
		include:
			- main
			  
pool:
	vmImage: 'ubuntu-latest'

steps:
	- task: UseDotNet@2
	  inputs:
		  packageType: 'sdk'
		  version: '5.x'
		  installationPath: $(Agent.ToolsDirectory)/dotnet
	
	- script: dotnet build --configuration Release
	  displayName: 'Build Project'
	  
	- task: ArchiveFiles@2
	  inputs:
		  rootFolderOrFile: $(System.DefaultWorkingDirectory)
		  includeRootFolder: false
		  archiveType: 'zip'
		  archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
		  replaceExistingArchive: true
	
	- task: PublishBuildArtifacts@1 
	  inputs:
		  pathToPublsih: $(PublishBuildArtifactStagingDirectiry)
		  artifactName: drop
		  publishLocation: 'Container'
	
	- task: AzureWebApp@1
	  inputs:
		  azureSubscription: 'your-service-connection'
		  appName: 'MyAppService'
		  package: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
```


Setting up a Service Connection using the Azure CLI:

```Azure CLI
# Create a service principal
az ad sp create-for-rbac
	--name http://my-service-connection
	--role contributor
	--scopes /subscriptions/{subscription-id}/resourceGroups/{rg-name}
```

### Using GitHub Actions

```GitHub Actions
name: Build and Deploy to Azure Web App

on:
	push:
		branches:
			- main
			  
jobs:
	build-and-deploy:
		runs-on: ubuntu-latest
		
		steps:
			- name: Checkout code
			  uses: actions/checkout@v2
			
			- name: Set up .NET
			  uses: actions/setup-dotnet@v2
			  with:
				  dotnet-version: '5.x'
			
			- name: Build project
			  run: dotnet build --configuration Release
			
			- name: Publish artifact
			  run: dotnet publish -C Release -o ./publish
			  
			- name: Deploy to Azure Web App
			  uses: azure/webapp-deploy@v2
			  with:
				  app-name: 'MyAppService'
				  slot-name: 'production'
				  publish-profile: $({ secrets.AZURE_WEBAPP_PUBLISH_PROFILE })
				  package: './publish'
```

1. The workflow triggers pushes to the main branch.
2. Checks out the code.
3. Sets up the .NET environment
4. Builds and publishes the project
5. Deploys the application to an Azure Web App using `azure/webapps-deploy` action


### Cleaning Up: Deleting App Service Resources and resource Groups

```Azure PowerShell
az webapp delete
	--name <app-name>
	--resource-group <resource-group-name>
```

This will delete the Web App from the resource group.
However, if you've created other resources like databases or storage accounts, you'll need to delete them individually or delete the entire resource group.

```Azure CLI
az group delete
	--name <resource-group-name>
	--yes
	--no-wait
```

--yes - flag skips confirmation prompt
--no-wait - allows the deletion to proceed asyncrhonously.

### Deleting an individual App Service

```Azure PowerShell
Remove-AzWebApp
	-Nmae <app-name>
	-ResourceGroupName <resource-group-name>
```


### Deleting the entire Resource Group, including its Resources

```Azure PowerShell
Remove-AzResourceGroup
	-Name <resource-group-name>
	-Force
```







