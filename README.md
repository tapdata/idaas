# Tapdata D8

## What is D8

An open platform for real time data integration 

## Use Cases

When to use D8?

- Heterogenous database replication
	- MySQL to MySQL
	- PostgreSQL to MongoDB
	- Oracle to MySQL
	- MySQL to Redis
	
- CDC data feed & pre-processing for Kafka / Flink / Data Warehouse / Big Data Platform

- Building materialized view



## Features 

- Any to Any heterogenous data replication 
- Specialized support for MongoDB / Elastic
- CDC support for 30+ databases and growing
- SDK for quickly add source & target Taps


## How to Use 


## Architecutre Diagram

Source Tap  --> Processor --> Processor  --> Target Tap  -> DB 

## Sequence Diagram


Batch 

Streaming Sync

Load Schema

Test Connection


## Technicality


## 1. 关于

Tapdata 实时数据服务平台的一个SDK工具。使用它你可以

- 快速发现、浏览Tapdta连接的所有数据源
- 交互方式下学习Tapdata的数据同步、处理API  
- 调试复杂的DAG流数据处理任务
- 第三方应用对接，二次开发


## 2. 安装

### 云版安装

- 待定			

	
### 企业版

- 待定

### 开发版

- Go to connector/ folder, build tapshell   
	
		mvn -Dmaven.test.skip=true package
     
- Go to jsdk/ folder

## 3. 运行

- Go into tapshell for interactive session
	
		./bin/tapshell -url http://x.x.x.x:3030/api/ -u xxx -p 		
- Run test
	
    	./bin/taptest -ts test/exmaple.js

- Run batch script

		./bin/tapshell -f config/demo.tapdata.net.yml -s projects/demo/my_script.js


## 4. DaaS Referene 

### Top Level Objects
	
	daas: 常用api 功能入口
	db:		数据库连接入口
	
	# DaaS 基本数据对象
	Flow
	Connection
	TestUtil
	
	# 待实现
	Metadata
	Interface
	DaasUtil
	Stage
	

### Top Level Functions
	
	addDB
	listDB
	removeDB
	listFlow


### daas

> daas.addDB(properties) 
	
	> daas.addDB( {
				name: "Alpha DB",
				database_alias: "oracle_alpha",
				ip:
				port:
				user:
				password:			
			});
  	  OK
	> 

> daas.listDB()

List all connected data sources/targets

> daas.removeDB(dbo)

 
>



### 4.2 Flow Management 

	// T1
####	listFlow() 
	listFlows();  // DataFlow objects or JSON? filter by permission
	

#### Data Validation
	

#### Notifications
	// T2
	td.sendNotification({});
	
#### MISC
	// T2
	td.refreshMetadata()

## Unified Data Access API 

### 初始化 (InitSDK.JS 完成）

- 初始化 ext object, 把所有的connections指向的db 放到ext下面
- 初始化daas object , 把子分类放到daas 下面 
- connection 默认要放到daas 下面，必须要有个 alias
- 是否需要初始化 所有的表

### Type of Objects

- Directory( Category )
- DB
- Table (Collection/View)

 
**list** 

List children elements

	> daas
	fdm		dir
	mdm	 	dir
	adm		dir
	ext		dir
	

	> fdm
	pim		db
	crm		db
	oms		db
	
	> fdm.oms

	ORDER				table
	ORDER_DETAIL		table 
	ADDRESS			table
	CUSTOMER_INFO		table
		

**count**

Count of the rows in table/collection
	
	> oms.ORDER.count()
	10392

	> oms.ORDER.count({CUSTOMER_ID:100})
	12
	
**findOne**

Display the first item for quick check of the table.

	> ext.oms.ORDER.findOne()
	{
		_id: xxx,
		ORDER_ID: 101,
		ORDER_DATE: '2020-02-02',
		ORDER_TITLE: 'Cabrinha Kite 12m'
		ORDER_STATUS: 'A'
	}

**find**

Find items from the table

	> fdm.oms.ORDER.find()
	> fdm.oms.ORDER.find().limit(5)
	> fdm.oms.ORDER.find().sort({ORDER_DATE:1});
	> fdm.oms.ORDER.find({ORDER_ID: 101}, {CUSTOMER_ID:1, STATUS:1, ORDER_DATE:1} );

**sql**
	
	> ext.alpha.sql("select * from customers where city='xitang'");

	

### T2 (Future Phase)



**schema** 

Displays the table schema

**stats** 

delegate to database to execute 

	> fdm.oms.ORDER.stats()
	

**diffByPK** Only applies to FDM collection or simple replicated tables

	> fdm.oms.ORDER.diff( target_table )
	-2	vs. ext.oms.ORDER
	 
**diffByFields** 

Only applies to FDM collection or simple replicated tables
This compares all fields. 

	> fdm.oms.ORDER.diffAll()
	source: 	[	]
	target:	[	]
	 
		

## Fluent DAG API

- Flow ~= DAG
- Flow 可以包含多个Pipeline
- Pipeline 如何定义


### Pipeline API

真正的API方法执行在Pipeline Stage上面。
	  
	.from(source)
	
	.to( sink )			// sink:  table, collection, file?
	
	.mapWithCache(cache, keyFn, valFn)	// cache, IMAP

	.match()			// 字段过滤
	
	.project()		// 字段处理器
	
	.mapWithJS()		// JS
		
	.merge()			// Jet API
	
	.group()
	
	.addFields()		 
	
	.limit()
	
	.skip()
	
	.lookup()
	
	.unwind()

	.filterTable()		//表过滤器

