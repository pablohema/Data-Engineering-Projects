# Data Engineering On Azure

## Hurricane Harvey  Tweets and Satellite Images 
<br>


# Content

- [Data Engineering On Azure](#data-engineering-on-azure)
  - [Hurricane Harvey  Tweets and Satellite Images](#hurricane-harvey--tweets-and-satellite-images)
- [Content](#content)
- [Introduction & Goals](#introduction--goals)
- [Data](#data)
- [Used Tools](#used-tools)
  - [Connect](#connect)
  - [Buffer](#buffer)
  - [Processing](#processing)
  - [Storage](#storage)
  - [Visualization](#visualization)
- [Steps along the project](#steps-along-the-project)
  - [Datasets and Local Preprocessing](#datasets-and-local-preprocessing)
    - [Introduction to Datasets and local preprocessing](#introduction-to-datasets-and-local-preprocessing)
    - [Deploying your code on Visual Studio to Docker containers](#deploying-your-code-on-visual-studio-to-docker-containers)
  - [Azure Functions and Blob Storage](#azure-functions-and-blob-storage)
    - [Develope Azure Functions via Python and VS Code](#develope-azure-functions-via-python-and-vs-code)


# Introduction & Goals

**Main goals:**
>> Engineer data streaming pipeline on Azure with a main purpose to ingest and process tweets and satellite images data from Hurricane Harvey natural disaster, and serve Power BI report.

This is a meta repository that contains documentation and links to two subfolders in this repository, each of them having a distinct purpose:

  1) [hurricane-proc-send-data](https://github.com/team-data-science/azure-data-engineering/tree/main/hurricane-proc-send-data). Pre-processing of tweets about the hurricane harvey events, combining it with satellite images of the building s with and without damage and simulating a streaming data source by building a python program that sends requests to a Azure API endpoint (#TODO fire CLI)
  
  2) [hurricane-streaming-az-funcs](https://github.com/team-data-science/azure-data-engineering/tree/main/azure-func-demo) Azure data streaming pipeline that:
        - Ingests tweets from the local source client via Azure API management having a Azure Function as backend
        - Utilizes Azure Event Hub as a message queue service
        - Azure Function that takes messages from Azure Event Hub and writes them to Azure Cosmos Database

<br>

**Tools:**

- **Local:**
  - [WSL2 with Ubuntu 20.10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
    - as operating system for local development
  
  - [Visual Studio Code](https://code.visualstudio.com/) with plugins for Azure Services
    - local development and deployment do Azure (Azure Functions, Azure Web App)

  - Python and its libraries - Pandas, Requests 
    - data processing, and sending https requests to Azure API management

  - Azure SDK's 
    - for relevant Azure Services in Streamlit App use case - [azure-cosmos](https://pypi.org/project/azure-cosmos/)

  - Power BI 
    -  visualization of data from Azure Cosmos DB

- **Azure:**

  - [Azure API Management](https://docs.microsoft.com/en-us/azure/api-management/api-management-key-concepts)

  - [Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)

  - [Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)

  - [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) - SQL Core - Document Store

  - [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) 



# Data

1) [Hurricane Harvey Tweets](https://www.kaggle.com/dan195/hurricaneharvey) from Kaggle.

Tweets containing Hurricane Harvey from the morning of 8/25/2017. I hope to keep this updated if computer problems do not persist.

*8/30 Update
This update includes the most recent tweets tagged "Tropical Storm Harvey", which spans from 8/20 to 8/30 as well as the properly merged version of dataset including Tweets from when Harvey before it was downgraded back to a tropical storm.

1) [Satellite Images of Hurricane Damage](https://www.kaggle.com/kmader/satellite-images-of-hurricane-damage) from Kaggle.

Overview
The data are satellite images from Texas after Hurricane Harvey divided into two groups (damage and no_damage). The goal is to make a model which can automatically identify if a given region is likely to contain flooding damage.

Source
Data originally taken from: https://ieee-dataport.org/open-access/detecting-damaged-buildings-post-hurricane-satellite-imagery-based-customized and can be cited with http://dx.doi.org/10.21227/sdad-1e56 and the original paper is here: https://arxiv.org/abs/1807.01688

<br>

# Used Tools
## Connect

- [Azure API Management](https://docs.microsoft.com/en-us/azure/api-management/api-management-key-concepts)
  >> API Management (APIM) is a way to create consistent and modern API gateways for existing back-end services.
  
## Buffer

- [Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-about)
  >> Azure Event Hubs is a big data streaming platform and event ingestion service. It can receive and process millions of events per second. Data sent to an event hub can be transformed and stored by using any real-time analytics provider or batching/storage adapters.

## Processing

- [Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)
  >> Azure Functions is a serverless solution that allows you to write less code, maintain less infrastructure, and save on costs. Instead of worrying about deploying and maintaining servers, the cloud infrastructure provides all the up-to-date resources needed to keep your applications running.

## Storage

- [Azure Blob Storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) 
  >> Azure Blob storage is Microsoft's object storage solution for the cloud. Blob storage is optimized for storing massive amounts of unstructured data. Unstructured data is data that doesn't adhere to a particular data model or definition, such as text or binary data.

- [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) - SQL Core - Document Store
  >> Azure Cosmos DB is a fully managed NoSQL database for modern app development. Single-digit millisecond response times, and automatic and instant scalability, guarantee speed at any scale. Business continuity is assured with SLA-backed availability and enterprise-grade security.

## Visualization

- [Power BI Desktop Report](https://powerbi.microsoft.com/en-us/desktop/)
  >> Rich, interactive reports with visual analytics.

<br>

# Steps along the project
## Datasets and Local Preprocessing
### Introduction to Datasets and local preprocessing
The two datasets described on [data](#data) since there is no apparent real relation in the data between tweets and satellite images, for this project, the relation between them will be mock and randomly assigned ids from images to tweets to create an artificial relationship.  
<br>
![Preprocessing diagram](img/img-dataset_pre.png)
<br>

The goal in this step is to create a single JSON file that contains messages, where each object in the JSON array is merged between tweets and images.  
Once it is done, a python script will send objects in JSON as messages via HTTP requests to Azure API Management API endpoint, emulating source of streaming data.  

### Deploying your code on Visual Studio to Docker containers
- Develop Azure Functions in Python
   - Includes Python3, Azure Functions SDK, Docker CLI
- Prerequisites to use this feature:
  - Install Docker and Docker Compose
  - Install Visual Studio Code
  - Install the Remote - Container extension
  - Install [Azure Functions container](https://hub.docker.com/_/microsoft-azure-functions-python)  

<br>
  
1. Open visual studio and lets add definition
2. Press F1
3. Type "Remote Containers..." and select Remote-Containers: Add Development Container Configuration Files...
4. Select Azure Functions & Python 3
5. Press F1
6. Search and select "Reopen in a Container"
7. Press F1
8. Search and select "Azure Functions: Create Function..."
9. Create new project
   1.  Language "Python"
   2.  Select virtual environment from container
   3.  Select HTTP trigger
   4.  Name it as "-HttpTrigger1"
   5.  Authorization level "Anonymous"
10. Press F5 to run and debug application
11. Use insomnia to test the function
    - For example, name - Pablo
    - If all is okay it will send back "Hello, Pablo. This HTTP triggered function executed successfully."

<br>

## Azure Functions and Blob Storage
### Develope Azure Functions via Python and VS Code

