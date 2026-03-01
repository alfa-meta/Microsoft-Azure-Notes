## Azure Container Registry vs Docker Hub

### What are they?

Both are **container image registries** — services that store, manage, and distribute Docker (and OCI-compatible) container images.

**Docker Hub** is the original, default public registry. When you run `docker pull nginx`, it pulls from Docker Hub. It's cloud-agnostic, widely used, and the de facto standard for public open-source images.

**Azure Container Registry (ACR)** is Microsoft's private registry service, hosted in Azure. It's designed to integrate tightly with Azure's ecosystem — AKS, Azure Container Apps, Azure DevOps, and so on.

---

### Key Differences

**Ownership & Hosting** — Docker Hub is run by Docker Inc. ACR is run by Microsoft inside your Azure subscription, meaning the registry lives in your own Azure resource group.

**Privacy** — Docker Hub's free tier forces public repositories. Private repos require a paid plan. ACR is private by default; nothing is public unless you explicitly make it so.

**Authentication** — Docker Hub uses Docker credentials. ACR integrates with Azure Active Directory / Entra ID, meaning you can use managed identities, service principals, and Azure RBAC — no passwords floating around.

**Geo-replication** — ACR (Premium tier) supports geo-replication, so images are mirrored across Azure regions for low-latency pulls. Docker Hub has no equivalent.

**Network security** — ACR supports private endpoints and VNet integration, so traffic never leaves your private network. Docker Hub is public internet only.

**Rate limiting** — Docker Hub heavily rate-limits unauthenticated and free-tier pulls (100 pulls/6h unauthenticated, 200/6h free). This causes real pain in CI/CD pipelines. ACR has no such limits.

**Cost** — Docker Hub is free for public images but paid for private. ACR has three tiers: Basic (~£0.14/day), Standard, and Premium, plus storage and egress costs.

---

### When to use Docker Hub

Use it when you're distributing **public, open-source images** to the world, when you're working in a **multi-cloud or cloud-agnostic** environment, or when you're just pulling well-known base images (postgres, redis, node, etc.) that already live there.

---

### When to use ACR

Use it when you're running workloads **in Azure** (AKS, Container Apps, App Service). The integration is seamless — AKS can pull from ACR using managed identity with zero credential management. Use it when you need **private images**, **network isolation**, **geo-replication**, or when Docker Hub rate limits are causing CI/CD failures.

---

### Practical Rule of Thumb

If you're shipping public software to the world → Docker Hub. If you're running a private workload in Azure → ACR, full stop. The security, RBAC integration, and lack of rate limiting alone justify it.