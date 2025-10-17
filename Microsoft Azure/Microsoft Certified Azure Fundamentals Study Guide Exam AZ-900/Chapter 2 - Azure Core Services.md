## Core Azure Architectural Components

### Geographies and Regions

Azure China is physically isolated instance of Azure located wholly in China.

Within each geography are Azure regions.

You can choose where to deploy a service, but not any region pairs.

### Availability Zones
Availability Zone - is a physically separate zone within a region, each with its own power, network and cooling.
	Think of it as a data centre.

Zonal Services - resources are pinned to a specific zone.
	Example include Virtual Machines.

Zone-redundant services - Azure replicates the service automatically across zones.
	Storage is a type of zone-redundant service.


### Bringing It All Together

Geography - defines a discrete market that preserves data residency and compliance boundaries.
	Usually two or more regions.
	Can be constrained by country but usually isn't.

Region - collection of data centres that interact to provide data redundancy and service availability.

Region Pair - consists of two regions within the same geography. Updates are rolled out to regions serially, meaning only one region is updated at a time.
	Region pair is only updated after the first one has been updated successfully.

Data centre - individual data centre within a region host the servers and other infrastructure needed to host services within that region.

Availability zones - encompasses separate power, networking, and cooling, and it is intended to guard against data loss or outages caused by failures in any of those three categories.
### Resources and Resource Groups

Azure resources - are manageable items in Azure such as virtual machines, databases, virtual networks, and storage accounts.

Resource group - a logical container for one or more resources.

CanNotDelete lock can be added to resource groups.
Any resources that you later apply to the resource group will also be automatically protected from deletion.

Things that you can do with resource groups:
	Apply policies,
	Control administrative permissions,
	Assign resources to the group,
	apply CanNotDelete lock.

Things to consider when using resource groups and managing resources:
	Lifecycle - all resources in a resource group should share the same lifecycle for deployment, updates, and deletion.
	Resource assignment - a resource can exist in only one group, but you can add or remove a resource to or from the group as needed. You can also move resources from one group to another.
	Location - resources in a group can reside in various regions. However, resource groups do have an assigned location that determines where the group's metadata is stored.
	Scope - resource groups enable you to apply management scope to resources collectively. You can assign Azure policies, Azure roles, or resource locks to the group, which then apply to the resources contained within it.
	Resource interaction - resources in different groups can interact with one another.
	Deletion - when you delete a resource group, all resources in the group are deleted.
	Creation - you can use the Azure portal, PowerShell, Azure CLI, or an Azure Resource Manager template to create a resource group.
	Tags - can be used to differentiate different resource groups with labels.


### Azure Resource Manager

Azure Resource Manager (ARM) - is the service that enables you to manage resources, serving as the deployment and management service for Azure.
	Not a tool or Interface.
	Functions as the broker between management tools like Azure portal and resource providers.

## Azure Subscriptions and Billing Scope

Azure is a consumption-based service where you pay for those services that you use.
### Azure Subscriptions 

Azure Subscription - serves as logical container for you Azure resources but at a higher level.
	Resource group can only exist in one subscription.

Azure subscriptions simplify resource management, billing, and cost containment.

Can serve as Scale boundaries for Azure, imposing limits for scaling Azure resources.

Can be used to establish administrative boundaries.
### Azure Billing Accounts

Billing Account - is a mechanism that you use to pay for Azure services.

Enterprise agreement (EA) - enables you to purchase software and services from Microsoft under a (typically) multiyear agreement. 

Microsoft Customer Agreement (MCA) - consolidates monthly Azure, Azure Marketplace, and Microsoft AppSource invoices.
	In some regions, an MCA is created automatically when you subscribe to pay-as-you-go or Azure free subscriptions.
### Azure Billing Scope

Azure billing scope - is a node within a billing account that enables you to manage invoices, payments, accounts, and other Azure billing-related data.

Billing scopes for Microsoft Online Services Program:
	Billing account - this node represents a single administrator account for one or more Azure Subscriptions.
	Subscription - a subscription is a grouping of Azure resources, and the Subscription scope enables invoices to be generated for each subscription, each with its unique associated payment methods.


Azure Tenant - is a specific instance of Azure Active Directory (AAD) that contains accounts and groups.
	Tenant is a group of users.
## Core Azure Services

Virtual Machine (VM) - an emulation of computer system that provides the functionality of a physical computer.
	Reduce power and physical space.

Host - a physical computer.

Hypervisor - software manages VMs running on a host.

Virtual Machine Scale Sets - simplify the creating and managing a group of load-balanced VMs.
	Supports up to 1,000 standard VM instances, or up to 600 custom instances.
	VMs in a scale set are all created from the same OS image.
	Azure Scale sets can use either Azure Load Balancer or Azure. Application Gateway to balance traffic to the VMs in the set.

Azure Site Recovery Service - enables organisations to easily replicate virtual machines and physical servers from a primary site to a secondary site.
	You can replicate physical servers or VMs from your own data centre to Azure.

Availability Sets - feature that helps you avoid potential outages caused by hardware issues, updated, or other events.
	Availability set distributed VMs across multiple fault domains and update domains.
