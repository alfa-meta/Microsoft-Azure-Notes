
---
## Prerequisites & Storage

1. `az storage account create --name <sa> --resource-group <rg> --location uksouth --sku Standard_LRS` Create a storage account. A storage account is **mandatory** for Function Apps — it stores trigger state, keys, and logs. Know `Standard_LRS` is the minimum required SKU.
    
2. `az storage account show-connection-string --name <sa> --resource-group <rg>` Retrieve the storage connection string. Used when manually wiring up `AzureWebJobsStorage`.
    

---
## Function App Creation

3. `az functionapp create --name <func> --resource-group <rg> --storage-account <sa> --consumption-plan-location uksouth --runtime dotnet --functions-version 4` Create a Function App on the **Consumption plan** (serverless). This is the most tested creation command. Know `--runtime` values: `dotnet`, `dotnet-isolated`, `node`, `python`, `java`, `powershell`.
    
4. `az functionapp create --name <func> --resource-group <rg> --storage-account <sa> --plan <plan> --runtime python --runtime-version 3.11 --functions-version 4` Create a Function App on an **existing App Service / Premium plan**. Know when to use Consumption vs Premium: Premium supports VNet integration, no cold starts, and always-ready instances.
    
5. `az functionapp create --name <func> --resource-group <rg> --storage-account <sa> --consumption-plan-location uksouth --runtime dotnet --functions-version 4 --os-type Linux` Create a Linux-based Function App. Know that some runtimes (e.g. `python`) require Linux. `--os-type` is `Windows` by default.
    
6. `az functionapp create --name <func> --resource-group <rg> --storage-account <sa> --plan <plan> --deployment-container-image-name <image>` Create a Function App from a Docker container image. Requires a Premium or Dedicated plan — not supported on Consumption.
    

---
## Configuration & App Settings

7. `az functionapp config appsettings set --name <func> --resource-group <rg> --settings KEY=VALUE` Set application settings (environment variables). Heavily tested. Know that `AzureWebJobsStorage` and `FUNCTIONS_WORKER_RUNTIME` are mandatory settings set automatically on creation.
    
8. `az functionapp config appsettings list --name <func> --resource-group <rg>` List all application settings including built-in ones.
    
9. `az functionapp config appsettings delete --name <func> --resource-group <rg> --setting-names KEY` Delete an application setting.
    
10. `az functionapp config set --name <func> --resource-group <rg> --always-on true` Enable always-on. **Only available on App Service / Premium plans** — not Consumption. On Consumption, always-on is irrelevant as the host scales to zero by design.
    

---

## Deployment

11. `az functionapp deployment source config-zip --name <func> --resource-group <rg> --src func.zip` Deploy a ZIP package. The most commonly tested deployment method for Function Apps. Know that with `WEBSITE_RUN_FROM_PACKAGE=1` set, the ZIP is mounted read-only rather than extracted.
    
12. `az functionapp deployment source config --name <func> --resource-group <rg> --repo-url <url> --branch main --manual-integration` Configure continuous deployment from a Git repository.
    

---

## Identity & Security

13. `az functionapp identity assign --name <func> --resource-group <rg>` Assign a system-assigned managed identity. Heavily tested in conjunction with Key Vault access — eliminates the need to store credentials in app settings.
    
14. `az functionapp identity assign --name <func> --resource-group <rg> --identities <resourceId>` Assign a user-assigned managed identity. Know the distinction: system-assigned is tied to the app's lifecycle and deleted with it; user-assigned is independent and can be shared across resources.
    

---

## Scaling & Plans

15. `az functionapp plan create --name <plan> --resource-group <rg> --location uksouth --sku EP1 --is-linux` Create a **Premium (Elastic Premium) plan**. Know the SKUs: `EP1`, `EP2`, `EP3`. Premium supports pre-warmed instances, VNet integration, and unlimited execution duration. Consumption has a 10-minute timeout (5 minutes default); Premium is unlimited.
    
16. `az functionapp update --name <func> --resource-group <rg> --plan <premiumPlan>` Move a Function App from Consumption to a Premium plan. Know you can scale **up** from Consumption to Premium but **cannot scale back down** to Consumption via CLI.
    

---

## Logging & Monitoring

17. `az functionapp log tail --name <func> --resource-group <rg>` Stream live logs from a running Function App. Same pattern as `az webapp log tail`.
    
18. `az monitor app-insights component create --app <ai> --resource-group <rg> --location uksouth` Create an Application Insights instance. Know that `APPINSIGHTS_INSTRUMENTATIONKEY` or `APPLICATIONINSIGHTS_CONNECTION_STRING` must be set in app settings to wire it up — connection string is the modern preferred approach.
    

---

## Durable Functions

19. `az functionapp config appsettings set --name <func> --resource-group <rg> --settings AzureWebJobsStorage=<connectionString>` Durable Functions require `AzureWebJobsStorage` to be explicitly set — it uses Azure Storage (Tables, Queues, Blobs) as the orchestration state store by default. Know this is the key dependency for Durable Functions.

---

## Slots

20. `az functionapp deployment slot create --name <func> --resource-group <rg> --slot staging` Create a deployment slot for a Function App. **Only available on Premium and App Service plans** — not Consumption. Slot swap behaviour is identical to App Service: sticky settings do not swap, non-sticky settings do.

---

## Key Concepts the Exam Tests Beyond Commands

**Hosting plans** are the most tested topic in this area — know all three cold:

|Plan|Timeout|Scale|VNet|Always On|
|---|---|---|---|---|
|Consumption|5 min (max 10)|Automatic|No|No|
|Premium (EP1-3)|Unlimited|Automatic + pre-warmed|Yes|Yes|
|Dedicated (App Service)|Unlimited|Manual / autoscale|Yes|Required|

**Triggers vs Bindings** — know the difference: a trigger is what invokes the function (only one allowed); bindings are declarative input/output connections (many allowed). You configure both in `function.json` or via attributes in code.

**`WEBSITE_RUN_FROM_PACKAGE=1`** is a commonly tested app setting — when set, the deployed ZIP is mounted directly rather than extracted to the file system, which improves cold start performance and prevents file modification at runtime.

**Durable Functions patterns** appear regularly: Function Chaining, Fan-out/Fan-in, Async HTTP API, Monitor, and Human Interaction. Know which orchestration pattern fits which scenario.