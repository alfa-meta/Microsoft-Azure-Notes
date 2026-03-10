
Azure Functions - a serverless compute service allows developers to run code on demand without having to explicitly provision or manage the underlying infrastructure.
	Platform as a Service

Regular Web API - typically has a server or set of servers running all the time listening for requests.
	Servers need to be maintained
	Scaled
	Monitored
	Consume resources even when there are no incoming requests

Functions are event-driven, meaning they execute only when triggered by an event such as an HTTP request, a message in a queue, a database change, or a scheduled timer.

### Creating Azure Function Apps



Before creating a resource you must first login
```Azure CLI
az login
```


Creating a resource group
```Azure CLI
az group create --name <resource-group-name> --location <location>
```

Once the resource group is created, you can then create a storage account, which is required for the function app, with the command:
```Azure CLI
az storage account create
	--name <storage-account-name>
	--location <location>
	--resource-group <resource-group-name>
	--sku <sku>
```

Once the storage account is in place, the next step is to create the actual function app by running:

```Azure CLI
az functionapp create
	--resource-group <resource-group-name>
	--consumption-plan-location <location>
	--name <app-name>
	--storage-account <storage-account-name>
	--runtime <runtime>
	--os-type <os-type>
	--functions-version <functions-version>
```


Doing the same but in Azure PowerShell

Logging in
```Azure PowerShell
Connect-AzAccount
```


Creating a Storage Account
```Azure PowerShell
New-AzStorageAccount
	-ResourceGroupName <resource-group-name>
	-Name <storage-account-name>
	-Location <location>
	-SkuName <sku>
```

Once the storage account is set up, create the function app itself
```Azure PowerShell
New-AzStorageAccount
	-ResourceGroupName <resource-group-name>
	-Name <storage-account-name>
	-Location <location>
	-Runtime <runtime>
	-FunctionsVersion <functions-version>
```


## Why Do Function Apps Need Storage Accounts?

A storage account is needed to support the underlying infrastructure and function of an Azure Function App.

Azure Function App use cases for a Storage Account:
	1. File Storage
	2. Manage state, allowing the storing and retrieving of data between function executions


## Deploying Azure Function Apps


### Deploying with the Azure CLI

```AzureCLI
az functionapp deployment source config-zip
	--resource-group MyResourceGroup
	--name MyFunctionApp
	--src functionapp.zip
```

### Deploying with Azure PowerShell

```Azure PowerShell
$resourceGroup = "MyResourceGroup"
$functionAppName = "MyFunctionApp"
$packagePath = "./functionapp.zip"

Publish-AzWebApp
	-ResourceGroupName $resourceGroup
	-Name $functionAppName#
	-ArchivePath $packagePath
```



## Hosting Options for Azure Function Apps

Consumption Plan - allows your functions to scale automatically based on demand
	Cost-effective for applications with unpredictable or infrequent workloads.

Flex Consumption Plan - suitable for complex serverless applications.
	Supports Event-driven scaling decisions per function.
	Supports advanced features such as a private networking and instance memory size selection.

Premium Plan - offers additional features like VNET integration, increased memory , and longer execution times.
	Always-ready instances to avoid cold starts
	Ideal for high performance applications and consistent response times
	
## Triggers and Bindings

Trigger - is a specific type of event that initiates the execution of an Azure Function.
	It can bee an HTTP request
	New message in a queue
	Timer event
	New file in a storage blob

Bindings - are used to connect the function to other resources or services, allowing it to read or write data without needing additional code to handle these interactions.

Example HTTP request 
```C#
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Function,
"get",
"post",
Route = null)] HttpRequest req, ILogger log)
{
	log.LogInformation("C# HTTP trigger function was called.");
	
	string name = req.Query["name"];
	return name != null
		? (ActionResult) new OkObjectResult($"Hello, {name}")
		: new BadREquestObjectResult("Please add a name");
}
```

Example Timer trigger
```C#
public static void Run([TimerTrigger("0 */5 * * * *")] TiemrInfo myTimer, ILogger log)
{
	log.LogInformation($"Function executed at: {DateTime.Now}");
}
```