#### Example:
 
 		p = new Pipeline();
 		p.from().map().to(); 		
 		submit(p );
 		

### Flow API 

	tapdata.makeFlow()
	
	flow.newPipeline();
	flow.addPipeline(pipeline);
	flow.preview(NUMBR_OF_ROWS); // submit for preview
	flow.submit(); // submit for actual execution
	flow.reset()
	flow.setMode( INITIAL | CDC )
	flow.setSchedule({
		startTime: '2020-02-02 10:10:00",
		repeatable: {
		},
		xxxs
	})
	flow.getStats()
	flow.getStatus()
	
	
### Flow Example
	
 		p1 = new Pipeline();
 		p1.from().map().to(); 		

 		p2 = new Pipeline();
 		p2.from().map().to(); 		
 		
 		flow = makeFlow();
 		flow.addPipleline(p1,p2);
 		flow.name="my flow"
 
 		submit(flow);
 		



## Sample Scenarios

### Add Data Source/Sink



		addDB({alias:"insurance_db", database_type:"MySQL", type:"SOURCE", host:"x.x.x.x"});
		addDB({alias:"daas_db", database_type:"MongoDB", type:"TARGET", host:"x.x.x.x"});	
		

###  1. DB Clone task 
 

	var flow = tapdata.makeFlow({ name: 'demo flow' });	var pipeline = flow.newPipeline();
	pipeline.from( ext.product_db )
		.filterTable({ includes: ['PRODUCT','PROD_ATTR'] } })
		.to(fdm.product_db );		
			
	flow.execute();
	

###  2. Oracle - Mongo single table CDC 
 
	var flow = tapdata.makeFlow({ name: 'demo flow 2' });
	var pipeline = flow.newPipeline();
	pipeline.from(ext.product_db.PRODUCT )
			.map( prod -> { prod.NAME = prod.LONG_NAME; return prod })
			.to( fdm.product_db.PRODUCT1 ).to(fdm.product_db.PRODUCT2) 
	flow.submit();
	
	
###  3. Two FDM collection union
 
	var flow = tapdata.makeFlow({ name: 'demo flow 3' });
	var pipeline = flow.newPipeline();
	pipeline.from( fdm.alpha.sys_iv.CSS_MODEL, );
	pipeline.from( 
	fdm.alpha.sys_iv.CSS_MODEL.merge(fdm.shpd.sys_iv.CSS_MODEL)
		.to( mdm.pim.CSS_MODEL );	

	flow.submit();
	
### 4. With Union & Lookup

	var flow = tapdata.makeFlow({ name: 'Demo5 - Union and Lookup ' });
	var modelCache = tapdata.makeCache('modelCache', mdm.pim.CSS_MODEL);
	
	fdm.alpha.sys_iv.IV_INVNT_INDEX.addField("SRC_DB", "ALPHA")
		.mapWithCache(cache, 
						invnt->{ return invnt.MODEL_SEQ_NBR } , 
						(invnt,model) -> { invnt.MODEL_NBR = model.MODEL_NBR; return invnt} ).
		.to

	fdm.shpd.sys_iv.IV_INVNT_INDEX.addField("SRC_DB", "SHPD")
		.mapWithCache(cache, 
						invnt->{ return invnt.MODEL_SEQ_NBR } , 
						(invnt,model) -> { invnt.MODEL_NBR = model.MODEL_NBR; return invnt} );

	flow.add([ 	fdm.alpha.sys_iv.IV_INVNT_INDEX,  fdm.shpd.sys_iv.IV_INVNT_INDEX ])
		
		
	flow.submit();






## Todo
#### HIGH 
- Improve download page 
- Host SDK jar on tapdata.net website

#### MEDIUM
- a
- b



	- 每个数据库有一条并且仅有一条database元数据
	- 每个database 元数据有个alias 字段: 不超过10个字符，英文开头，可以有数字，下划线
	- 每个外部库的表自动实现  Source 类型
	- 每个中台库的表自动实现  SOurce / SInk 类型
	- 每个目标库的表自动实现 Sink类型




**list** 

List children elements

	> daas
	fdm		dir
	mdm	 	dir
	adm		dir
	ext		dir
	
	> ext
	pim		db
	crm		db
	oms		db

	> ext.oms.ORDER.count()
	102

	> fdm
	pim		db
	crm		db
	oms		db
	
	> fdm.oms

	ORDER				table
	ORDER_DETAIL		table 
	ADDRESS			table
	CUSTOMER_INFO		table
		
	>fdm.oms.ORDER.count()
	102

	> fdm.oms.ORDER.findOne()
	{
		_id: xxx,
		ORDER_ID: 101,
		ORDER_DATE: '2020-02-02',
		ORDER_TITLE: 'Cabrinha Kite 12m'
		ORDER_STATUS: 'A'
	}

**find**
Find items from the table

	> fdm.oms.ORDER.find()
	> fdm.oms.ORDER.find().limit(5)
	> fdm.oms.ORDER.find().sort({ORDER_DATE:1});
	> fdm.oms.ORDER.find({ORDER_ID: 101}, {CUSTOMER_ID:1, STATUS:1, ORDER_DATE:1} );

**sql**
	
	> ext.alpha.sql("select * from customers where city='xitang'");

## Completed
- shell password prompt (java)	


Q: What should be return value type? JSON or Java object?

