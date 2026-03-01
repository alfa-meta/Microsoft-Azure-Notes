Azure App Service is a poor fit in these situations:

**You need low-level OS control.** App Service abstracts the underlying infrastructure. If you need custom kernel modules, specific OS configurations, or privileged system access, use a VM or AKS instead.

**Long-running background processes or heavy compute.** It's not designed for CPU-intensive workloads, HPC, or jobs running for hours. Use Azure Batch, VMs, or Container Instances.

**Very high traffic with unpredictable spikes requiring fine-grained scaling.** The scaling is relatively coarse. Kubernetes (AKS) gives you far more control over scaling behaviour and resource allocation.

**Microservices at scale.** App Service can host individual services, but it gets expensive and unwieldy managing dozens of them. AKS or Container Apps are better suited.

**Non-HTTP workloads.** App Service is built around HTTP/S. If you're running TCP servers, UDP services, or message queue consumers as a primary workload, it's the wrong tool.

**Strict latency or cold start requirements on the free/shared tiers.** Cold starts are real, particularly if you're on lower tiers or using slots. For latency-sensitive apps, you need to pay for always-on or look elsewhere.

**Cost at scale.** Once you're running many instances continuously, App Service Plans become expensive compared to containerised workloads on AKS where bin-packing improves density.

**Windows-only dependencies that don't fit the sandbox.** App Service runs in a sandboxed environment with restrictions (e.g. no GDI+ in some contexts, limited registry access). Some legacy Windows apps simply won't run correctly.

The short version: App Service is excellent for straightforward web apps and APIs where you want managed infrastructure without fuss. The moment you need serious control, scale efficiency, or non-web protocols, something else will serve you better.