Fault Domain - logical grouping of hardware that shares a power source and network switch, similar to a physical rack in a data centre.

Update Domain - logical group of hardware that undergoes maintenance activities or reboot events at the same time.

According to SLA the following scenarios are available:
	99.99 percent for all virtual machines across two or more availability zones.
	99.95% all virtual machines with two or more instances deployed in the same availability set will have connectivity to at least one instance 99.95% of the time in a specified SLA period.
	99.9% Any single virtual machine using Premium SSD or Ultra Disk for all operating system and data disks will have connectivity to the instance 99.9 percent of the time in a specified SLA period.
	99.5% Using standard SSD managed disks.
	95% for any VMs using Standard HDD Managed Disks.

Azure App Service - enables you to quickly develop and deploy web applications.
	Is a Platform as a Service (PaaS)
	Supports .NET, Java, Ruby, Python among others.
	Can be run on Windows or Linux.
	Encompasses load balancing, auto-scaling, automated management and security features.

Azure Container Instances (ACI) -is the Azure service that gives you the ability to create and deploy containerised applications. 

Docker - is an open source project for automating the deployment of containers.

Container - provides a means for packaging and deploying applications virtually.

Azure Kubernetes Service (AKS) - container orchestration service that monitors container health, provides container scalability, and enables resource sharing among containers in a Kubernetes cluster.

Container Group - is a collection of containers that run on the same host machine and share the same operating system, lifecycle, local network, resources, and storage.

Windows Virtual Desktop (WVD) - is an Azure service that enables users to run a Windows client in the cloud.
	Enables non Windows devices to connect to and use a WVD session.



## Core Azure Storage

Blob Storage - is optimised to store very large amounts of unstructured data such as text and binary data.

Blob Storage Tiers:
	Hot Access - optimised for storing frequently accessed data.
	Cool Access - optimised for data that you access infrequently and for a relatively limited period of time, and that typically would move to a lower tier of storage when access is no longer likely.
	Archive Access - intended for data that you rarely access, if at all. An example is long-term storage for backups.

Hot and cold access tiers store data online.
Archive access tier stores data offline.

Cool access:
	Lower storage cost but higher access cost.

Archive Access:
	Lowest storage cost but highest access cost.

Disk Storage - Azure disks are virtualised storage presented as a disk and attached to a virtual machine, much like a physical disk in a server.

Azure offers three main disk roles:
	Data disk - persistent,
	OS disk - persistent
	Temporary disk - deleted on reboot or VM redeployment.

Disk storage offer two types of encryption:
	Server-side encryption - provides encryption-at-rest to safeguard your data and meet compliance and policy requirements. Enabled by default.
	Disk encryption - uses Bit-locker for Windows and DM-Crypt for Linux. Enables you to encrypt OS and data disks.

File Storage - files that can be accessed securely from anywhere in the world, not associated by any specific VM. 
	Can be accessed by Server Message Block (SMB) and Network File System (NFS)
	Can be accessed concurrently by on-premises as well as Azure services.

Storage Accounts - contains Azure Storage objects and provides a unique namespace through which you can access those storage objects via HTTP and HTTPS.

Types of Storage Accounts:
	General-purpose v1 - legacy account type intended for blobs, files, queues, and tables.
	General-purpose v2 - storage account type is also intended for blobs, files, queues, and tables, as well as Data Lake Gen2.
	BlockBlockStorage - intended for block blobs and append blobs in high-performance scenarios such as high storage transaction rates, or where storage consists of small objects and/or low latency.
	FileStorage - storage account type is intended for files-only storage scenarios where premium performance is required.
	BlobStorage - legacy blob-only storage account type.


## Core Data Services

Structured and Unstructured data:
	Structured Data - defined by a schema that determines the characteristics and types of data stored in a data set.
	Unstructured Data - no predefined structure, and is not organised in a predefined way.

Azure SQL Database - abstracts all infrastructure needed to host a SQL database.
	PaaS offering.
	99.99% up time.
	

SQL Managed Instance - 
	Very similar to Azure SQL DB.
	Offers auditing, authentication, backups, .
	Integrated with Azure Data Migration Service.

Cosmos DB - multi model database service that enables you to scale data out to multiple Azure regions across the world.
	Available to worldwide users.
	Supports Gremlin API. Enabling you to store and query massive graphs at any scale, including those with potentially billions of vertices and edges.

Azure Database for MySQL - gives you the capability to deploy, manage, and use MySQL databases without deploying MySQL on a server or VM.

PostgreSQL is an open source relational database management system with its origins in POSTGRES, which was a success to the Ingress database developed at the university of California, Berkeley.

Azure Database for PostgreSQL - Azure based implementation of PostgreSQL that supports the PostgreSQL database engine with the scalability, elasticity, high availability, and other cloud features you would expect from an Azure service.

Azure Database Migration Service - supports a variety of database migration scenarios for both one-time (offline) and continuous synchronisation (online) migrations.


Microsoft Marketplace - 
	Part of Microsoft AppSource and Azure Marketplace.

Microsoft AppStore offers business solutions for Azure, Dynamics 365, Microsoft 365, web apps, and Power Platform.

Azure Marketplace is an online store that enables you to find and purchase a variety of Azure solutions and managed services.