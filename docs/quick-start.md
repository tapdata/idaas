# Quick Start

Maintainer: @Berry

## Prerequisites:

Make sure iDaaS has been successfully installed. Please refer to [Installation](installation.md) for steps. 

## 1. Launch iDaaS Shell 

	# ishell  
	Tapdata iDaaS v2.0.39
	> 
	
## 2. Create Source & Target Connections

Note the database used are for demo only. Demo databases will be reset on a daily basis. 

	> createDatabase( {
				alias: "mysql_demo",				// required
				database_type: "mysql", 			// required
				database_version: "",				// optional
				host: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: "demo123",
				db: "insurance" 			
			};
  	OK
  	
  	> createDatabase( {
				alias: "mongodb_demo",
				host: "demodb.tapdata.net",
				port: 27000,
				user: demo
				password: "demo123"
				db: "insurance"				
			});        
	OK 
	
## 3.	 Create a Table to Table Replication Pipeline
	
	> p = createPipeline("demo_pipeline");
	> p.readFrom(mysq_demo.Customer).writeTo(mongodb_demo.Customer).start();	// wait for a few seconds
	> p.stats()	

	Status: running
	Input Total: 2400
	Output Total: 2300
	Throughput:  500 Rows/second
	Last Input:   2022.02.01 15:00:03.203
	Last Output:  2022.02.01 15:00:03.829
	
## 4. Create an iModel/Materialized View backed by two tables

We have a Customer table & Policy table in Insurance DB. 

#### Policy Table
| POLICY_ID         	| INCEPTION_DATE | POLICY_HOLDER\_ID|
|----|---|------|
|  9999   | 2022-01-02 | 123 | 

#### Customer Table
| CUSTOMER_ID     | NAME | PHONE |ADDRESS|
|----|---|------| -----|
|  123   | TJ |  555-1234| 1 Main St |

We would like to create a new Policy table that  combines the columns from Policy & Customer tables so it looks like this:

#### PolicyModel Table
| POLICY_ID         	| INCEPTION_DATE | POLICY_HOLDER\_ID| POLICY_HOLDER\_NAME| POLICY_HOLDER\_PHONE|
|----|---|------| ---- |----|
|  9999   | 2022-01-02 | 123 |  TJ | 555-1234|

Here's how we would do it:

	// ADM_DB is the database alias we want to create the model in 
	> pm = createIModel("PolicyModel", "ADM_DB");   
	> pm.readFrom(mysq_demo.Policy)
	> pm.readFrom(
			mysql_demo.Customer		// source table
			.project({ADDRESS:0})   		// Exclude ADDRESS column
			.rename("NAME","POLICY_HOLDER_NAME")
		)
	> pm.