## Azure App Service Web Apps — Exam Objectives

**1. Create an Azure App Service Web App**

- Understand App Service Plans (tiers: Free, Shared, Basic, Standard, Premium, Isolated)
- Know which tier enables which features (e.g., custom domains, SSL, autoscale, deployment slots)
- Creating via portal, CLI (`az webapp create`), ARM templates, and SDK

**2. Configure and implement diagnostics and logging**

- Application logging, web server logging, detailed error pages, failed request tracing
- Enable/stream logs via portal or CLI
- Log storage destinations: filesystem vs. Blob Storage
- Integrate with Application Insights

**3. Deploy code and containerized solutions**

- Deployment methods: ZIP deploy, FTP, Git (local Git, GitHub Actions, Azure DevOps), Run from Package
- Deploying container images from ACR
- Understand `WEBSITES_ENABLE_APP_SERVICE_STORAGE` and similar settings for containers

**4. Configure settings including TLS, API settings, and service connections**

- TLS/SSL bindings, minimum TLS version, HTTPS-only enforcement
- App settings vs. connection strings (and how they're exposed as environment variables)
- CORS configuration
- Virtual Network integration and hybrid connections
- Managed Identity-based service connections (e.g., to Key Vault)

**5. Implement autoscaling**

- Scale up (change plan tier) vs. scale out (add instances)
- Manual scaling vs. autoscale rules (metric-based and schedule-based)
- Autoscale requires Standard tier or higher
- Know the metrics: CPU %, memory, HTTP queue length, etc.
- Cool-down periods and rule evaluation logic

**6. Configure deployment slots**

- Slots are available on Standard tier and above
- Slot-specific vs. "sticky" settings (settings that don't swap)
- Swap process: production ↔ staging, warm-up before swap
- Auto-swap configuration
- Traffic routing (percentage) to slots for A/B testing
- Swapping rolls back by swapping again — no separate rollback command

---

## High-Frequency Exam Topics (what actually shows up)

- **Which App Service Plan tier supports what**: deployment slots (Standard+), autoscaling (Standard+), isolated network (Isolated/ASE)
- **Deployment slot swap behavior**: what gets swapped vs. what's slot-specific (e.g., connection strings marked as "slot setting" do NOT swap)
- **Managed Identity for Key Vault access**: enable system-assigned identity on the App Service, then grant it Key Vault access policy
- **WebJobs**: triggered vs. continuous, SDK for queue-triggered jobs — expect at least one scenario question
- **Run from package**: `WEBSITE_RUN_FROM_PACKAGE=1` app setting, files are read-only at runtime
- **Scaling**: autoscale is not available on Free/Shared/Basic — a common trap question

---

## Quick CLI Commands to Know

```bash
az webapp create --name <name> --resource-group <rg> --plan <plan>
az webapp deployment slot create --name <app> --resource-group <rg> --slot staging
az webapp deployment slot swap --name <app> --resource-group <rg> --slot staging
az webapp log config --name <app> --resource-group <rg> --application-logging filesystem
az webapp config appsettings set --name <app> --resource-group <rg> --settings KEY=VALUE
```

These are Azure CLI commands for managing App Service web apps:

1. **`webapp create`** — Creates a new web app, attaching it to a specified resource group and App Service Plan (which defines the compute/pricing tier).

2. **`deployment slot create`** — Creates a deployment slot named "staging" on the web app. Slots are live environments (with their own URLs) used for staging releases before they hit production.

3. **`deployment slot swap`** — Swaps the staging slot with production. This promotes your staged deployment to live with near-zero downtime, and simultaneously moves the old production to staging (making rollbacks trivial).

4. **`log config`** — Enables application logging to the filesystem. Logs are written locally on the instance and are accessible via the Kudu console or `az webapp log tail`. Note: filesystem logging resets after 12 hours on Azure automatically.

5. **`config appsettings set`** — Sets application settings (environment variables) on the web app. These are injected as environment variables at runtime and override anything in your app's config files. Sensitive values set here are encrypted at rest.

---



## Pricing Tiers
1. Shared Free & Shared Tiers
2. Dedicated Basic, Standard & Premium Tiers
3. Isolated