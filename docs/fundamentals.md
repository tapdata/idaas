# Architecture & Fundamentals

Maintainer: @TJ

## iDaaS Components Overview

The iDaaS Data Platform consists following components:

#### iDaaS Core Components:

- Incremental Engine: The core component handles data capture, process and output tasks. 
- Storage Engine: An abstraction layer for DaaS data store, including centralized data store and event store(log)
- Service Engine: responsible for publish & serve Data API
- Management: platform admin, including metadata, security, observability, etc. 

#### iDaaS Client Components
- Shell: Interactive shell to interact with iDaaS platform
- Client SDK: Language SDKs to manage/interact with iDaaS Platform from programming language, currently Python, JS, Java is planned. 
- PDK: Plugin Development Kit for custom sources, targets & processors


![image](https://user-images.githubusercontent.com/1950232/152469064-15d14ee8-5e97-42a2-bda7-417a244903e2.png)

## Incremental Engine

Incremental Engine is the core power house in iDaaS platform.  Its responsibilities include:

- Full load of data from sources
- CDC replication from any source to any target
- On-the-fly streaming processing, including filtering, merging, joining, cache lookup, renaming, restructuring etc. 
- Perodically & Incrementally verify the data correctness between source & target


## iDaaS Data Architecutre

The classic data flow in iDaaS is three steps:

1. Capture & Process
2. Unify & Store 
3. Publish & Serve

![image](https://user-images.githubusercontent.com/1950232/152471844-5bf93763-bfea-45e0-8b56-c914fcd59242.png)

FDM / MDM / ADM concept is originated fro  IBM's Master Data Management theory & practice. They represents the tiering/layering approach typically found in centralized data management. They're similar to data warehouses ODS/DWD/DWS/DM tiering concept.

Foundational Data Model, or FDM, is similar to data warehouse's ODS layer. In this layer we will store a nearly identical copy of the source data. With minimum processing and transformations, iDaaS will be able to ensure the correctness, consistency and promptness of the data in FDM layer. 

Master Data Model, defines the enterprises main object data model. 

Application Data Model, which can consists of Analytical & Transactional applications, defines the data models based on the downstream applications. This is usually optional step. 

All the data movement from data sources to FDM layer, from FDM to MDM, are all handled by the Incremental Engine, which provides near real time data replication & on-the-fly transformation capability. 

FDM/MDM/ADM are only logical layering, physically they're still supported by database storage. 


## Terms & Concepts

- iModel / IncrementalModel
- iDatabase / IncrementalDatabase
- Data API / Model backed RESTful/GraphQL or Streaming API
- Job
- Pipeline
- Connection
- Table

#### Incremental Model

In DaaS architecture, data in centralized store are typically replicated from sources. Hence  we call a data model(or table) in DaaS data store "Incremental Model" or iModel as they're incrementally updated by the change events occurred in the source systems. 

iModels are first class citizens and the majority of the functionalities and activities are around iModels. 


![image](https://user-images.githubusercontent.com/1950232/151934068-2527c288-69b2-494b-b58c-ebea631d6326.png)


#### DaaS DB

This is a conventaional database, however with a desgination of "DaaS DB". This is to diffentiate it from the source dbs. 

#### Event Store


#### Data AP I

To serve the data to the downstream consumer, iDaaS provides automatica data APIs on top of iModels. 

- RESTful API
- GraphQL

#### Incremental Event / Change Event
Change events from all managed sources, are captured by iEngine and cached/stored in iEvent Store in DaaS. 


### Job
### Pipeline
#### Connection
#### Table



