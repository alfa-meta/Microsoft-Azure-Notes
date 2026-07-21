
Azure App Service - is a fully managed platform designed to simplify the deployment and scaling of web apps, mobile back ends, and RESTful APIs.
	It abstracts away infrastructure management, letting you focus on writing code and shipping features faster.

Windows or Linux
Supports Container deployments.


Ability to scale up/down or scale out/in.
Resources you can increase or decrease include, number of cores, RAM and SSD.


Azure App Service, you can deploy and run containerised web apps on Windows or Linux.
You can pull container images from a private Azure Container Registry or Docker Hub.
Supports multi-container apps, Windows containers and Docker Compose for orchestrating container instances.


## Continuous integration/deployment support

Azure portal provides out-of-the-box continuous integration and deployment with Azure DevOps Services, GitHub, Bitbucket, FTP or a local Git repository on your development machine.

CI/CD for containerised web apps is also supported using either Azure Container Registry or Docker Hub.

When deploying a web app, you can use a separate deployment slot instead of the default production slot when you're running in the Standard App Service pricing tier or better.
Deployment slots are live apps with their own host names.

If a Linux runtime is not supported you can run a custom container.


You can retrieve the current list by using t he following command in the Cloud Shell.

```Shell
az webapp list-runtimes --os-type linux
```

## Limitations

- App Service on Linux isn't supported on Shared pricing tier.
- Azure portal shows only features that currently work for Linux apps.
	- As features are enabled, they're activated on the portal.
- When deployed to built-in images, your code and content are allocated as a storage volume for web content, backed by Azure Storage.
	- The disk latency of this volume is higher and more variable than the latency of the container filesystem.
	- Apps that require heavy read-only access to content files might benefit from the custom container option, which places files in the container filesystem instead of on the content volume.

## App Service Environment

App Service Environment - is an Azure App Service feature that provides a fully isolated and dedicated environment for running App Service apps.
	It offers improved security at high scale.

Compute is dedicated to a single customer.



# Examine Azure App Service plans

App Service always has an App Service plan
App Service plan - defines a set of compute resources for a web app to run.
	One or more apps can be configured to run on the same computing resources (or in the same App Service Plan)

Each App Service plan defines:
- Operating System
- Region
- Number of VM instances
- Size of VM instances (based on pricing tier)
- Pricing tier:
	- Free
	- Shared
	- Basic
	- Standard
	- Premium
	- PremiumV2
	- PremiumV3
	- IsolatedV2


Categories of pricing tiers:
- Shared Compute: 
	1. Free and Shared - two base tiers, runs an app on the same Azure VM as other App Service apps.
	2. Availability of these tiers depends on the selected operating system.
	3. These tiers allocate CPU quotas to each app that runs on the shared resources and the resources can't scale out.
- Dedicated Compute:
	1. The Basic, Standard, Premium, PremiumV2, and PremiumV3 tiers run apps on dedicated Azure VMs.
	2. Only apps in the same App Service plans share the same compute resources.
	3. The higher the tier, the more VM instances are available to you for scale-out.
- Isolated:
	1. Isolated, and IsolatedV2 tiers run dedicated Azure VMs on dedicated Azure Virtual Networks.
	2. It provides network isolation on top of compute isolation to your apps. It provides the maximum scale-out capabilities.


## How does my app run and scale?

In the Free and Shared tiers, an app receives CPU minutes on a shared VM instance and can't scale out.

In other tiers, an app runs and scales as follows:
- An app runs on all the VM instances configured in the App Service plan.
- If multiple apps are in the same App Service plan, they all share the same VM instances.
- If you have multiple deployment slots for an app, all deployment slots also run on the same VM instances.
- If you enable diagnostic logs, perform backups, or run WebJobs, they also use CPU cycles and memory on these VM instances.

App Service plan is the scale unit of the App Service apps.
If the plan is configured to run five VM instances, then all apps in the plan run on all five instances.
If the plan is configured for autoscaling, then all apps in the plan are scaled out together based on the autoscale settings.

## What if my app needs more capabilities or features?

Changing the pricing tier of the plan will allow scaling up or down at any time.
Isolating compute resources might improve the app's performance if your app is in the same App Service plan with other apps.


Isolate your app into a new App Service plan when:
- The app is resource-intensive.
- You want to scale the app independently from the other apps in the existing plan.
- The app needs resources in a different geographical region.



# Deploy to App Service


## Automated Deployment



Azure App Service supports automated deployment from several source control systems as part of a continuous integration and deployment (CI/CD) pipeline.
- Azure DevOps Services -
- GitHub - Automated deployments from GitHub.
	- Any changes to your production branch on GitHub are automatically deployed for you.
- Bitbucket

## Manual Deployment

Options to push your code to Azure:
- Git - App Service web apps feature a Git URL that you can add as a remote repository
- CLI - the `az webapp up` is a feature of the `az` command-line interface that packages your app and deploys it.
	- Unlike other deployment methods, `az webapp up` can create a new App Service web app for you.
- Zip deploy
- FTP/S

## Use Deployment Slots

