## Network Security 

### Defence in Depth

Physical Security - controlling physical access to your facilities.

Identity and access - protects services and data by ensuring that only authenticated users can access resources and that authorisation ensures users can access only those resources allowed to them.

Network Perimeter - first point at which bad actors can potentially gain access to servers and other resources. 

Network - limiting traffic only as necessary, ensuring highly secure connection between your on-premises networks and Azure.

Computer - ensure all systems are patched against all potential threats.

Application - third-party applications should be kept patched against vulnerabilities.

Data - This layer applies protection to your data in multiple locations, including databases and storage.

### Azure Firewall

Firewall - a device or service that inspects network traffic flowing through it and applies actions to that traffic based on rules that you specify.

Azure Firewall - managed firewall service.
	Stateful firewall in that it inspects sessions of network traffic and can act based on the context and state of the packets.
	Stateless firewall inspects individual data packets and is more limited in the information it gleans and therefore the actions it can take.

Azure Firewall supports three types of rule collections:
	NAT rules - enable traffic to be forwarded between network segments, such as from the Internet to Azure resources.
	Network rules - these rules allow or deny traffic based on protocol type, inbound or outbound address, and inbound or outbound port.
	Application rules - these rules allow specific applicaitons to communicate across the firewall and control traffic by FQDN

### Web Application Firewall

Azure Web Application Firewall (WAF) - is a firewall service that you can deploy with (CDN, Azure Application Gateway, and Azure Front Door) to provide firewall services specifically for your web applications.

### National Security Groups

Network Security Groups (NSGs) - additional firewall service offered in Azure.
	Enables you to filter network traffic between Azure resources.
	Can be scoped to a subnet or to a network interface.
	NSG can apply to multiple VMs or subnets.

### Application Security Groups

Application Security Group (ASG) - enables you to group servers based on the applications running on them and then manage security for them as a group.

ASGs simplify how you apply NSGs to virtual machines.

### User-Defined Routes

When creating routes, Azure creates default routes to determine how resources in the subnet will communicate with resources in other subnets.

User-Defined Routes - enables you to define a custom route to override the default route.
### Azure DDoS Protection

Distributed Denial-of-Service attacks (DDoS) - attacks overwhelm a service by flooding the service with requests.

Types of DDoS:
	Volumetric attacks - these attacks flood an endpoint with a very large volume of traffic to overwhelm the service and/or consume all of the network bandwidth available within the network or between the endpoint and the rest of the Internet.
	Protocol attacks - target specific server resources through weaknesses in the protocol. 
	Resource layer attacks - target the application layer of the protocol stack to affect web application traffic between hosts.

## Authentication and Authorisation


### Azure Active Directory

Azure Active Directory (Azure AD) - is Microsoft's cloud-based identity and access management service.
	Allows users to log into cloud services such as Microsoft 365 and access resources in Azure.


Azure AD plans:
	Azure Active Directory Free - provides management of users and groups, in synchronisation with on-premises Active Directory, basic reporting, self-service password change for accounts in Azure AD, and single sign-on (SSO) for Azure, Microsoft 365, Dynamics 365, and other applications hosted in the cloud.
	Azure Active Directory Premium P1 - all free features along with the capability to access on-premises resources as well as cloud resources, support for dynamic groups, self-service group management, Microsoft Identity Manager, and cloud write-back to allow self-service password changes for on-premises users.
	Azure Active Directory Premium P2 - includes Free and P1 features along with Azure Active Directory Identity Protection for conditional access to apps and critical data, and Privileged Identity management to discover, monitor, and restrict administrative access to resources.
	Pay-as-you-go feature licenses - add other features to a pay-as-you-go tenant.


Azure AD Support Role-Based Access Control (RBAC) - to manage access to cloud resources.

RBAC:
	Security Principal - represents a user, group, service principal, or managed identity.
	Role - a collection of permissions that determine the actions that the role can perform, such as read, write, and delete.
	Scope - set of resources to which the access applies. Can be specified at the management group, subscription, resource group, or resource level.
### Authentication and Authorisation

