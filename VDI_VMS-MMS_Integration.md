# VDI VMS – MMS Integration

- [Revision History](#revision-history)
- [Purpose of the Standard](#purpose-of-the-standard)
- [Goals and Objectives](#goals-and-objectives)
- [Overview](#overview)
- [Key Terms](#key-terms)
- [System Features](#system-features)
- [Data Interfaces](#data-interfaces)
- [Data Exchange](#data-exchange)
- [Interface File Definitions](#interface-file-definitions)
  - [Transaction Header](#transaction-header)
  - [Sale document](#sale-document)
  - [Consumer Transaction](#consumer-transaction)
  - [Product Catalog](#product-catalog)
  - [Kiosk](#kiosk)
  - [Market](#market)
  - [Consumer](#consumer)
  - [Cash Collection](#cash-collection)
- [Appendix A: XML Examples](#appendix-a-xml-examples)
  - [Sales](#sales)
  - [Consumer Transactions](#consumer-transactions)
  - [Products Catalog](#products-catalog)
  - [Kiosk](#kiosk)
  - [Markets](#markets)
  - [Collections](#collections)
  - [Consumers](#consumers)

## Revision History

|  **Version** |  **Date** |  **Notes**    |
|-------------:|----------:|---------------|
| 1.0          |  4/15/15  | Original Draft for field trials |
| 1.1          |  7/20/15  | Added Revision History section. Changed VDIXMLType for each interface to include “mms-“ prefix. |
| 1.2          | 10/22/15  | Added @ProductCode, @CategoryCode, @Category attribute to Products. @ConsumerName attribute added to Consumer Transactions and Sales. Added @MarketID and removed @KioskID from Market element of Products message type. Added details for Bills and Coins for Consumer Transaction messages. Added XML examples in Appendix A. |
| 1.3          | 12/14/15  | Sale document Type: Sales @SaleID Notes changed term *Customer* to *Operator*. Sales document Summary Notes added text *Summarizes the item details*. Sales document Type: Fee @total Notes added text *for reference only*. Sales document Type: Item @price Notes text *Net* removed from *Price*. |
| 1.4          |            | -   Corrected documentation and samples to clarify that Codes can/should be grouped under a common parent element (\#3) |

## Purpose of the Standard

Developing and adhering to an industry standard for integrating
MicroMarkets and Vending Management Systems provides benefits not only
to operators, but also the respective MMS and VMS. Some of these
benefits include:

-   Specifying the system features needed to manage a MicroMarket
    business
-   Define which system should be responsible for each feature
-   Reduce cost and time when integrating new MMS or VMS providers
-   Promotes interchangeability and compatibility between systems

## Goals and Objectives

Our objective in this document is to identify key information and system
features that are required to manage a MicroMarket business and then
decide which system should “own” this information or feature. Given the
difficulty of managing the same data in multiple systems and keeping
this data in synch, it is a goal of ours that only *one system* manages
or creates a specific set of data. It is acceptable that a set of data
be shared with the other system so that the 2^nd^ system can perform
certain tasks that are required by MicroMarket operations. It is
understood that both systems (and their vendors) could implement all
system feature listed in this document. However, we will base this
document on the fact that this single system does not exist and we need
to clearly define the *one* system that the operator is to use to
complete a specific task.

Integrating MMS and VMS will also provide a consolidated source for both
MicroMarket and vending machine management which includes:

-   Full warehouse inventory accountability
-   A single source for sales and inventory reporting
-   A single source for product and price Information
-   A single hand held for servicing both vending POS and MicroMarket
    POS
-   Prekitting using as near to real time sales as possible
-   Complete cash and inventory accountability


## Overview

Below is a list of system features required to manage a MicroMarket
business. There is a basic assumption that the entity operating the
MicroMarket business is a vending operator and this entity has already
invested in a Vending Management System to support their vending
business. This Vending Management System includes information such as
their customer master and product master and this system supports key
processes such as warehouse inventory management and inventory
replenishment at their points of sale.

In this analysis, we are considering the existence of two key systems
that will be used to support the MicroMarket business for a given
operator. They are:

1. **Vending Management System (VMS)** – the route accounting system
   typically used to support the vending business and OCS business.
2. **MicroMarket System (MMS)** – the key “system” in this solution is
   the kiosk and associated services that allows consumers to purchase
   items from the market using a self-checkout process.

## Key Terms

- **Kiosk**: The self-checkout system/hardware used for purchasing 
  products in the MicroMarket.
- **Market**: The physical room where the MicroMarket exists. The 
  market may contain one or more kiosks.
- **Operator**: The business entity that is managing the market.
- **Client**: The client account where the market operates. The
  operator “sells” the MicroMarket concept to the client and then
  installs the store to conduct business.
- **Consumer**: The individual who actually purchases the product in
  the market. With prepaid accounts playing a key role in the
  MicroMarket business, there is a good opportunity for the operator
  to understand who is buying which products.

## System Features


| Feature                       | Proposed |
|-------------------------------|----------|
| **Market Profitability**<br/>Ability to report individual market profitability based on sales, product cost, labor cost, commissions, fixed and variable expenses.   | **VMS** |
| **Client Account Profitability**<br/>Ability to report consolidated client account profitability based vending business, OCS business, MicroMarket business for each client of the operator. | **VMS** |
| **Physical Inventory of Market**<br/>Ability to capture physical counts of products in the market to determine if there are shortages based on perpetual inventory of the market. |**VMS** |
| **Perpetual Inventory of Market**<br/>Ability to track perpetual inventory for each product in the market. This is the calculated inventory levels based on the inflow and outflow of products. | **VMS** |
| **Route Inventory Management**<br/>Ability to track inventory levels on the route truck. This includes perpetual inventory by product, transfers of products to/from the truck, truck spoilage and physical product counts on the truck. | **VMS** |                                           
| **Warehouse Inventory Management**<br/>Ability to track inventory levels in the operator’s warehouse. This includes perpetual inventory by product, purchases from suppliers, transfers of products to/from the warehouse, warehouse spoilage and physical product counts in the warehouse. | **VMS**
| **Market Orders for Replenishment**<br/>Ability to determine products required to replenish the market. This should take into account the market sales and past inventory replenishments to arrive at a perpetual inventory count for each product. Perpetual is then compared to desired inventory levels and may trigger and order. | **VMS** |
| **Order Delivery/Product Delivery**<br/>Ability to record quantities of product delivered to the market for replenishment. Spoiled products removed from the market are also recorded. Perpetual product counts in the market are updated. | **VMS** |
| **Client Account Master Data**<br/> Ability to setup client master data in system to track key information about the client. This includes vending locations and machines, MicroMarket locations and OCS locations. | **VMS** |
| **Sales Tax Rates**<br/>Ability to setup sales tax rates by tax jurisdiction and apply to market catalogs, vending sales and OCS invoices. | **VMS** |
| **Market Planogram**<br/>Ability to define the products that are to be sold in a specific market, along with product price, sales tax, bottle deposit fees, and recommended inventory levels. Planograms are used in the ordering process to determine replenishment values based on current perpetual and the planogram desired/re-order quantity. | **VMS** |                                             
| **Market Sales**<br/>Ability to record sales in the market. Sales should include products and their quantities, sales taxes, tender method, time of purchase, consumer (if known). | **MMS** |
| **Prepaid Account Funding**<br/>Ability for consumer to fund their prepaid account using credit card or cash in the market. | **MMS** |
| **Tender Capture - Cash**<br/>Ability to track amount of cash/coin transacted in the market. This includes bills and coins provided by consumers during the purchase process as well as bills and coins returned to consumers. | **MMS** |
| **Tender Capture – Debit/Credit**<br/>Ability to capture debit/credit card usage (transactions) made by consumers in the market when purchasing products or funding their prepaid accounts. | **MMS** |
| **Tender Capture – Prepaid**<br/>Ability to capture prepaid account usage (transactions) made by consumers in the market when purchasing products. | **MMS** |
| **Tender Capture – Closed-Loop Payment Systems**<br/>Ability to capture closed-loop payments such as Quickcharge (payroll deduct) and Freedom Pay. | **MMS** |                              
| **Prepaid Account Management**<br/>Ability to track prepaid consumer account balances based on consumer funding and purchases made using prepaid accounts. | **MMS** |
| **Kiosk Cashout**<br/>Ability to track amount of cash/coins collected by an operator (typically a driver) when cashing out a kiosk | **MMS** |
| **Collections - Cash**<br/>Ability to record cash collections (counted) for each market “collect”. System can provide “expected” cash using the cash tender tracking feature (Tender Capture – Cash) in the market vs. the actual cash counted in the money room. | **VMS** |
| **Collections - Debit/Credit**<br/>Ability to reconcile debit/credit transactions in the market with transactions settled at the bank. Actual deposits are reconciled against credit transactions and settlement fees. | **VMS** |
| **Consumer Management**<br/>Setup and management of consumer profile information such as username/password, e-mail, phone, preferences, etc. | **MMS**
| **Promotions/Discounts/Bundling**<br/>Setup and management of promotions, discounts, bundling, etc. | **MMS** |
                                                                                               

## Data Interfaces

| Phase |   Data                                                                                                                                                                                                                                                                                                                   | Flow_________ |
|-------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------:|
| **1** | **Sales**<br/>Market sales and payment tenders. Data will be by sales ticket and will represent the “basket” purchased and the payment methods used to purchase. Tenders (payment methods) may be a combination of cash, credit, stored value, discounts, coupons. <br/><br/>_FREQUENCY: As they occur / near real-time_ | VMS ⇐ MMS |
| **1** | **Kiosk**<br/>Kiosks deployed in the field. Includes model and configuration data. This data represents the kiosk asset list that is managed by the MMS vendor. The VMS can use this list to reconcile its list of kiosks related to the vendor.<br/><br/>_FREQUENCY: As they occur_                                     | VMS ⇐ MMS |
| **1** | **Product Catalog**<br/>Product catalog for each market that is to be used to facilitate consumer self-checkout. Data includes: Market, product, barcode, price, tax, bottle fees.<br/><br/>_FREQUENCY: As they occur_                                                                                                   | VMS ⇒ MMS |                              
| **1** | **Market**<br/>Master data describing the market as defined in the VMS. May include client/account information and address of market.<br/><br/>_FREQUENCY: As they occur_                                                                                                                                                | VMS ⇒ MMS |
| **1** | **Consumer Transaction**<br/>Cash and credit transactions that occur outside of the sales ticket activity. Most of these transactions are fundings of prepaid accounts – either by cash or by credit.<br/><br/>_FREQUENCY: As they occur / near real-time_                                                               | VMS ⇐ MMS |
| **1** | **Cash Collections**<br/>Values surrounding a kiosk cash out, generally initiated at the kiosk when money is removed from the kiosk.<br/><br/>_FREQUENCY: As they occur / near real-time_                                                                                                                                | VMS ⇐ MMS |
| **2** | **Consumer**<br/>Includes consumer names and account balances related to stored value as well as loyalty award balances. Prepaid account balances should be used to recognize their prepaid financial liability.<br/><br/>_FREQUENCY: Once Per Day_                                                                      | VMS ⇐ MMS |

## Data Exchange

To allow for near real time data exchange both integrating parties (i.e.
VMS and MMS providers) will need to develop a web service which will
accept VDITransaction messages as defined in this document. To help with
the integration of new participants it is recommended that all
implementation follow the following guidelines:

The web service should be implemented using SOAP 1.1 standard.
Authentication should be implemented by adding standard HTTP 1.0 header
as a SOAP header containing base64 encoded username and password.

The web service should expose method VDIDataExchange returning string
with the parameters specified below and corresponding to the attributes
in the VDITransaction XML.

The return value is 'SUCCESS' or error message in case of a failure.

| Name                    | Type    | Comments                           |
|-------------------------|---------|------------------------------------|
| **VDIXMLVersion**       | string  |                                    |
| **VDIXMLType**          | string  |                                    |
| **ProviderID**          | string  |                                    |
| **ApplicationID**       | string  |                                    |
| **ApplicationVersion**  | string  |                                    |
| **TransactionID**       | string  |                                    |
| **TransactionTime**     | string  |                                    |
| **OperatorID**          | string  |                                    |
| **CompressionType**     | string  | Future use. Omit if not used.      |
| **CompressionParam**    | string  | Future use. Omit if not used.      |
| **Encoding**            | string  | Examples: UTF-8, UTF-16            |
| **VDIXML**              | string  |                                    |
| **UserData**            | string  | Future use. It is not the same as UserData node in the VDITransaction |


## Interface File Definitions


### Transaction Header

Each transaction is wrapped up in a standard header containing basic
information about the exchange. This effectively is the root element of
each XML message taking part in the exchange.

**Root element: VDITransaction**

| **Field**           | **XML Type**     | **Length** | **Req’d** | **Notes** |
|---------------------|------------------|-----------:|-----------|-----------|
| @VDIXMLVersion      | Positive Integer |            | Y         | Version of VDIXML standard which was used to encode the information. For the implementation of the protocol defined in this document the value should be 1. |
| @VDIXMLType         | Token            | 32         | Y         | Data type included in the file. It should be lowercase string as defined for each of the specific data collection in this document. |
| @ProviderID         | Token            | 64         | Y         | ID of the provider taking part in the exchange. MMS or VMS technology provider. |
| @ApplicationID      | Token            | 64         | Y         | Unique identifier of the application taking part in the MMS &lt;-&gt; VMS data exchange. |
| @ApplicationVersion | Token            | 32         | Y         | Version of the application which can be used to determine how the data should processed. Actual values and their meanings will have to be discussed on the implementation level, but on the standard level we require to have non-empty value to be able to see if version have changed or not. In contrast to the VDIXMLVersion which changes with each iteration of the standard the ApplicationVersion attribute allows for changes in the implementation between the integrating parties (e.g. how the UserData is used) within the same version of the standard. |
| @OperatorID         | Token            | 64         | Y         | ID of the top level entity owning the data in both MMS and VMS system. This value will have to allow for unique identification of a single business entity in both systems. It is the customer of MMS and VMS providers. |
| @TransactionID      | Token            | 64         | Y         | Uniquely identifies the exchange transaction. If the value is repeated with the previous message it has to be exactly the same as the previous message. |
| @TransactionTime    | DateTime         |            | Y         | The date, time and time zone when the transaction was created. |
| \*                  | XML              |            | Y         | Single ‘data collection’ XML node which contains the payload exchanged as defined in this document. Acceptable nodes are: &lt;Sales /&gt;, &lt;Kiosks /&gt; |
| UserData            | XML              |            | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |


### Sale document

Sale document represents a single sale.

**Data collection element: Sales**

**Data item element: Sale**

**VDIXMLType value: mms-sales**

#### Type: Sale

| **Field**      |**Type**  | **Length**  | **Req’d**  | **Notes**
| -------------- | -------- | -----------:| ---------- | ------------------------------------------------------------------------------------------------------------------------------- |
| @MarketID      | Token    | 64          | Y          | ID of the market in which the sale took place. |
| @KioskID       | Token    | 64          | N          | ID of the kiosk, the physical machine, on which the sale took place |
| @ConsumerID    | Token    | 64          | N          | ID of the consumer who made the purchase |
| @ConsumerName  | Token    | 128         | N          | Human friendly name of the consumer who made the purchase. |
| @SaleID        | Token    | 64          | Y          | ID of sale which needs to be unique in the scope of a single Operator. |
| @SaleTime      | DateTime |             | Y          | Date, time and time zone of the Sale. |
| Summary        | XML      |             | Y          | Summary XML Type as defined in this document. Summarizes the item details. See Summary Type. |
| Items          | XML      |             | Y          | Items collection containing Item type nodes as defined in this document. See ItemCollection Type. |
| Tenders        | XML      |             | Y          | Tenders collection containing tender type nodes as defined in this document. See TenderCollection Type |
| UserData       | XML      |             | N          | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |

#### Type: Summary

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes**
| ----------- | ---------- | ------------:| ----------- | -------------------------------------------------------------------------------------------------------------------------------
| @Price      | Decimal    |              | Y           | Total net price
| @Discount   | Decimal    |              | N           | Total discount. It should be a negative number unless it is a negative discount which increases the price
| @Total      | Decimal    |              | Y           | Total gross amount. It should be equal Price + Discount + Deposit + Fees + Taxes.
| Fees        | XML        |              | N           | Collection containing all fees. See FeeCollection.
| Taxes       | XML        |              | N           | Collection containing all taxes. See TaxCollection.
| UserData    | XML        |              | N           | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs.

Fees collection name: Fees

#### Type: FeeCollection

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes** |
| ----------- | ---------- | ------------:| ----------- | ------------------------------------------------------ |
| @Total      | Decimal    |              | Y           | Total fees. Should equal to the sum all the elements |
| Fee         | XML        |              | Y           | See Fee Type. |

Fees collection element name: Fee

#### Type: Fee

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes**
| ----------- | ---------- | ------------:| ----------- | --------------------------------------------
| @ID         | Token      |              | N           | ID representing the fee
| @Name       | Token      |              | Y           | Name of the fee
| @Count      | Number     |              | Y           | How many times the fee has been applied
| @Value      | Decimal    |              | Y           | Value of the fee
| @Total      | Decimal    |              | Y           | Equal to Value\*Count (for reference only)

Taxes collection name: Taxes

#### Type: TaxCollection

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes**
| ----------- | ---------- | ------------ | ----------- | -------------------------------------------------------
| @Total      | Decimal    |              | Y           | Total taxes. Should equal to the sum all the elements
| Tax         | XML        |              | Y           | See Tax Type

Taxes collection element name: Tax

#### Type: Tax

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes** |
| ----------- | ---------- | ------------ | ----------- | ---------------------------------------------------------- |
| @ID         | Token      |              | N           | Represents the tax |
| @Name       | Token      |              | Y           | Name of the tax |
| @Rate       | Decimal    |              | Y           | Rate of the tax. 6.5% tax should be represented as 0.065 |
| @Value      | Decimal    |              | Y           | Value of the tax |
| @Count      | Number     |              | Y           | How many times the tax has been applied. |
| @Total      | Decimal    |              | Y           | Total value of tax. |

Items collection name: Items

#### Type: ItemCollection

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes**                                  |
| ----------- | ---------- | ------------ | ----------- | ------------------------------------------ |
| Item        | XML        |              | Y           | One or more item elements. See Item Type   |

Items collection element name: Item

#### Type: Item

| **Field**  | **Type**         | **Length**   | **Req’d**   | **Notes** |
| ---------- | ---------------- | ------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------- |
| @ProductID | Token            | 32           | Y           | ID of the product sold |
| @Code      | Token            | 32           | N           | Code representing the sold product. It can be barcode or any other form of product coding. |
| @Quantity  | Positive integer |              | Y           | Number of products sold |
| @Cost      | Decimal          |              | N           | Optional cost of a single product for the purpose of profit analysis |
| @Price     | Decimal          |              | Y           | Price of a single product |
| @Discount  | Decimal          |              | N           | Total discount applied to a single product |
| @Deposit   | Decimal          |              | N           | Total deposit applied to a single product |
| @Total     | Decimal          |              | Y           | The total amount for a single product |
| Fees       | XML              |              | N           | FeeCollection type |
| Taxes      | XML              |              | N           | TaxCollection type |
| UserData   | XML              |              | N           | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |

Tenders collection name: Tenders

#### Type: TenderCollection

| **Field**   | **Type**   | **Length**   | **Req’d**   | **Notes** |
| ----------- | ---------- | ------------ | ----------- | ----------------------------- |
| Tender      | XML        |              | Y           | One or more tender elements |

Tenders collection element name: Tender

#### Type: Tender

| **Field**    | **Type** | **Length** | **Req’d** | **Notes**
| ------------ | -------- | ----------:| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------
| @Type        | Token    | 32         | Y         | Type of the tender. Valid values:<ul><li>CARD (debit or credit card)</li><li>CASH</li><li>EXTERNAL</li><li>ACCOUNT (prepaid)</li>
| @Description | Token    | 64         | N         | Additional description of the tender type. It can specify the card type or payroll deduct system name.
| @Amount      | Decimal  |            | Y         | Value of the tender.
| @Reference   | Token    | 128        | N         | Optional reference number identifying the tender on the processor side which can be used for payment reconciliation.
| Bills        | XML      |            | N         | Bills type node representing bills of a specific denomination. See Bills Type
| Coins        | XML      |            | N         | Coins type node representing coins taken in or out of a specific denomination.
| UserData     | XML      |            | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. See Coins Type

Element name: Bills

#### Type: Bills

| **Field** | **Type** | **Length** | **Req’d** | **Notes**
| --------- | -------- | ---------- | --------- | -----------------------------------------------------
| Direction | Token    |            | Y         | Values: "IN" or "OUT"
| Value     | Decimal  |            | Y         | Denomination of bill
| Count     | Decimal  |            | Y         | Number of bills of the specified denomination
| Amount    | Decimal  |            | Y         | Total amount of bills of the specified denomination

Element name: Coins

#### Type: Coins

| **Field** | **Type** | **Length** | **Req’d** | **Notes** |
| --------- | -------- | ---------- | --------- | ----------------------------------------------------- |
| Direction | XML      |            | Y         | One or more tender elements |
| Value     | Decimal  |            | Y         | Denomination of coin |
| Count     | Decimal  |            | Y         | Number of coins of the specified denomination |
| Amount    | Decimal  |            | Y         | Total amount of coins of the specified denomination |

### Consumer Transaction

Consumer transaction represents adjusting consumer account’s balance

**Data collection element: ConsumerTransactions**

**Data item element: ConsumerTransaction**

**VDIXMLType value: mms-transactions**

#### Type: ConsumerTransaction

| **Field**        | **Type** | **Length** | **Req’d** | **Notes** |
| ---------------- | -------- | ----------:| --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| @MarketID        | Token    | 64         | Y         | ID of the market in which the sale took place |
| @KioskID         | Token    | 64         | N         | ID of the kiosk, the physical machine, on which the sale took place |
| @ConsumerID      | Token    | 64         | Y         | ID of the consumer who made the purchase |
| @ConsumerName    | Token    | 128        | N         | Human friendly name of the consumer who made the purchase. |
| @Type            | Token    | 64         | Y         | Transaction type with the following valid values:<br/><ul><li>CASH (cash funding)</li><li>CARD (credit or debit card)</li><li>EXTERNAL (external system integrations.)</li><li>ADJUSTMENT (adjustment)</li></ul> |
| @Description     | Token    | 64         | N         | Brief description of the payment type. Can be used to provide additional details.<br/><br/>Examples for CARD type:<br/><ul><li>AMX (any Amex card)</li><li>VISA DEBIT (visa debit card)</li><li>CREDIT (generic credit card)</li></ul>Actual values should be agreed on the implementation level |
| @TransactionID   | Token    | 64         | Y         | Token uniquely identifying the transaction. |
| @TransactionTime | DateTime |            | Y         | Date, time and time zone of the transaction. |
| @Amount          | Decimal  |            | Y         | The amount of the transaction. In case of negative adjustments it should be a negative value. |
| @Reference       |          |            | N         | An external reference that allows reconciliation with an external processing systems. |
| @Reason          |          |            | N         | Reason for the transaction. Most likely it will be only used for ADJUSTMENT type. |
| @Notes           | Token    | 256        | N         | Additional comments about the transaction |
| Bills            | XML      |            | N         | Bill type. Bills taken in or out of a specific denomination |
| Coins            | XML      |            | N         | Coin type. Coins taken in or out of a specific denomination |
| UserData         | XML      |            | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |

### Product Catalog

Contains products that are for sale in the market, along with their
price, sales tax rate and bottle fee.

**Data collection element: MarketsCollection**

**Data item element: Market**

**VDIXMLType value: mms-products**

| **Field**       | **Type** | **Length** | **Req’d** | **Notes**
| --------------- | -------- | ---------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------
| @MarketID       | Token    | 64         | Y         | 
| @CatalogVersion | Token    | 64         | N         | Any information that identifies the version of the catalog.
| @CatalogSize    | Token    |            | Y         | Values: "Full" or "Partial"
| ProductsUpdate  | XML      |            | N         | Collection containing Product type nodes with products which need to be added or updated. See Product Type.
| ProductsRemove  | XML      |            | N         | Collection containing Product type nodes with products which will be removed. Only used with incremental catalogs. See Product Type.

Product Detail

#### Type: Product

| **Field**     | **Type** | **Req’d** | **Notes** |
| ------------- | -------- | --------- | ------------------------------------------------------------------------ |
| @ProductID    | Token    | Y         | Unique ID of the Product |
| @ProductCode  | Token    | N         | Secondary ID. May or may not be unique depending on implementation. |
| @ProductName  | Token    | N         | Short name for product (40 char). |
| @CategoryCode | Token    | N         | Product category identifier |
| @Category     | Token    | N         | Product category description (40 char). |
| @Cost         | Decimal  | N         | Cost of the unit of the product. |
| @Price        | Decimal  | N         | Price at which the product is to be sold. |
| Codes         | XML      | N         | Collection containing Code nodes which hold barcode in the inner text. |
| Taxes         | XML      | N         | Collection of Tax type nodes |
| Fees          | XML      | N         | Collection of Fee type nodes |


### Kiosk

Kiosk represents deployed and configured kiosk which is part of a
market.

**Data collection element: KiosksCollection**

**Data item element: Kiosk**

**VDIXMLType value: mms-kiosks**

#### Type: Kiosk

| **Field**        | **Type** | **Req’d** | **Notes** |
| ---------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| @MarketID        | Token    | Y         | Unique ID of the market |
| @KioskID         | Token    | Y         | Unique kiosk ID |
| @LastSync        | Datetime | N         | Date, time and offset of the last time kiosk sync’d data. |
| @LastTransaction | Datetime | N         | Date, time and offset of the last time a sale or a consumer transaction was made |
| @CatalogVersion  | Token    | N         | Product catalog version which is currently deployed in the kiosk. The value should be discussed on the implementation level. |
| UserData         | XML      | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |


### Market

Market represents market details as understood by VMS

**Data collection element: MarketsCollection**

**Data item element: Market**

**VDIXMLType value: mms-markets**

#### Type: Market

| **Field**       | **XML Type** | **Length** | **Req’d** | **Notes**
| --------------- | ------------ | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------------------
| @MarketID       | Token        | 64         | Y         | Unique ID assigned to the market by the VMS
| @MarketName     | Token        | 128        | Y         | Name of the market
| @MarketAddress  | Token        | 128        | N         | Address of the market
| @MarketLocation | Token        | 128        | N         | 
| @ClientId       | Token        | 64         | N         | 
| @ClientName     | Token        | 128        | N         | 
| Kiosks          | Collection   |            | N         | Collection of kiosks associated with the market represented by type MarketKiosk
| UserData        | XML          |            | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs.

| **Field** | **XML Type** | **Length ** | **Req’d** | **Notes** |
| --------- | ------------ | ----------- | --------- | ------------------------- |
| @KioskID  | Token        | 64          | Y         | Kiosk/POS identifier |
| @KioskSN  | Token        | 64          | N         | Kiosk/POS Serial Number |

### Consumer

**Data collection element: Consumers**

**Data item element: Consumer**

**VDIXMLType value: mms-consumers**

#### Type: Consumer

| **Field**       | **Type** | **Req’d** | **Notes** |
| --------------- | -------- | --------- | ------------------------------------------------- |
| ProviderID      | Varchar  | Y         | Unique ID of provider (VDI Integration partner) |
| ConsumerID      | Varchar  | Y         | Consumer identifier (MMS Customer ID) |
| FirstName       | Varchar  | Y         | Consumer First Name |
| LastName        | Varchar  | Y         | Consumer Last Name |
| Email           | Varchar  | N         | Consumer Email Address |
| Balance         | Money    | Y         | Current balance on Consumer account |
| BalanceDateTime | DateTime | Y         | The last time the balance was updated/changed. |

### Cash Collection

**Data collection element: CashCollections**

**Data item element: CashCollection**

**VDIXMLType value: mms-collections**

#### Type: CashCollection

| **Field**       | **Type** | **Req’d** | **Notes** |
| --------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------------------------------- |
| @MarketId       | Token    | Y         | Unique ID of the market |
| @KioskID        | Token    | Y         | Unique kiosk ID |
| @CollectionTime | Datetime | Y         | Date, time and offset of the collection |
| @Amount         | Money    | Y         | Total value of the collection |
| @CollectedBy    | Token    | N         | Any type of identifier that represents the person who collected the cash |
| @Reference      | Token    | N         | Reference used for reconciliation |
| Bills           | XML      | N         | Bills taken in or out of a specific denomination. See Bills Type. |
| Coins           | XML      | N         | Coins taken in or out of a specific denomination. See Coins Type. |
| UserData        | XML      | N         | Single XML node containing any valid XML elements which is used to extend the standard for any implementation specific needs. |

## Appendix A: XML Examples

### Sales

```xml
<?xml version="1.0" encoding="utf-8"?>
<VDITransaction VDIXMLVersion="1" VDIXMLType="mms-sales" ProviderID="365" ApplicationID="smartshop" ApplicationVersion="1902" TransactionID="C2601E9D-6688-47D2-A6A7-12AB110FBF09" TransactionTime="2015-10-14T06:37:37.7881105-07:00" OperatorID="46">
    <Sales>
        <Sale MarketID="2008:46941" KioskID="BF9EE4B3-5908-E511-A42C-90B11C2CD708" ConsumerID="d97e04e1-e318-e511-a42c-90b11c2cd708" SaleID="11312e2d-224c-4447-ab00-d71a4687b90e" SaleTime="2015-10-14T09:32:26-04:00">
            <Summary Price="3.89" Discount="0.00" Total="4.16">
                <Fees Total="0.00" />
                <Taxes Total="0.27">
                    <Tax Rate="0.0700" Count="2" Total="0.2700" />
                </Taxes>
            </Summary>
            <Items>
                <Item Code="894700010168" Quantity="1" Cost="1.2100" Price="1.8900" Discount="0.00" Total="2.02">
                    <Fees Total="0.0000" />
                    <Taxes Total="0.1300">
                        <Tax Rate="0.0700" Value="0.1300" Count="1" Total="0.1300" />
                    </Taxes>
                </Item>
                <Item Code="038057500624" Quantity="1" Cost="0.9000" Price="2.0000" Discount="0.00" Total="2.14">
                    <Fees Total="0.0000" />
                    <Taxes Total="0.1400">
                        <Tax Rate="0.0700" Value="0.1400" Count="1" Total="0.1400" />
                    </Taxes>
                </Item>
            </Items>
            <Tenders>
                <Tender Type="ACCOUNT" Amount="4.16" />
            </Tenders>
        </Sale>
        <Sale MarketID="2008:46941" KioskID="BF9EE4B3-5908-E511-A42C-90B11C2CD708" ConsumerID="16142a8b-e718-e511-a42c-90b11c2cd708" SaleID="fff3995e-69b9-4904-bc1e-11c958977ad3" SaleTime="2015-10-14T09:31:07-04:00">
            <Summary Price="0.50" Discount="0.00" Total="0.50">
                <Fees Total="0.00" />
                <Taxes Total="0.00" />
            </Summary>
            <Items>
                <Item Code="" Quantity="1" Cost="0.2900" Price="0.5000" Discount="0.00" Total="0.50">
                    <Fees Total="0.0000" />
                    <Taxes Total="0.0000" />
                </Item>
            </Items>
            <Tenders>
                <Tender Type="ACCOUNT" Amount="0.50" />
            </Tenders>
        </Sale>
    </Sales>
</VDITransaction>
```

### *Consumer* Transactions

```xml
<?xml version="1.0" encoding="utf-8"?>
<VDITransaction VDIXMLVersion="1" VDIXMLType="mms-transactions" ProviderID="365" ApplicationID="smartshop" ApplicationVersion="1803" TransactionID="24636B8C-B196-4196-B50E-9E9EE438C500" TransactionTime="2015-08-26T09:07:34.0907520-07:00" OperatorID="46">
    <ConsumerTransactions>
        <ConsumerTransaction MarketID="3371:35358" KioskID="945DFC23-5DD2-4022-8032-A4BF8746FFF9" ConsumerID="c425d5ce-2c26-e511-a42c-90b11c2cd708" Type="CASH" Description="Cash" TransactionID="653b0fd8-0a24-41cf-9122-2b83dc916463" TransactionTime="2015-08-26T11:45:00-04:00" Amount="10.0000">
            <Bills Direction="IN" Value="10.0000" Count="1" Amount="10.0000" />
        </ConsumerTransaction>
        <ConsumerTransaction MarketID="3371:35358" KioskID="945DFC23-5DD2-4022-8032-A4BF8746FFF9" ConsumerID="c425d5ce-2c26-e511-a42c-90b11c2cd708" Type="CASH" Description="Cash" TransactionID="ce427cf1-b304-4548-b782-cf7e33ab6fa9" TransactionTime="2015-08-26T11:45:00-04:00" Amount="5.0000">
            <Bills Direction="IN" Value="5.0000" Count="1" Amount="5.0000" />
        </ConsumerTransaction>
        <ConsumerTransaction MarketID="3371:35358" KioskID="945DFC23-5DD2-4022-8032-A4BF8746FFF9" ConsumerID="c425d5ce-2c26-e511-a42c-90b11c2cd708" Type="CASH" Description="Cash" TransactionID="d517d03b-ea0c-4e84-bfa8-938f40c2c2d3" TransactionTime="2015-08-26T11:45:00-04:00" Amount="10.0000">
            <Bills Direction="IN" Value="10.0000" Count="1" Amount="10.0000" />
        </ConsumerTransaction>
    </ConsumerTransactions>
</VDITransaction>
```

### Products *Catalog*

```xml
<VDITransaction VDIXMLVersion="1" TransactionID="03CE060E-435F-4132-B458-010EE4A4AF5D" VDIXMLType="mms-products" TransactionTime="2015-10-01T14:18:06.520-04:00" ProviderID="Crane" OperatorID="894" ApplicationID="VendMAX" ApplicationVersion="5.0.8.1084">
    <MarketsCollection>
        <Market MarketID="5330:46707" CatalogSize="Partial">
            <ProductsUpdate>
                <Product ProductID="1:3583" ProductCode="54001" ProductName="8oz 2%" CategoryCode="540" Category="Milk" Price="0.0000" Cost="0.2100">
                    <Codes>
                        <Code>043119005375</Code>
                        <Code>044100100550</Code>
                        <Code>078800110625</Code>
                    </Codes>
                    <Taxes>
                        <Tax ID="5330:5063" Name="Test Market Tax" Rate="0.0800" IncludedInPrice="0" />
                    </Taxes>
                </Product>
                <Product ProductID="5330:31929" ProductCode="51530" ProductName="RP Caprese Salad" CategoryCode="510" Category="Packaged Foods" Price="0.0000" Cost="2.7183">
                    <Codes>
                        <Code>077745296654</Code>
                    </Codes>
                    <Taxes>
                        <Tax ID="5330:5063" Name="Test Market Tax" Rate="0.0800" IncludedInPrice="0" />
                    </Taxes>
                </Product>
            </ProductsUpdate>
        </Market>
    </MarketsCollection>
</VDITransaction>
```

### Kiosks

```xml
<?xml version="1.0" encoding="utf-8"?>
<VDITransaction VDIXMLVersion="1" VDIXMLType="mms-kiosks" ProviderID="365" ApplicationID="smartshop" ApplicationVersion="1902" TransactionID="9699F845-C4BD-448E-B4EF-270A72772215" TransactionTime="2015-10-23T07:52:31.1649762-07:00" OperatorID="46">
    <KiosksCollection>
        <Kiosk MarketID="2008:46941" KioskID="BF9EE4B3-5908-E511-A42C-90B11C2CD708" KioskSN="VSH312309" LastSync="2015-10-23 10:50:32.8500000 -04:00" LastTransaction="2015-10-23 10:37:40.0000000 -04:00" CatalogVersion="2015-10-23 07:37:31.8451114 -07:00" />
        <Kiosk MarketID="3371:35358" KioskID="945DFC23-5DD2-4022-8032-A4BF8746FFF9" KioskSN="VSH312369" LastSync="2015-10-15 13:02:35.2400000 -04:00" LastTransaction="2015-10-08 08:47:50.0000000 -04:00" CatalogVersion="2015-10-22 10:27:32.1075657 -07:00" />
        <Kiosk MarketID="3371:40560" KioskID="B6487518-4C66-E511-8759-90B11C2CD708" KioskSN="VSH312944" LastSync="2015-10-23 10:50:38.8100000 -04:00" CatalogVersion="2015-10-23 05:42:32.2661055 -07:00" />
    </KiosksCollection>
</VDITransaction>
```

### Markets

```xml
<VDITransaction VDIXMLVersion="1" TransactionID="321CADD4-2EFB-4F43-A4F5-DEE08F0678CF" VDIXMLType="mms-markets" TransactionTime="2015-10-22T18:39:00.277-04:00" ProviderID="Crane" OperatorID="46" ApplicationID="VendMAX" ApplicationVersion="5.0.8.1088">
    <MarketsCollection>
        <Market MarketID="3371:40560" MarketName="Acme Market" LocationID="3371:7321" LocationName="297 Somewhere Road" CustomerID="3371:6074" CustomerName="Looney Tunes" MarketAddress="297 Somewhere Road Troy MI 48084">
            <Kiosks>
                <Kiosk KioskID="B6487518-4C66-E511-8759-90B11C2CD708" KioskSN="VSH312944" />
            </Kiosks>
        </Market>
    </MarketsCollection>
</VDITransaction>
```

### Collections

```xml
<?xml version="1.0" encoding="utf-8"?>
<VDITransaction VDIXMLVersion="1" VDIXMLType="mms-collections" ProviderID="365" ApplicationID="smartshop" ApplicationVersion="1902" TransactionID="35C1E8A4-4DE1-42C3-A07D-30B3BBBA97F7" TransactionTime="2015-10-23T07:07:33.9505665-07:00" OperatorID="46">
    <CashCollections>
        <CashCollection MarketID="2008:46941" KioskID="BF9EE4B3-5908-E511-A42C-90B11C2CD708" CollectionTime="2015-10-23T10:04:38-04:00" Amount="208.0000" CollectedBy="ljenkins">
            <Bills Direction="OUT" Value="208.0000" Count="1" Amount="208.0000" />
        </CashCollection>
    </CashCollections>
</VDITransaction>
```

### Consumers

```xml
<?xml version="1.0" encoding="utf-8"?>
<VDITransaction VDIXMLVersion="1" VDIXMLType="mms-consumers" ProviderID="365" ApplicationID="smartshop" ApplicationVersion="1901" TransactionID="4D4371BD-9108-4635-9BF5-80724FC3EC03" TransactionTime="2015-10-23T08:02:32.2011817-07:00" OperatorID="894">
    <Consumers>
        <Consumer MarketID="5330:46707" KioskID="31D4F875-1526-495A-B437-650EE1DC9F53" ProviderID="TODO: PROVIDER" ConsumerID="95bdb5cd-4668-e511-8759-90b11c2cd708" FirstName="CHAD" LastName="YOUNG" Email="CHAD.YOUNG@ACMEVENDING.COM" Balance="33.3100" BalanceDateTime="2015-10-23T15:02:32.160" />
    </Consumers>
</VDITransaction>
```
