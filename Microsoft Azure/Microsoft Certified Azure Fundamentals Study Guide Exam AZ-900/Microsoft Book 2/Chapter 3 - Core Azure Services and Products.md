
### Azure Compute Services

Virtual Machines (VMs) - fundamental compute service, offering Infrastructure as a Service (IaaS).

Azure App Services - fully managed platform for building, deploying, and scaling web applications quickly and efficiently.
	Support many frameworks.
	Includes auto-scaling, integrated performance monitoring, and security.

Azure Functions - serverless compute option for developers to run event-driven pieces of code without explicitly provisioning or managing infrastructure.

Azure Kubernetes Service (AKS) - managed Kubernetes environments, making it easier to deploy, manage and scale containerised applications using Kubernetes on Azure.


### Azure Storage Solutions

Azure Blob Storage - designed to store massive amounts of unstructured data, such as text and binary data.
	Highly scalable and secure service.

Azure Files - service offers fully managed file shares in the cloud that are accessible via the industry-standard Server Message Block (SMB) protocol.
	Optimised for migrating legacy applications.

Azure Queues - messaging store for reliable messaging between application components, whether they run in the cloud, on the desktop.
	Durable queuing system for asynchronous tasks.

Azure Table Storage - NoSQL data store for semi-structured data. 
	Used for storing flexible datasets like user data for web applications.

### Azure Networking Capabilities

Azure Virtual Network (VNet) - enables Azure resources like VMs to securely communicate with each other.

Azure ExpressRoute - provides a private connection between an on-premise organisation infrastructure and Microsoft Azure.

Azure Content Delivery Network (CDN) - service stores cached content on edge servers in point-of-presence (POP) locations closer to end-users, to minimise latency.

### Azure Database Services

Azure SQL Database - a fully managed relational database with auto-scale.
	Allows users to migrate their SQL Server workloads to the cloud with minimal changes.

Azure Cosmos DB - globally distributed, multi-model database service that enables you to elastically and independently scale throughput and storage across any number of Azure regions.

Azure Database for MySQL - fully managed, scalable MySQL relational database with high availability and security built into the service.

### Additional Core Services

Azure Marketplace - an online store that offers applications and services either built on or designed to integrate with Azure's platform.

Azure Active Directory (AAD) - Microsoft's multi-tenant, cloud-based directory, and identity management service that combines core directory services.

Azure Resource Manager (ARM) - deployment and management service for Azure, providing a management layer that enable you to create, update, and delete resources in your Azure account.

Azure Monitor - a comprehensive solution for collecting, analysing, and acting on telemetry from your cloud and on-premises environments.



## Azure Compute Options

Virtual Machines (VMs):
	On-demand scalability.
	Variety of options - wide selection of VM sizes and images for various use cases.
	 Windows and Linux support.
	 Great for migrating existing applications to the cloud.

Azure Kubernetes Service (AKS):
	Managed Kubernetes service, reduced complexity of by offloading much of that responsibility.
	Integrated developer tools - seamlessly integrate with Azure DevOps, Visual Studio Code, and other tools to simplify the workflow.
	Microservices-ready: AKS is ideal for microservices  architectures due to its high levels of scalability and flexibility.


Azure Container Instances (ACI):
	Event-driven: Great for short-lived, event-driven applications.
	Per-second billing.
	Persistent storage: Can mount Azure file shares, providing persistent storage for stateful containers.

Azure App Service:
	Fully managed platform for building, deploying, and scaling web apps, APIs, and mobile back ends.
	Multiple Languages
	High Productivity: built-in CI/CD, staging environments, custom domain, and SSL certificates.
	Enterprise-grade security: Integrated authentication and authorisation, as well as security patching.

Azure Functions:
	Event  Triggered code
	Triggers and bindings supports a variety of triggers and bindings to easily connect to other Azure services or on-premises systems.
	Support for multiple languages
	Ideal for automating tasks and building systems composed of event-driver components.

Azure Batch:
	For large-scale parallel and high-performance computing (HPC) batch jobs, Azure Batch enables you to run large-scale parallel and high-performance computing applications efficiently in the cloud.
	Job Scheduling, automatically scales to adjust to the workload demands.
	Works with on-premises software or services running in Azure.
	Batch processing, optimal for running large-scale parallel batch compute jobs across a managed collection of virtual machines.

Azure Virtual Desktop:
	AVD is a desktop and app virtualisation service that runs on the cloud.
	Can make multi-session Windows 10, allowing multiple users to share the same Windows 10 or 11 experience.
	Optimised for Office 365.
	Secure, including Azure Firewall, Azure Security Center, Azure Sentinel, And Microsoft Defender ATP.

Spot VMs:
	Spot VMs allow you to take advantage of unused Azure compute capacity at a Significant cost savings.
	Up to 90% off the regular the regular price of VMs.
	Use Spot VMs for various workloads, including dev/test, batch processing and workloads that can handle interruptions.

Dedicated Host:
	Azure Dedicated host provides physical servers that host one or more Azure VMs for Windows and Linux.
	Control over maintenance, you decide when maintenance is applied.
	Hardware isolation, no other customers workloads run on your dedicated host.



## Azure Storage Solutions

Azure Blobs - for unstructured data such as text or binary data, including documents, images, and videos.

Azure Files - Managed file shares for cloud or on-premises deployments.

Azure Queues - for storing and retrieving large volumes of messages.

Azure Tables - a NoSQL data store for semi-structured data.

Azure Disks - Block-level storage volumes for Azure VMs.


Azure Blob Storage:
	Optimised for storing massive amounts of unstructured data.
	Grouped into containers that provide a way to organise sets of blobs similar to a directory in a file system.
		Block Blobs - ideal for text and binary data.
		Append Blobs - optimised for append operations, making them perfect for logging data from virtual machines.
		Page Blobs - designed for frequent read/write operations and are the foundation of Azure Virtual Machine Disks.



























