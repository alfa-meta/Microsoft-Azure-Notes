
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

Data centre - individual data centre within a region host the serves and other infrastructure needed to host services within that region.

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

Azure Resource Manger (ARM) - is the service that enables you to manage resources, serving as the deployment and management service for Azure.
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

### Azure Billing Scope


## Core Azure Services

## Core Azure Storage

## Core Data Services