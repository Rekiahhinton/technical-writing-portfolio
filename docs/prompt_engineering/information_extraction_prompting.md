---
title: Information Extraction Prompting
---

## Information Extraction Prompting

### Introduction 

Information extraction is useful in technical writing, especially in fast-paced environments where documentation must be updated immediately. If a developer changes a parameter in the source code of software, the technical writer capable of AI-assisted information extraction can act quickly to maintain uniformity acrosss documentation.

In this example, I demonstrate how to extract information from a document to obtain meaningful information for use in a documentation project. 

### Resources

This example borrows the [Appwrite Web SDK ReadME](docs/sample-documents/README_Appwrite.md) document from [Appwrite](https://github.com/appwrite), an open-source cloud platform. 

### Workflow

After assigning role and establishing context, I request the first extraction: 

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
> const client = new Client();
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

Next, I compile the information from the __Client__ entity into a configuration table.

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
### Conclusion 