Example Blob Storage trigger
```C#
public static void Run([BlobTrigger("samples-workitems/{name}", Connection = "AzureWebJobsStorage")]
Stream myBlob, string name, ILogger log)
{
	log.LogInformation($"Blob trigger function Processed blob \n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```


Example Queue Storage trigger
```C#
public static void Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsSotrage")] string myQueueItem, ILogger log)
{
	log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
}
```


Example Service Bus trigger
```C#
public static void Run([ServiceBusTrigger("myqueue", Connection = "AzureWebJobsServiceBus")] string myQueueItem, ILogger log)
{
	log.LogInformation($"C# ServiceBus queue trigger function processed messag: {myQueueItem}");
}
```

Example Cosmos DB trigger
```C#
public static void Run([CosmosDBTrigger(
	databaseName: "ToDoList",
	collectionName: "Items",
	ConnectionStringSetting = "CosmosDBConnection",
	LeaseCollectionName = "leases")] IReadOnlyList<Document> input, ILogger log)
{
	if (input != null && input.Count > 0)
	{
		log.LogInformation("Documents modified " + input.Count);
		log.LogInformation("First document id " + input[0].Id);
	}
}
```

Function is triggered whenever documents in the Items collection of the `ToDoList` database are modified.
Logs the number of modified documents and the ID of the first document.


Bindings in Azure Functions make it easier to work with various services without writing boilerplate code.
For example output binding can write data to a blob storage directly from a function

```C#
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, [Blob("output-container/{rand-guid}.txt", FileAccess.Write)]
Stream outputBlob, ILogger log)
{
	string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
	byte[] byteArray = Encoding.UTF8.GetBytes(requestBody);
	outputBlob.Write(byteArray, 0, byteArray.Length);
	
	return new OkObjectResult("Data written to blob");
}
```

In this example, the function reads the body of an HTTP request and writes it to a new blob in the output-container.
The blob name includes a random GUID (Globally Unique Identifier) to ensure it is unique.


```C#
publoic static IActionResult Run(
	[HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]
		HttpRequest req,
	[CosmosDB(
		databaseName: "ToDoList",
		collectionName: "Items",
		ConnectionStringSetting = "CosmosDBConnection",
		Id = "{Query.id}",
		PartitionKey = "{Query.partitionKey}")] ToDoItem todoItem,
		ILogger log)
{
	if (todoItem != null)
	{
		return new OkObjectResult(todoItem);
	}
	else
	{
		return new NotFoundResult();
	}
}
```


## Blue-Green Deployment with Slots

First, create a new deployment slot named `staging` for a function app named `SampleFunctionApp` in the resource group `SampleRg`.

```AzureCLI
az functionapp deployment slot create 
	--name SampleFunctionApp
	--resource-group SampleRg
	--slot staging
```


```Azure PowerShell
New-AzFunctionAppSlot
	-ResourceGroupName "SampleRg"
	-Name "SampleFunctionApp"
	-Slot "staging"
```

After successfully creating your stating slot, deploy your new application to the staging slot

```Azure CLI
az functionapp deployment source config-zip
	--resource-group SampleRg
	--name SampleFunctionApp
	--src ./path/to/file.zip
	--slot staging
```

```Azure PowerShell
Publish-AzWebApp
	-ResourceGroupName SampleRg
	-Name SampleFunctionApp
	-Slot staging
	-ArchivePath "./path/to/file.zip"
```

Once your function app has been deployed to the staging slot, you may run all your tests there until they pass or you get a satisfactory result.

```Azure CLI
az functionapp deployment slot swap
	--resource-group SampleRg
	--name SampleFunctionApp
	--slot staging
```

```Azure PowerShell
Swap-AzWebAppSlot
	-ResourceGroupName "SampleRg"
	-Name "SampleFunctionApp"
	-SourceSlotName "staging"
```


## Automating Azure Function Deployments

Azure DevOps
```YAML
- task: AzureFunctionApp@1
  inputs:
	  azureSubscription: 'your-service-connection'
	  appName: 'MyFunctionApp'
	  package: $(Build.ArtifactStagingDirectory)/$(Build.BuildId)
```


