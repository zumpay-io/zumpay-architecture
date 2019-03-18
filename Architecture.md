# ZumPay™ Service Delivery Architecture

## Objective

This document provides the working standard for the infrastructure design and service delivery model of the ZumPay™ project. While this document does serve as an outline and model for how the services will be designed and ultimately provided, it is subject to change without notice and actual service delivery models may differ from this document.

Our goal; however, is to maintain concurrency between the service platform and this document for all to enjoy.

## Infrastructure

Connecting to the ZumCoin™ network requires utilizing the `ZumCoind` binary to provide core services. As this is our gateway to the network, the stability, reliability, and reachability of the node services is one of the most important things for ZumPay™ services.

To ensure [99.999%](https://en.wikipedia.org/wiki/High_availability#Percentage_calculation) or higher uptime for the services the infrastructure must be designed with high-availability in mind. To accomplish this, multiple redundant layers including [inter-process communication](https://en.wikipedia.org/wiki/Inter-process_communication) between those layers is required.

### ZumCoin™ Node Deployment

Nodes will be deployed in localized clusters. The clusters will consist of not less than 3 nodes at any given time. For the highest availability on the current ZumCoin™ software, Microsoft Windows hosts will be used.

Each cluster will consist of the nodes and a high-availability load balancer that verifies the state of the node as suitable. Each load balancer is charged with making sure that the cluster is always available.

***Note:*** ZumPay™ and the underlying wallet(s) used for the service will, at no point, directly communicate with **any** nodes.

### Zum Blockchain Database Storage System (ZBDSS)

To store the blockchain, multiple candidate database systems are being evaluated.

Core factors to consider in database system selection include [ACID](https://en.wikipedia.org/wiki/ACID_(computer_science)) properties:

* Atomicity
* Consistency
* Isolation
* Durability

Candidate systems include, but are not limited to:

* [MySQL](https://dev.mysql.com/)
* [MariaDB](https://mariadb.org/)
* [PostgreSQL](https://www.postgresql.org/)
* [SQLite](https://www.sqlite.org/index.html)
* [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/default.aspx)

***Note:*** The selection of a standard cloud provider for this portion of the service is acceptable as the information contained within their data store is **public** knowledge that exists already on the blockchain.

The highly scalable database will be used in lieu of working directly with the ZumCoin™ software for a number of reasons, including:

* Provides a [DMZ](https://en.wikipedia.org/wiki/DMZ_(computing))-style buffer between the ZumCoin™ network and the ZumPay™ services
* Provides an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer) that does not rely on the underlying node software
* Provides faster random blockchain access than the node software permits at a much larger scale

### Standard Services Database

ZumPay™ requires the same level of database control and ACID properties as the blockchain storage for managing and maintaining information such as the following:

* Developer Account Services
* Application Endpoint Information
* Transaction Information Through the Service
* Audit Trails & Logs as Required
* Debugging, Troubleshooting, & Support Information

## Core Services

### [Zum Blockchain Data Collection Agent (ZBDCA)](https://github.com/zumpay-io/zum-blockchain-data-collection-agent)

To mitigate any issues with the node software, data (blocks & transaction mempool) will be collected from the clustered regions at standard intervals.

The BDCA(s) will poll the clusters for changes in the blockchain including re-organization events, new blocks, and transaction memory pool changes. Each instance of the BDCA will exist as close to the clusters as possible to minimize latency in the polling of the node.

This information, once collected, will be pushed into the ZBDSS for further consumption by ZumPay™ services.

### [ZumCoin™ Wallet Service (ZCWS)](https://github.com/zumpay-io/zumpay-wallet)

As with any blockchain system, a wallet must be maintained. The core functionality of a wallet is as follows:

* Hold Public & Private Keys
* Scan blockchain transactions
  * Retrieve outputs meant for our keys
  * Track spendable outputs
  * Create new inputs from spendable outputs
* Generate & Sign new transactions

Our wallet(s), instead of relying on a service such as `zum-service` will live as native services that interact not with the node but with the data collected by the BDCAs. The wallet(s) will scan transactions and the transaction memory pool directly from the database.

Benefits of working with the data in the database instead of a traditional wallet include, but are not limited to:

* Provides a [DMZ](https://en.wikipedia.org/wiki/DMZ_(computing))-style buffer between the ZumCoin™ network and the ZumPay™ services
* Provides an [abstraction layer](https://en.wikipedia.org/wiki/Abstraction_layer) that does not rely on the underlying wallet software
* Fine grain atomic control of wallet operations
* Easy to scale and spin up on demand
  * Permits One Time Use (OTU) wallets

***Note:*** Development of a [Node.js](https://nodejs.org/) native wallet is underway to support this effort.

### [Zum Blockchain Relay Agent (ZBRA)](https://github.com/zumpay-io/zum-blockchain-explorer-node)

Each ZBRA is tasked with ensuring that new transactions from the ZumPay™ platform are properly relayed to the ZumCoin™ network for processing. Each transaction meant for network consumption is queued by the ZumPay™ platform before being broadcast to the ZumCoin™ network. The ZBRA is charged with verifying that the transaction has been accepted by a node and providing the state of such back to ZumPay™.

***Note:*** Multiple outgoing transfers from the ZumPay™ platform may be combined into a single network transaction to make the most efficient use of block space.

## ZumPay™ Services

### [Public Website](https://zumpay.io)

The front-end website provides general functionality similar to other payment gateway platforms such as, but not limited to:

* Automated Account Creation
* Account Maintenance
* Application Maintenance
* Security Services

### Developer Tools

#### [Application Programming Interface (API)](https://docs.zumpay.io)

API services as used by developers will be provided via standard HTTP RESTful calls to a common gateway interface (i.e. https://api.zumpay.io).

The API design will be outlined further in another document within this repository at a later date.

#### Standard Libraries

The project will provide a collection of standard utilities to interact with the ZumPay™ API. Different libraries will be made available upon request and within reason.

#### Example Applications

The project will provide a number of example applications including how to integrate ZumPay™ services into other applications or e-commerce platforms upon request and within reason.

#### Sandbox Mode

A sandbox mode for all API requests will be provided to all developers to aide in the development and testing of applications built on the ZumPay™ platform.

## Service Delivery Model

The following diagram has been created to document the design concept driving [Phase 2](https://github.com/zumpay-io/zumpay-architecture/blob/master/Roadmap.md#phase-2).

![ZumPay™ v1 Process Flow]()

###### (c) 2018 ZumPay™ Development Team