Authentication - identifies a user.
Authorisation - determines the actions that an authenticated user can perform.
Authorisation happens after authentication.
### Azure Multi-factor Authentication

Azure Multi-factor Authentication (MFA) - is mechanism that uses more than one factor to authenticate you.

### Conditional Access

Conditional Access - enables you to use various identity signals to allow or deny access to Azure resources in addition to authentication and RBAC.

### Single Sign-On (SSO)

Single Sign-On (SSO) - enables you to use a single set of credentials to access multiple resources.

Azure AD Connect is the primary tool to connect AD and SSO.
Azure AD Connect - synchronises changes between AD and Azure AD, providing seamless authentication and access experience for the user.

## Security Tools and Features

### Azure Security Centre

Azure Security Centre - is a monitoring service that provides a framework for advanced threat protection of your IT workloads.

Security Centre is offered at two service levels:
	Free - limited to assessments and recommendations only for Azure resources.
	Standard - provides a broad range of features for continuous monitoring and threat detection.
	
Can perform just-in-time (JIT) access control for ports.
JIT closes ports until they are needed, and keeping them open only as needed.

Secure Score - provides an indication of the overall security posture across your environment, presented as a percentage.
### Azure Key Vault

Azure Key Vault - enables you to securely store secrets such as tokens, passwords, certificates, cryptographic keys, and API keys.

### Azure Information Protection

Azure Information Protection (AIP) - enables you to classify and protect documents and emails by applying labels to them.

Can be created and managed by Administrators for use by users, created by users, or created by users based on recommendations.
### Azure Advanced Threat Protection

Azure Advanced Threat Protection (ATP) - leverages your on-premises Active Directory to detect and identify threats directed at your organisation.

Reconnaissance Attacks - attackers scan the network to locate assets and services they can compromise.

Compromised Credentials - attackers attempt to gain access with compromised credentials, such as brute-force attack.

Lateral Account Movement - attackers steal user data on one computer in order to gain access to other computers.

Domain Dominance - attackers compromise the domain through activities such as remote code execution on the domain controller.

### Azure Sentinel

Security Information and Event Management (SIEM) system.

Azure Sentinel is a Microsoft Azure based SIEM.
Azure Sentinel collects data across your enterprise from users, device, applications, and infrastructure.
Uses analytics to correlate alerts from across the environment into incidents.

### Azure Dedicated Hosts

Azure Dedicated Host - is a resource mapped to a physical server in Azure that you provision in an Azure region.

## Azure Governance Methodologies

Governance - describes policies and methods that control how a service is used.

Azure policies - define business rules that you can use to assess and ensure compliance with organisational standards in Azure.

Azure policy alias enables you to restrict the values and conditions permitted for a property of a given resource. Created as JSON files.

You do not apply permissions with Azure policies. Instead, you specify what actions people can take within a particular management scope.

Azure initiative - is a group (collection) of Azure policies.
Initiatives can contain only policies in a single subscription.


### Role-Based Access Control

RBAC is a primary authorisation mechanism in Azure.

To apply RBAC, you first create a role assignment, which consists of three elements that effectively translate to who, what, and where:
	Security Principal - specifies the individual user, group, or managed identity to which the role assignment will apply.
	Role Definition - a collection of permissions that specifies the operations that can be performed, such as read, write, and delete.
	Scope - specifies the resources to which the role assignment applies.

Classic Subscription roles:
	Account Administrator - billing owner. Can manage billing in the Azure portal, manage all subscriptions in an account, change the billing for a subscription, and change the Service Administrator.
	Service Administrator - manages the services in the Azure subscription and can cancel the subscription and assign users to the Co-Administrator role.
	Co-Administrator - role has the same privileges as the Service Administrator but cannot change the association of Subscriptions to Azure directories.

Virtual Machine Administrator Login role can view VMs in the Azure portal and log in as an administrator.


Four main roles:
	Owner - has full access to all resources and can delegate access to others.
	Contributor - can create and manage all types of Azure resources and create new tenants in Azure AD but cannot grant access to others.
	Reader - can view (consume) Azure resources, applies to all resource types.
	User Access Administrator - this role can manage access to Azure resources.

