## Azure Container Registry (ACR)

ACR is simply a **private Docker registry** hosted on Azure. It stores and manages container images (and Helm charts, OCI artefacts, etc.). That's it — it doesn't run anything. Think of it as a private Docker Hub that lives inside your Azure environment.

**Use it when:**

- You're building containerised applications and need a place to store images privately
- You want images co-located with your Azure infrastructure for faster pulls and no egress costs
- You need geo-replication of images across regions
- You want to integrate with Azure Kubernetes Service (AKS), Azure Container Apps, or App Service

---

## Azure Functions

Azure Functions is a **serverless compute service**. You write a function (a small unit of code), deploy it, and Azure handles the infrastructure. It runs in response to triggers — HTTP requests, queue messages, timers, blob uploads, etc.

**Use it when:**

- You have discrete, event-driven tasks (e.g. process a queue message, respond to a webhook)
- You want to pay only for execution time (consumption plan)
- You don't want to manage servers or containers
- Your workload is sporadic or unpredictable in scale

---

## The Key Distinction

These two services are not really alternatives to each other — they operate at different layers:

- ACR is **storage** for container images
- Azure Functions is **compute** that runs code

You could absolutely use both together: deploy a Function running in a custom container, with that container image pulled from ACR.

The more meaningful comparisons would be:

- **ACR vs Docker Hub** — where to store images
- **Azure Functions vs Azure Container Apps / AKS / App Service** — where to run code