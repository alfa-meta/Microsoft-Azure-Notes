
Azure DevOps provides agents to do CI/CD work for you.

Agent - a dedicated machine that helps to perform builds or deployments.

Agent pool section in pipelines:
	1. Azure Pipelines - Microsoft-hosted agent pool containing machines with all platforms, Windows, Linux, and MacOS with many software tools installed in them.
	2. Private/self-hosted Agent Pools - a default private pool is available and you can create more private agent pools as per your requirements.

Hosted Agents:
- Public Hosted agents: allow for ten parallel executions on hosted pipelines at a time.
- Private Hosted agents: allow for one execution on hosted pipelines at a time.


### Classic vs Non-Classic Pipelines

Classic pipelines are built via GUI
Non-Classic pipelines are defined as Infrastructure as Code (IaC) via YAML or Bicep.

### Agent Pool Permissions

Reader - can only view the agent pools
User - can view and use pools but cannot manage or create agent pools
Administrator - can administer, manage, view, and use agent pools

![[Pasted image 20251020133902.png]]

A single Agent pool can be applied with individual permissions set similar to the one above


### Deployment Groups

Deployment Group - is a set of machines set up with agents.
	Each group is a dedicated deployment environment.

### Build Pipelines

Azure DevOps build pipelines can be used to build your source code to identify issues with the code early by using a continuous integration option.

You can build, test, and create deployable packages of your code using Azure DevOps build pipelines.

Classic pipelines use GUI.
YAML Ain't Markup Language (YAML) - used to write declarative scripts to define the build pipelines as a code.

In Build Pipelines, there are agent phases that allow you to group agents\` tasks under each phase.

Agent Phase - it is connected with the agent in the agent pool and uses the agent pool agent to execute the tasks.

Agentless phase - it doesn't have the capability to connect with an agent in an agent pool.
	All the tasks under the phase will execute in Azure DevOps server itself.

Main purpose of the build pipeline is building the code, testing it, and generating an output package.

Trigger - pipeline feature that allows us to decide when to start a build.
	Can be used to decide when a new build should start.
	Define what branch starts a build.
	We can control the triggers using folder paths.
	Build on Commit.

### Release Pipelines

Main purpose is to deploy the deployable packages created to the target hosting platform.

Artifact - a starting point of a release pipeline.

In the stages of the deployment pipeline, you can define pre-deployment and post-deployment conditions that allow you to control the deployment.

#### Pre-Deployment

Pre-Deployment has three main conditions:
- Manual Trigger.
- Start deployment after creating a new release.
- Trigger the deployment of the given stage if the deployment of the previous stages of pipeline have succeeded.

Pre- and Post- deployment approval lets you control deployment flow based on manual approvals.

Gates - allow you to set various conditions based on Azure functions, REST API, work items queries, and several other gates.
	Also, schedule deployments can be controlled with pre-deployment conditions.

If automated tests don't test a reasonable coverage of code, manual testing must take place before deployment.

### Task Groups

Task group - facilitates implementable, reusable steps a single block in multiple pipelines.

### Library

Library - can be used to keep variable values of pipelines as variable groups and to store files as secure files.

Azure DevOps Library allows you to keep secured files like certificates and keys that can be used in pipelines.
### Service Connection

Azure DevOps Service Connection - allows for external resources such as platform services like Azure, source control providers or NuGet feeds to connect to our pipelines.

Connecting to GitHub is done via Service Connection.

Two types of user permissions for a service connection:
- User - Can use the service connection but can't administrate it.
- Administrator - Can create, administer, and use the service connection.
### Environments

Azure DevOps deployment/release pipelines can be used to do various kinds of deployments. 
- We mostly do web app deployments
- DB deployments
- AKS Deployments
- Function app deployments.

Azure DevOps Environment represents a collection of resources that can be targeted by deployment pipelines.

Environments allow you to track the deployment and pipeline history with deployment resource details.

Creator - Can administer, create, and manage the environment.
Reader - can see the environments
User - can create environments
### Parallel Pipelines and Billing

Microsoft Hosted free tier: 1,800 Minutes a month.
Boards and Repo are free for up to five users.
2GB or Artifact storage for free.

