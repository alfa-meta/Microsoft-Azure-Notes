# Azure CLI — App Service Reference (AZ-204)

---

## 1. Resource & Plan Setup

|Command|Purpose|
|---|---|
|`az group create --name <rg> --location uksouth`|Create a resource group — always the first step|
|`az appservice plan create --name <plan> --resource-group <rg> --sku B1 --is-linux`|Create a Linux App Service plan|
|`az appservice plan update --name <plan> --resource-group <rg> --sku S1`|Scale up (vertical scaling) an existing plan|
|`az appservice plan delete --name <plan> --resource-group <rg>`|Delete a plan|

**SKU tiers:** F1 (Free) → B1 (Basic) → S1 (Standard) → P1V2 (Premium)

---

## 2. Web App Creation

|Command|Purpose|
|---|---|
|`az webapp create --name <app> --resource-group <rg> --plan <plan>`|Basic web app|
|`az webapp create ... --runtime "DOTNET:8.0"`|Specify a runtime|
|`az webapp create ... --deployment-container-image-name <image>`|Deploy from a Docker image|

**Common runtime strings:** `"NODE:20-lts"` · `"PYTHON:3.11"` · `"JAVA:17:JAVA SE:17"` · `"DOTNET:8.0"`

---

## 3. Configuration & App Settings

|Command|Purpose|
|---|---|
|`az webapp config appsettings set ... --settings KEY=VALUE`|Set app settings (env vars)|
|`az webapp config appsettings list ...`|List all app settings|
|`az webapp config appsettings delete ... --setting-names KEY`|Delete an app setting|
|`az webapp config connection-string set ... --connection-string-type SQLAzure --settings MyDb="..."`|Set a connection string|
|`az webapp config set ... --always-on true`|Enable Always On (required for WebJobs; Basic tier+)|
|`az webapp config set ... --http20-enabled true`|Enable HTTP/2|
|`az webapp config set ... --min-tls-version 1.2`|Set minimum TLS version|

> **App settings vs connection strings:** Different storage mechanisms with different use cases — know the distinction.

---

## 4. Deployment

|Command|Purpose|
|---|---|
|`az webapp deployment source config-zip ... --src app.zip`|Deploy a ZIP file (Kudu v1)|
|`az webapp deploy ... --src-path app.zip --type zip`|Deploy a ZIP file (Kudu v2 — newer)|
|`az webapp deployment source config ... --repo-url <url> --branch main --manual-integration`|Configure Git-based continuous deployment|
|`az webapp log deployment show ...`|Show deployment logs|

---

## 5. Deployment Slots

|Command|Purpose|
|---|---|
|`az webapp deployment slot create ... --slot staging`|Create a staging slot (Standard tier+)|
|`az webapp deployment slot swap ... --slot staging`|Swap staging into production|
|`az webapp deployment slot list ...`|List all slots|
|`az webapp deployment slot show ... --slot staging`|Show slot config (incl. auto-swap status)|
|`az webapp deployment slot auto-swap ... --slot staging`|Enable auto-swap on staging|
|`az webapp deployment slot auto-swap ... --slot staging --auto-swap-slot production`|Auto-swap to a specific target slot|
|`az webapp config appsettings set ... --slot staging --settings KEY=VALUE --slot-setting true`|Set a sticky (slot-specific) setting|

> **Key rule:** `--slot-setting true` marks a setting as sticky — it does **not** travel with the slot during a swap.

---

## 6. Scaling

|Command|Purpose|
|---|---|
|`az monitor autoscale create --resource-group <rg> --resource <app> --resource-type Microsoft.Web/serverfarms --name autoscale-rule --min-count 1 --max-count 5 --count 1`|Create an autoscale setting on the plan|
|`az monitor autoscale rule create ... --condition "CpuPercentage > 70 avg 5m" --scale out 2`|Add a CPU-based scale-out rule|

> You scale the **plan**, not the individual app. Scale-out = horizontal (more instances); scale-up = vertical (bigger SKU).

---

## 7. Logging & Monitoring

|Command|Purpose|
|---|---|
|`az webapp log config ... --web-server-logging filesystem --level information`|Configure application logging|
|`az webapp log tail ...`|Stream live logs|
|`az webapp log download ... --log-file logs.zip`|Download logs as a ZIP|

**Log levels:** `off` · `error` · `warning` · `information` · `verbose`

---

## 8. Identity & Security

|Command|Purpose|
|---|---|
|`az webapp identity assign ...`|Assign a system-assigned managed identity|
|`az webapp identity assign ... --identities <resourceId>`|Assign a user-assigned managed identity|
|`az webapp auth update ... --enabled true --action LoginWithAzureActiveDirectory`|Enable Easy Auth with Azure AD (no code changes needed)|

> **System-assigned:** tied to app lifecycle. **User-assigned:** independent lifecycle, shareable across resources.

---

## 9. Networking (VNet Integration)

|Command|Purpose|
|---|---|
|`az webapp vnet-integration add ... --vnet <vnet> --subnet <subnet>`|Connect app to a VNet subnet for outbound access to private resources|
|`az webapp vnet-integration list ...`|List all VNet integrations|
|`az webapp vnet-integration remove ...`|Remove VNet integration|

> The subnet must be delegated to App Service before use.

---

## 10. Custom Domains & TLS

|Command|Purpose|
|---|---|
|`az webapp config hostname add --webapp-name <app> --resource-group <rg> --hostname <domain>`|Bind a custom domain (DNS verification required first)|
|`az webapp config ssl upload ... --certificate-file <path> --certificate-password <pass>`|Upload a PFX certificate (returns thumbprint)|
|`az webapp config ssl bind ... --certificate-thumbprint <thumb> --ssl-type SNI`|Bind certificate to domain via SNI SSL|

> SNI SSL is the standard choice. IP SSL is legacy and incurs extra cost.

---

## 11. Containers & Private Registry

|Command|Purpose|
|---|---|
|`az webapp config container set ... --docker-registry-server-url https://<acr>.azurecr.io --docker-registry-server-user <user> --docker-registry-server-password <pass>`|Configure a private container registry|
|`az webapp config container show ...`|Show current container config|
|`az webapp config container delete ...`|Remove container config (reverts to code-based deployment)|

> For Azure Container Registry, managed identity is preferred over credentials.

---

## 12. WebJobs

|Command|Purpose|
|---|---|
|`az webapp webjob continuous list ...`|List continuous WebJobs and their status|
|`az webapp webjob continuous start ... --webjob-name <job>`|Start a continuous WebJob (requires Always On)|
|`az webapp webjob triggered run ... --webjob-name <job>`|Manually trigger a triggered WebJob|

> Triggered jobs can also be scheduled via a CRON expression in a `settings.job` file.

---

## Key Exam Patterns

|Topic|What to know|
|---|---|
|`config appsettings set` vs `config connection-string set`|Different storage, different use cases|
|`deployment slot swap`|Understand what sticks (slot settings) and what swaps (non-sticky settings)|
|`--slot` flag|Most `webapp` commands accept it to target a specific slot|
|`identity assign`|Gateway to passwordless Key Vault access|
|`log config` + `log tail`|Know both configuring and streaming logs|
|Always On|Required for WebJobs; Basic tier and above only|
|Autoscale|Targets the plan (`Microsoft.Web/serverfarms`), not the app|
