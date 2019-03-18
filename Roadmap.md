# ZumPay™ Project Roadmap

This is a general roadmap for the ZumPay™ project. It is subject to change and re-ordering as the community requires and as the ZumCoin  and ZumPay™ projects evolve. As with the ZumCoin project, we believe that slow and steady is the best way to ensure success and as such are dividing the different development goals into individual phases to help guarantee our success.

## Phase 1

Phase 1 will consist of the completion of the following items:

* Public Website Launch
    * Website will contain general information about the project, this roadmap, and other pertinent project information
    * Account services will **not** be ready at this point
* All necessary ZumCoin nodes will be deployed
* The [Zum Blockchain Database Storage System (ZBDSS)](https://github.com/zumpay-io/zumpay-architecture/blob/master/Architecture.md#zum-blockchain-database-storage-system-zbdss) will be implemented
* The [Blockchain Data Collection Agent (ZBDCA)](https://github.com/zumpay-io/zumpay-architecture/blob/master/Architecture.md#zum-blockchain-data-collection-agent-zbdca) will be implemented
* The design of the Application Programming Interface (API) will be prepared

## Phase 2

Phase 2 will consist of the completion of the following items:

* v1 of the Application Programming Interface (API)
    * Access to blockchain information as provided by the  ZBDSS 
    * Ability to generate a Payment Request will be available
    * Ability to specify a POST BACK url once the system determines that the payment as been completed on chain.
* Tracking of transactions by Payment ID
* Standard libraries in a few languages will be completed to interact with the API at this point

## Phase 3

* Implementation of [ZumCoin Wallet Services (ZCWS)](https://github.com/zumpay-io/zumpay-architecture/blob/master/Architecture.md#zumcoin-wallet-services-zcws) into the platform
    * Includes the development of One Time Use (OTU) wallets
* v2 of the Application Programming Interface (API)
    * Payment Requests will be altered to permit the use of the ZCWS wallet(s) to serve as intermediary wallet(s).
    * Additional methods will be provided to allow developers to poll the status of the transaction(s) via the API
* Updated standard libraries for interacting with the API
* ZumPay™ account services including:
    * Dashboard
    * Account Maintenance
    * Application Registration
    * Account Creation

## Phase 4

* Sandbox launch
* Sample applications created to showcase the ease of integration

## Phase 5

TBD

###### (c) 2019 ZumPay™ Development Team