RBAC supports four levels of management scope:
	Management group
	Subscription
	Resource Group
	Resource

### Resource Locks

ReadOnly lock - enables authorised administrators to read a resource but not delete or update.

CanNotDelete lock - allows authorised administrators to read and modify a resource but not delete it.

Cannot delete a resource without firstly removing the lock.
### Azure Blueprints

Azure Blueprint - lets you define a repeatable group of Azure resources and associated role assignments and policies.
	Used for quick and easy deployment.
	
Blueprints use ARM templates.
Azure Resource Manager (ARM) templates let you easily deploy resources, but keep no connection to the resources they deploy.
Blueprints do maintain the connection.

Draft - unpublished version of a blueprint.
Published - blueprint available for assignment.
Assigning - deploys blueprint artefacts.

Publishing a new version of a blueprint does not affect existing assignments.

Published version of a blueprint cannot be altered.
Modification of Blueprints require redeployment.

None of the resources defined in a blueprint are deleted when you unassign a blueprint version and delete that version.

### Microsoft Cloud Adoption Framework for Azure

Cloud Adoption Framework for Azure - is a large collection of resources, including documentation, deployment guidance, templates, best practice documentation, and various tools to help with planning, deploying, and assessing your Azure deployment.

## Azure Monitoring and Reporting Options

### Azure Monitor

Application Insights - enables developers to integrate monitoring for live applications by sending telemetry data to Azure.

Azure Monitor for VMs - provides monitoring for Windows and Linux VMs in Azure, on-premises, and in other cloud environments.

Azure Monitor for Containers - provides monitoring for container workloads deployed to Azure Container Instances, Azure Kubernetes Services, and other container instances both in Azure and on-premises.

Log Analytics - provides the capability to write log queries and analyse query results.

Smart Alerts - solution automatically groups alerts using machine learning.

Automated Actions - Create actions that execute automatically in response to specific alerts, such as suppressing informational alerts during a planning maintenance window.

Dashboards - Create and share dashboards to visualise the results of log queries.

Workbooks - Create composite reports from multiple data sources to provide insights into performance, availability, resource usage, and more in an interactive report.

Monitoring begins automatically as soon as you add a resource to a subscription.

Metrics and logs are created for you automatically.

Application Insights enables developers to send telemetry data about the applications they develop to Azure.

### Azure Health Service

Azure Service Health - keeps you informed regarding planned maintenance and changes.

Azure Status - provides information on Azure services globally to help you see at a glance what services are affected and in what regions.

Service Health - tracks the state of your Azure services by region and gives you access to information about service issues, planned maintenance, health advisories, and security advisories.

Resource Health - tracks the state of the resources you have deployed to Azure.

Up to 2,000 action groups can be created in a subscription.
### Azure Advisor

Azure Advisor - provides a web-based report intended to help you optimise your Azure environment.
	Captures performance criteria, cost-effectiveness, reliability, security and operational excellence.


## Compliance and Data Protection Standards

### Industry Compliance Standards and Terms

Health Insurance Portability and Accountability Act (HIPAA) - US federal law that regulates protected health information (PHI) with a goal of protecting privacy surrounding individuals' health care.

International Organisation for Standards (ISO) - ISO is a standards-based, non-regulatory organisation located in the United States.

International Electrotechnical Commission (IEC) - is a non-profit, non-regulatory standards organisation based in Geneva, Switzerland.

National Institute of Standards and Technology (NIST) - is a non-regulatory agency of the US Department of Commerce.

General Data Protection Regulation (GDPR) -  defines data protection and privacy requirements as a regulation in European Union (EU) law.

Data Protection Addendum (DPA) - defines the terms for legal compliance, disclosure or processed data, data security practices and policies, data encryption, data access, and audit compliance.

Azure Government - is a separate instance of Azure with data centres only in the United States to support US federal agencies, state and local governments.

ExpressRoute can be used to establish a secure connection between Azure China and your on-premises data centre or private cloud located within China.


