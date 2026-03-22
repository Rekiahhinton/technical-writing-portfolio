---
title: Information Extraction Prompting
---

## Information Extraction Prompting

### Introduction 

Information extraction is a critical skill for technical writers working in active development environments. When a developer updates a parameter in a source code spec, I can use AI-assisted extraction to quickly identify and isolate the change — maintaining uniformity across documentation without interrupting the development workflow.

In this example, I demonstrate how to extract structured, meaningful information from a complex document for use in a documentation project.

### Resources

This example uses the [Appwrite Web SDK ReadME](sample-documents/README_Appwrite) document from [Appwrite](https://github.com/appwrite), an open-source cloud platform. 

### Workflow

The first step is to assign role and establish context before requesting the first extraction: 

> User: You are my technical assistant tasked with extracting information from the attached document, [README_Appwrite.md]. Identify the necessary data by extracting the Entities, the building blocks of the Appwrite tool.
>
> Assistant: From the __README_Appwrite.md__, the core __Entities (building blocks) of the Appwrite Web SDK__ can be extracted by identifying the primary objects, services, models, and configuration components referenced in the setup and usage examples.
>
> Below is a structured extraction of those entities.
>
> 1. Client
> 
> __Purpose__:
> The main configuration object that initializes communication with the Appwrite server.
>
> __Responsibilities__
>
> * Connects the frontend application to the Appwrite backend.
> * Stores configuration for endpoint and project.
> * Acts as the base dependency for all other services.
> 
> Key Properties / Methods
> 
> * ```setEndpoint(endpointURL)```
> * ```setProject(projectID)```
> 
> __Example__
> 
> ```const client = new Client();
>
> client
> 
> .setEndpoint('http://localhost/v1')
> 
> .setProject('455x34dfkj');
>
 
> __Extracted Entity Attributes__
>
> * Endpoint URL
> * Project ID
> * SDK initialization state
>

The second step is to compile the information from the __Client__ entity into a configuration table.

> User: Draft a configuration table for the Client entity. List the parameter name, whether it's required, and a brief description of what it does. 
>
> Assistant: Based on the initialization section of the SDK, the __Client__ entity is configured primarily through chained setter methods. The README shows two required configuration parameters used to establish communication with the Appwrite server.
>
> __Client Configuration Table__
>
> | Parameter | Required | Description |
> | ----------| ---------|-------------|
> | ```endpoint```| Yes | The base URL of the Appwrite server API that the SDK will communicate with. This tells the client where to send all backend requests. |
> | ```project``` | Yes | The unique identifier for the Appwrite project. It associates all requests from the client with the correct project configuration on the Appwrite server. |

The third step is to request a quick integration sheet that a developer could quickly reference for understanding the __Client__ entity.
>
> Great! Now create a quick integration sheet based only on the Client entity configuration table. A developer should be able read it to understand the Client entity quickly.
>
>__Appwrite Web SDK — Client Entity Quick Integration Sheet__
>
> __Purpose__
>
> The __Client__ entity is the foundational configuration object in the Appwrite Web SDK.
It establishes the connection between your frontend application and the Appwrite backend API. All Appwrite services (such as authentication or databases) rely on this initialized client to send requests.
> 
> __Client Configuration__
>
> | Parameter | Required | Description |
> | ----------| ---------|-------------|
> | ```endpoint```| Yes | The base URL of the Appwrite server API that the SDK will communicate with. This tells the client where to send all backend requests. |
> | ```project``` | Yes | The unique identifier for the Appwrite project. It associates all requests from the client with the correct project configuration on the Appwrite server. |
>
> __Initialization Flow__
>
> 1. Import the SDK components.
> 2. Create a __Client__ instance.
> 3. Configure the __endpoint__ and __project ID__.
> 4. Pass the configured client into Appwrite services.
>
> __Minimal Example__
>
> ```import { Client } from "appwrite";
>
> const client = new Client();
>
> client
>   .setEndpoint("http://localhost/v1") 
>   .setProject("455x34dfkj");
>

> Once configured, the client can be used to initialize services:

>
>
> ```import { Account } from "appwrite";
>    const account = new Account(client);
>

> __Key Takeaways__
>
> - The __Client__ is the root object of the SDK.
> - It stores the __API endpoint__ and __project context__.
> - Every Appwrite service must receive the configured client.
> - Without initializing the client, __no backend calls can be made__.
>
> In one sentence:
> 
> The Client entity is the connection layer that authenticates your app with an Appwrite project and routes all SDK requests to the correct backend API.
>
The following table summarizes each stage of the information extraction process: 
>
> 
> | Original State | Extraction Process | Resulting Value |
> | -------------- | ------------------ | --------------- |
> | __Monolithic README__: The ```Client``` configuration was embedded within general narrative text and mixed with various installation paths. | __Entity Isolation__: Identified and isolated the ```Client``` as the core object. Extracted only the mandatory methods ```(setEndpoint, setProject)``` and their required string arguments. | __Zero-Friction Setup__: Created a "Single Source of Truth" for the Client entity, allowing developers to configure the SDK core in seconds without parsing secondary service logic. |


### Validation

The final step is to validate the information extraction. The validation occurs by comparing the original source document to the specific elements of the output and checking for relevancy. This process ensures the model extracts the relevant information according to the prompt without including bias that would distort the output.

#### Results

The validation is accurate because the output included the relevant elements of the original document according to the initial prompt. These elements are highly relevant because they provide a logical overview of the **Client** entity and how it is necessary to make backend calls. 

### Efficiency Gains

Reduced information extraction time from 52 minutes to 21 minutes using this methodology.
