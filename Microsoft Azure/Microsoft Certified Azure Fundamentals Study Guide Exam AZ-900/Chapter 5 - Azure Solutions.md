
## Internet of Things (IoT)

Internet of Things (IoT) - describes connected devices equipped with sensors that collect data and send that data to an endpoint for logging, processing, and/or other actions.

Azure IoT Hub - is an Azure-hosted service that functions as a messaging hub for bidirectional communication between your deployed IoT devices and Azure services.
	Supports multiple Protocols and SDKs.
	Highly scalable.

Azure IoT Hub supports:
	Device to cloud telemetry, file upload and data transfer
	Monitoring
	Request/reply methods for controlling devices from the cloud.


Azure IoT Central - builds on the functions provided by IoT Hub to provide visualisation, control, and management features for IoT devices.

Central provides device templates allowing to connect new devices without any coding in IoT Central.

Azure Sphere - Integrated IoT solution for the following tasks:
	Azure Sphere micro-controller units (MCUs) - hardware component built into the IoT device that processes the OS and signals from attached sensors.
	Management Software - custom Linux operating system that manages communication with security service and runs the vendor's device software.
	Azure Sphere Security Service (AS3) - handles certificate-based device authentication to Azure, ensures that the device has not been compromised, and pushes OS and other software updates to the device as needed.
## Artificial Intelligence (AI)

AI - any device that perceives its environment and takes actions that maximise its chance of successfully achieving its goals.

Deep learning - uses a system modelled on the human mind to enable the service to discover information, learn, and grow.

Machine learning - data science technique that uses data to train a data model, test the model for relative accuracy, and then apply the model to new data.

Azure Machine Learning - consists of a collection of Azure services and tools that enable you to use data to train and validate models.

Azure Machine Learning Studio - is a web portal through which developers can create no-code and code-first solutions using a selection of tools, including drag-and-drop model design.

Azure Cognitive Services (ACS) - provides machine learning models designed to interact with humans and execute cognitive functions that humans would normally do, such as recognising images.
	Language - use ACS to process natural language to determine, for example, the user's sentiment or what the user is requesting or asking.
	Speech - use ACS to convert speech into text or text into speech. Speech services can also translate from one language to another, as well as recognise and verify a speaker.
	Vision - service provides identification and recognition services for analysing images, videos, and similar visual data.
	Decision - use this service to personalise a user experience with recommendations, monitor and remove offensive content, and evaluate time-series data abnormalities.

Azure Bot Service - enables you to create and use virtual agents to interact with users by answering questions, gathering information, and potentially initiating activities through other Azure services.
## Serverless Computing

Serverless computing - enables you to run code without deploying and managing a server to host and run that code.

Azure Functions - enables you to host a single method or function that runs in response to an event such as a queued message, HTTP request, or timer event. 
	Stateless - does not store its state from execution to execution.

Azure Logic Apps - allows you to graphically create low-code and no-code complex workflows using a web-based design environment.

## DevOps

DevOps - describes process, practices, and services designed to integrate development and IT with the end goal of simplifying and streamlining development efforts while maintaining high quality.


Azure DevOps - is not a single service but rather a group of services designed to enable and support development at multiple stages in the development process.

Azure DevOps Services:
	Azure Artifacts - provides a repository for storing development artefacts such as compiled source code.
	Azure Boards - provides capabilities for managing development projects and individual items, including user stories, backlog items, tasks, features, and bugs.
	Azure Pipelines - enables you to automatically build and test code projects.
	Azure Repos - a source-code repository for publishing and collaborating on development projects.
	Azure Test Plans - provides an automated testing tool for testing code.


GitHub Actions - provides workflow automation services.

Azure DevTest Labs - automates deployments, configuration, and decommissioning of virtual machines and other Azure resources.
	Can use ARM templates to deploy nearly any type of Azure resource, enabling your development team to model application environments and quickly provision and decommission.
	No monitoring, alerting, or telemetry services.