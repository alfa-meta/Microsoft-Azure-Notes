## Azure Compute Options: A Direct Comparison

### Azure Functions

Serverless, event-driven execution. You write a function, Azure runs it when triggered (HTTP, queue message, timer, blob upload, etc.) and you pay per execution. Cold starts are a real problem unless you're on Premium plan. The runtime manages scaling automatically down to zero.

**Use when:** You have discrete, short-lived tasks. Processing a file upload, responding to a webhook, running a scheduled job. Ideal when load is spiky and unpredictable. Terrible for long-running processes (default 5-minute timeout, max 10 minutes on Consumption plan).

---

### Azure Container Apps (ACA)

A managed platform built on top of Kubernetes (specifically KEDA, Dapr, and Envoy under the hood). You bring a container, Azure handles the orchestration. Scales to zero like Functions but supports any language/runtime. More flexible than Functions, less operational overhead than AKS.

**Use when:** You need containerised workloads without managing Kubernetes. Microservices, background workers, APIs that need more control than Functions allows. The sweet spot between Functions and AKS. Also good for event-driven workloads via KEDA scaling.

---

### AKS (Azure Kubernetes Service)

Managed Kubernetes. Azure handles the control plane, you manage worker nodes (or use node pools with auto-scaling). Full Kubernetes API, full control, full responsibility.

**Use when:** You need Kubernetes specifically — complex multi-service architectures, specific networking requirements, custom operators, GPU workloads, or your team already knows Kubernetes and needs that level of control. Do not choose this because it sounds powerful; the operational burden is significant. You own patching, node sizing, networking, RBAC, monitoring setup, etc.

---

### App Service

PaaS for web apps and APIs. Deploy code or containers to a managed web server. No scaling-to-zero (unless on Consumption tier for Functions, which is separate). Straightforward, predictable pricing via App Service Plans.

**Use when:** You're deploying a traditional web app, REST API, or background worker and don't need containerisation complexity. Great for .NET, Node, Python, Java apps. Simple deployment, built-in slots for staging/prod swaps, easy SSL and custom domains. The "just works" option for web-facing workloads.

---

## Decision Matrix

||Functions|Container Apps|AKS|App Service|
|---|---|---|---|---|
|**Operational overhead**|Very low|Low|High|Very low|
|**Scale to zero**|Yes|Yes|No (by default)|No|
|**Cold starts**|Yes|Yes|No|No|
|**Custom runtime**|Limited|Any container|Any container|Limited|
|**Long-running processes**|Poor|Good|Good|Good|
|**Kubernetes control**|No|Partial (KEDA/Dapr)|Full|No|
|**Cost model**|Per execution|Per vCPU/memory|Node VMs|App Service Plan|

---

## Honest Guidance

- **Default to App Service** for web apps and APIs unless you have a specific reason not to.
- **Default to Container Apps** if you need containers or microservices without a Kubernetes team.
- **Use Functions** only for genuinely event-driven, short-lived, discrete tasks.
- **Use AKS** only if you have a Kubernetes requirement you cannot meet elsewhere, or a dedicated platform engineering team. Most organisations that choose AKS end up regretting the complexity.