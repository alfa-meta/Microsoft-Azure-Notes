

Bicep - declarative Domain Specific Language (DSL) for delivering Azure resources.
## Understanding IaC

Infrastructure as Code - acts as a blueprint for your resources, ensuring that the same configuration is applied every time it is executed.

Configuration Drift - manual updates being applied to different environments over time.
	Making mismatches between different environments.

## Azure Bicep Language Essentials

Setting up a Storage Account
```Bicep
resource myStorageAccount 'Microsoft.Storage/storageAccounts@2021-04-01' = {
	name: 'teststorage'
	location: 'eastus'
	sku: {
		name: 'Standard_LRS'
	}
	kind: 'StorageV2'
}
```

### Parameters

Parameters - allow you to pass values into a Bicep file at the time of deployment, making it possible to use the same template in different environments with different configurations.

```Bicep
param deploymentRegion string = 'eastus'
```

To deploy using the parameter file
```Azure CLI
az deployment group create
	--resource-group sample-resource-group
	--template-file main.bicep
	--paramters @parameters.json
```

### Conditionals

Conditionals - in Bicep help us include or exclude resources and configurations based on specific conditions.

```Bicep
param deployStroage bool = true

resource myStorageAccount 'Microsoft.Storage/storageAccoutns@2021-04-01' =
if (deployStorage) {
	name: 'mystorageaccount'
	location: 'eastus'
	sku: {
		name: 'Standard_LRS'
	}
	kind: 'StorageV2'
}
```


### Loops

Loops allow you to deploy multiple instances of a resource based on a set of input values.

```Bicep
param numberOfInstances int = 3

resource myStorageAccounts 'Microsoft.Storage/storageAccounts@2021-04-01' =
[for i in range(0, numberOfInstances): {
	name: 'mystorageaccount$(i)'
	location: 'eastus'
	sku: {
		name: 'Standard_LRS'
	}
	kind: 'StorageV2'
}]
```


### Variables

Variables are only used within the Bicep file to store intermediate values or results of calculations.

```Bicep
param baseName string = 'app'
param numberOfInstances int = 3
param baseResourceGroupId string = '/subscriptions/12345678/resourceGroups/base-rg'

var storageAccountNames = [for i in range(0, numberOfInstances): '$(baseName)storage$(i)']
var resourceGroupId = '${baseResourceGroupId}/subGroups/${environment}'

resource storageAccounts 'Microsoft.Storage/storageAccounts@2021-04-01' = 
[for i in range(0, numberOfInstances): {
	name: storageAccountNames[i]
	location: 'eastus'
	sku: {
		name: 'Standard_LRS'
	}
	kind: 'StorageV2'
}]
```

### Outputs

How to declare outputs in Bicep
```Bicep
output storageAccountId string = myStorageAccount.id
```


First Bicep template `main.bicep`
```Bicep
param location string

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-04-02' = {
	name: 'storageaccount${uniqueString(resourceGroup().id)}'
	location: location
	sku: {
		name: 'Standard_LRS'
	}
	kind: 'StorageV2'
}
```

Second Bicep template `dependent.bicep`
```Bicep
param storageAccountName string

resource storageAccount 'Microsoft.Storage/storageAccounts@2021-04-01' existing = {
	name: storageAccountName
}

resource storageContainer 'Microsoft.Storage/storageAccounts/blobServices/containers@2023-04-02' = {
	name: 'mycontainer'
	parent: storageAccount
}
```

Azure Pipeline
```YAML
trigger:
	- main

pool:
	vmImage: 'ubuntu-latest'

steps:
	- task: AzureCLI@2
	  
inputs:
	azureSubscriptions: 'YourSubscriptionName'
	scriptType: 'bash'
	script: |
		az deployment group create \
			--name myResourceGroup \
			--template-file main.bicep \
			--parameters location=westus
			
- task: PowerShell@2
  inputs:
	  targetType: 'inline'
	  script: |
		  $deployment = az deployment group show --name myResourceGroup --output json
		  $storageAccountName = $deployment.properties.outputs.storageAccountName.value
		  Write-Host "##vso[task.setvariable variable=storageAccountName]$storageAccountName"
		  
- task: AzureCLI@2
  inputs:
	  azureSubscripton: 'YourSubscriptionName'
	  scriptType: 'bash'
	  script: |
		  az deployment group create \
			  --name myResourceGroup \
			  --tempalte-file dependent.bicep \
			  --parameters storageAccountName=$(storageAccountName)
```

The preceding code snippet is executed in the following steps:
- The first Bicep template creates a storage account and outputs its name
- The second Bicep template creates a container in a storage account based on the provided name
- The pipeline deploys the first template
- It extracts the output variable `storageAccountName` from the deployment and sets it as a pipeline variable
- The pipeline deploys the second template, passing the `storageAccountName` as a parameter.


### Modules

Modules in Bicep allow you to encapsulate and reuse infrastructure code.
	Module is a separate file that defines a set of resources.


Defining a Website Module
```Bicep
param appServicePlanName string
param appName string
param location string

resource appServicePlan 'Microsoft.Web/serverfarms@2021-02-01' = {
	name: appServicePlanName
	location: location
	sku: {
		name: 'P1v2'
		tier: 'PremiumV2'
	}
}

resource webApp 'Microsoft.Web/sites@2021-02-01' = {
	name: appName
	location: location
	properties: {
		serverFarmId: appServicePlan.id
		httpsOnly: true
	}
}

resource appInsights 'Microsoft.Insights/components@2020-02-02' = {
	name: '${appName}-ai'
	location: location
	properties: {
		Application_Type: 'web'
	}
}

output appInsightsInstrumentationKey string = appInsights.properties.InsturmentationKey
```

In the preceding module, three Azure resources were created.

`appServicePlan` defines an App Service Plan, which determines the pricing tier and the amount of resources allocated to your web app.

`webApp` defines the app service (web app) itself, specifying the `appServicePlan.id` to link it with the App Service plan.

`appInsights` sets up Application Insights for monitoring and logging.

## Deploying Resources with Azure Bicep

Azure Bicep allows you to deploy Azure resources declaratively, specifying the desired state in a Bicep file.
	This approach ensures resources are consistently created and configured.


### Resource Group Scope

The most common deployment scope is the resource group.

```Bicep
targetScope = 'resourceGroup'

resource myCosmosDB 'Microsoft.DocumentDB/databaseAccounts@2021-06-15' = {
	name: 'mycosmosdb'
	location: 'eastus'
	properties: {
		databaseAccountOfferType: 'Standard'
		consistencyPolicy: {
			defaultConsistencyLevel: 'Session'
		}
	}
}
```

In this example, a Cosmos DB Account is deployed in the `eastus` region.
The `target` Scope is set to `resourceGroup`, indicating that the deployment will be within a specific resource group.

### Subscription Scope

## Managing and Updating Deployed Infrastructure

## Best Practices for IaC in Azure

