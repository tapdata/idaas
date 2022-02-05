# Installation

Maintainer: @Berry

You may choose one of the following methods to setup iDaaS

- [Install using docker](#Install locally from source)
- [Install locally from source](#Install locally from source)
- [Works with Tapdata iDaaS Cloud](#Works with Tapdata iDaaS Cloud)

## Requirements

## Install using Docker

	docker pull xxx
	docker run 

## Install from source

	git clone https://github.com/tapdata/idaas.git
	cd idaas
	mvn clean package
	# add ./bin to the execution path
	idaas start
	

## Install with Tapdata Cloud

	git clone https://github.com/tapdata/idaas.git
	
	
## Verify Installation

	# idaas -u admin@example.com -p
	> list databases
	No databases found. Please setup at least two databases. 
	
