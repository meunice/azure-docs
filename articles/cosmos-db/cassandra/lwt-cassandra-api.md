---
title: Lightweight Transactions in Azure Cosmos DB Cassandra API
description: Learn about Lightweight Transaction support in Azure Cosmos DB Cassandra API
author: IriaOsara
ms.author: IriaOsara
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 11/19/2021
ms.custom: template-how-to
---

# Azure Cosmos DB Cassandra API Lightweight Transactions with Conditions
[!INCLUDE[appliesto-cassandra-api](../includes/appliesto-cassandra-api.md)]

> [!IMPORTANT]
> Lightweight Transactions for Azure Cosmos DB API for Cassandra is currently in public preview.
> This preview version is provided without a service level agreement. Certain features might not be supported or might have constrained capabilities.
> For more information, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Apache Cassandra as most NoSQL database platforms gives precedence to availability and partition-tolerance above consistency as it does not support ACID transactions as in relational database. For details on how consistency level works with LWT see [Azure Cosmos DB Cassandra API consistency levels](apache-cassandra-consistency-mapping.md). Cassandra supports lightweight transactions(LWT) which borders on ACID. It helps perform a read before write, for operations that require the data insert or update must be unique. 

## LWT support within Azure Cosmos DB Cassandra API
To use LWT within Azure Cosmos DB Cassandra API, we advise that the following flags are set at the create table level. 

```sql
with cosmosdb_cell_level_timestamp=true and cosmosdb_cell_level_timestamp_tombstones=true and cosmosdb_cell_level_timetolive=true
``` 
You might experience reduced performance on full row inserts compared to not using the flags.

> [!NOTE]
> LWTs are not supported for multi-region write scenarios.

## LWT with flags enabled
```sql
CREATE KEYSPACE IF NOT EXISTS lwttesting WITH REPLICATION= {'class': 'org.apache.cassandra.locator.SimpleStrategy', 'replication_factor' : '1'};
```

```sql
CREATE TABLE IF NOT EXISTS lwttesting.users (
  name text,
  userID int,
  address text,
  phoneCode int,
  vendorName text STATIC,
  PRIMARY KEY ((name), userID)) with cosmosdb_cell_level_timestamp=true and cosmosdb_cell_level_timestamp_tombstones=true and cosmosdb_cell_level_timetolive=true; 
```

This query below returns TRUE.
```sql
INSERT INTO lwttesting.users(name, userID, phoneCode, vendorName)
VALUES('Sara', 103, 832, 'vendor21') IF NOT EXISTS; 
``` 

There are some known limitations with flag enabled. If a row has been inserted into the table, an attempt to insert a static row will return FALSE. 
```sql
INSERT INTO lwttesting.users (userID, vendorName)
VALUES (104, 'staticVendor') IF NOT EXISTS;
```
The above query currently returns FALSE but should be TRUE.

## LWT with flags disabled
Row delete combined with IF condition is not supported if the flags are not enabled.

```sql
CREATE TABLE IF NOT EXISTS lwttesting.vendor_users (
  name text,
  userID int,
  areaCode int,
  vendor text STATIC,
  PRIMARY KEY ((userID), name)
);
```

```sql
DELETE FROM lwttesting.vendor_users 
WHERE userID =103 AND name = 'Sara' 
IF areaCode = 832;
```
An error message: Conditional delete of an entire row is not supported. 

## LWT with flags enabled or disabled
Any request containing assignment and condition combination of a static and regular column is unsupported with the IF condition.
This query will not return an error message as both columns are regular.
```sql
DELETE areaCode 
FROM lwttesting.vendor_users 
WHERE name= 'Sara' 
AND userID = 103 IF areaCode = 832;   
```
However, the query below returns an error message
`Conditions and assignments containing combination of Static and Regular columns are not supported.`
```sql
DELETE areaCode 
FROM lwttesting.vendor_users 
WHERE name= 'Sara' AND userID = 103 IF vendor = 'vendor21';  
```

## Next steps
In this tutorial, you've learned how Lightweight Transaction works within Azure Cosmos DB Cassandra API. You can proceed to the next article:
- [Migrate your data to a Cassandra API account](migrate-data.md)
- [Run Glowroot on Azure Cosmos DB Cassandra API](glowroot-cassandra.md)