GitHub Actions
```YAML
- name: Deploy to Azure Function App
  uses: azure.functions-action@v1
  with: 
	  app-name: 'MyFunctionApp'
	  publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
	  package: './publish'
```



## Writing Custom Logs in Your Code

C# uses ILogger interface for logging.
```C#
log.LogInformation($"Function processed: {item}");
```

JavaScript/TypeScript
```TypeScript
context.log('Http function was triggered.');
```

Python
```Python
loggin.info('HTTP trigger function processed as a request')
```

Java
```Java
logger.log(Level.INFO, "Function processed as a request.");
```


## Monitoring and Debugging Azure Functions

### Application Insights and Azure Monitor

App Insights and Azure monitor - provide comprehensive monitoring capabilities for your Azure Functions.
	The tools enable you to collect telemetry data, set up alerts, and create dashboards for monitoring the performance and usage of your functions.

### Azure Functions Runtime Logs

Example `host.json` file for logging
```JSON
{
	"version": "2.0",
	"logging": {
		"loglevel": {
			"Function.MyFunction": "Information"
		},
		"applicationInsights": {
			"samplingSettings": {
				"isEnabled": true
			}
		}
	}
}
```


Accessing logs with Azure CLI
```Azure CLI
az webapp log tail
	--resource-group <resource-group-name>
	--name <function-app-name>
```


Accessing logs with Azure PowerShell
```Azure PowerShell
Set-AzWebApp
	-RequestTracingEnabled $True
	-HttpLoggingEnabled $True
	-DetailedErrorLoggingEnabled $True
	-ResourceGroupName $ResourceGroupName
	-Name $AppName
```

### Kudu

Kudu - has a process explorer, which allows you see what processes are running in your function app environment.
	You can see detailed process information including ID and CPU and memory usage.

Tools section displays:
	Displays diagnostic information and logs
	Deployment logs
	Can help you understand what happened during a deployment

## Advanced Functions Concepts and Optimisations

By default, Azure Functions are stateless.
Function executions are independent.

Durable Functions - are an extension of Azure Functions that allow you to write stateful workflows in a serverless environment.

Durable Functions allow you to build workflows that can resume from the last checkpoint.
Workflows can work even with interruptions.



### Orchestrator functions

Orchestrator Functions - define the workflow and can call other functions, manage their outputs, and handle retries and error handling.

Orchestrator functions maintain their state between calls, allowing them to resume from where they left off after each awaited operation.

```C#
[FunctionName("DurableOrchestrationFunction")]
public static async Task RunOrchestrator([OrchestrationTrigger] IDurableOrchestrationContext context)
{
	var output1 = await context.CallActivityAsync<string>("ActivityFunction1", null);
	var output2 = await context.CallActivityAsync<string>("ActivityFunction2", output1);
	var output3 = await context.CallActivityAsync<string>("ActivityFunction3", output2);
	
	return output3;
}
```

### Activity functions

Activity functions perform the actual work and are called by the orchestrator functions.
	Stateless and can be executed multiple times if needed.

```C#
[FunctionName("ActivityFunction1")]
public static string RunActivity1([ActivityTrigger] string input, ILogger log)
{
	log.LogInformation($"Processing input: {input}");
	return $"Processed {input}";
}
```

### Entity functions

Entity functions - manage state directly.
	Can be invoked explicitly by sending messages to them.
	Not automatically triggered when the state is mutated.
	Must be called directly.

```C#
[FunctionName("EntityFunction")]
public static void EntityFunction([EntityTrigger] IDurableEntityContext ctx)
{
	switch (ctx.OperationName.ToLowerInvariant())
	{
		case "add":
			ctx.SetState(ctx.GetState<int>() + ctx.GetInput<int>());
			break;
		case "reset":
			ctx.SetState(0);
			break;
		case "get":
			ctx.Return(ctx.GetState<int>());
			break;
	}
}
```

### Binding Expressions

You can use binding expressions to dynamically bind parameters to functions based on runtime data.

```C#
public static void Run([QueueTrigger("samples-workitems", Connection = "AzureWebJobsStorage")] string myQueueItem, [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "AzureWebJobsStorage")] Stream myBlob, ILogger log)
{
	log.LogInformation($"Function processed: {myQueueItem}");
}
```

