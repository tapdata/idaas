# Fundamentals

@TJ

## Incremental Model

In DaaS architecture, data in centralized store are typically replicated from sources. Hence  we call a data model(or table) in DaaS data store "Incremental Model" or iModel as they're incrementally updated by the change events occurred in the source systems. 

iModels are first class citizens and the majority of the functionalities and activities are around iModels. 


	![image](https://user-images.githubusercontent.com/1950232/151934068-2527c288-69b2-494b-b58c-ebea631d6326.png)


## Incremental Database

This is similar to iModel except that you may create a database in DaaS that is replicated from a source database. Each iModel in the iDdatabase will have a corresponding table in the source DB. 


## Data API

To serve the data to the downstream consumer, iDaaS provides automatica data APIs on top of iModels. 

- RESTful API
- GraphQL

## Incremental Event
Change events from all managed sources, are captured by iEngine and cached/stored in iEvent Store in DaaS. 




## Others
- Data API / Model backed RESTful/GraphQL or Streaming API
- Job
- Pipeline
- Connection
- Table
- CDC vs. ETL


