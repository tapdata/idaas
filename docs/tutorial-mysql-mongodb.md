# Tutorial: MySQL - MongoDB Replication

Maintainer: 

## Objective

Learn how to use iDaaS to create a general data replication pipeline to sync data from source to target. The source & target can be any type of supported databases. In this tutorial we will use MySQL & MongoDB. 
 
## Prerequisites:

- iDaaS has been installed successfully(Refer to [Installation](installation.md) if you haven't done so)

- A MySQL database with binlog(row mode) enabled. Alternatively, you may use our sandbox mysql		
	* host: demodb.tapdata.net
	* port: 3306	
	* username: demo
	* password: tapdata
	* database: insurance
	 
- A MongoDB database, alternatively you may use our sandbox mongo

	* host: demodb.tapdata.net
	* port: 27000
	* username: demo
	* password:  tapdata
	* database:  insurance

- A database GUI tool such as Navicat or DataGrip, you need it to inspect the data as well as to simulate writes into the source database. 

## Step by Step Tutorial using Tapdata Shell

### 1. Launch Tapdata Shell and Connects to local iDaaS installation

	# tapdata -i	--local	
	Tapdata iDaaS version 2.0.39
	> 
	
- -i: Enter interactive mode(Shell)
- --local: connects to local Tapdata iDaaS, this will skip authentication

### 2. Create Source & Target Connections

	> createConnection( {
				alias: "mysql_demo",
				ip: 'demodb.tapdata.net',
				port: 3306,
				user: "demo",
				password: demo123,
				db: "insurance" 			
			});
  	> mysql_demo.validate();   # alias-ed connection will be automatically injected to the current scope
  	OK
  	
  	> createConnection( {
				alias: "mongodb_demo",
				ip: 'demodb.tapdata.net,
				port: 27000,
				user: demo
				password: demo123			
				db: "insurance"				
			}); 
       
	> mongodb_demo.validate();
	OK 
	
### 3.	Create pipeline & Start running
	
	> var pipeline = createPipeline("demo_flow");
	> pipeline.readFrom(mysq_demo.Customer);	# specify the source table
	> pipeline.writeTo(mongodb_demo.Customer); # specify the target table
	> pipeline.start()
	> pipeline.stats()

	Status: running
	Input Total: 2400
	Output Total: 2300
	Throughput:  500 Rows/second
	Last Input:   2022.02.01 15:00:03.203
	Last Output:  2022.02.01 15:00:03.829
	
### 4. Verifying the Pipeline 

- Connect to MySQL using Navicat
- Issue an insert or update to the Customer table, remember to commit the change.
- Connect MongoDB
- Review Customer table has the new updates within a short delay(one or two seconds)

