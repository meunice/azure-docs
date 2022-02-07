---
title: Get the latest restorable timestamp for Azure Cosmos DB accounts with continuous backup mode
description: Learn how to get the latest restorable timestamp for accounts enabled with continuous backup mode. It explains how to get the latest restorable time for SQL containers and MongoDB collections using Azure PowerShell and Azure CLI.
author: kanshiG
ms.author: govindk
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.date: 11/18/2021
ms.topic: how-to
ms.reviewer: sngun
---

# Get the latest restorable timestamp for continuous backup accounts
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

This article describes how to get the [latest restorable timestamp](latest-restore-timestamp-continuous-backup.md) for accounts with continuous backup mode. It explains how to get the latest restorable time for SQL containers and MongoDB collections using Azure PowerShell and Azure CLI. You can see the request and response format for the PowerShell and CLI commands.

## SQL container

### PowerShell

```powershell
Get-AzCosmosDBSqlContainerBackupInformation -AccountName <System.String> `
  -DatabaseName <System.String> `
  [-DefaultProfile <Microsoft.Azure.Commands.Common.Authentication.Abstractions.Core.IAzureContextContainer>] `
  -Location <System.String> `
  -Name <System.String> `
  -ResourceGroupName <System.String> [<CommonParameters>]
```

**Sample request:**

```powershell
Get-AzCosmosDBSqlContainerBackupInformation -ResourceGroupName "rg" `
  -AccountName "sqlpitracc" `
  -DatabaseName "db1" `
  -Name "container1" `
  -Location "eastus"
```

**Sample response (In UTC Format):**

```console
LatestRestorableTimestamp
-------------------------
10/26/2021 6:24:25 PM
```

### CLI

```azurecli
az cosmosdb sql retrieve-latest-backup-time -g {resourcegroup} \
  -a {accountname} \
  -d {db_name} \
  -c {container_name} \
  -l {location}
```

**Sample request:**

```azurecli
az cosmosdb sql retrieve-latest-backup-time -g "rg" \
  -a "sqlpitracc" \
  -d "db1" \
  -c "container1" \
  -l "eastus"
```

**Sample response (In UTC Format):**

```console
{
  "continuousBackupInformation": {
    "latestRestorableTimestamp": "10/26/2021 5:27:45 PM"
  }
}
```

## SQL database

Use the following script to get the latest restorable timestamp for a database. This script will iterate through all the containers within the specified database and returns the minimum of latest restorable timestamp of all its containers.

```powershell
Function Get-LatestRestorableTimestampForSqlDatabase {
[CmdletBinding()]
param(
  [Parameter(Position = 0, mandatory = $true)]
  [string] $resourceGroupName,
  [Parameter(Position = 1, mandatory = $true)]
  [string] $accountName,
  [Parameter(Position = 2, mandatory = $true)]
  [string] $databaseName,
  [Parameter(Position = 3, mandatory = $true)]
  [string] $location)

Write-Host
Write-Host "Latest restorable timestamp for a database is minimum of restorable timestamps of all the underlying containers"
Write-Host

Write-Debug "Listing all containers in database: $databaseName"
$containers = Get-AzCosmosDBSqlContainer -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $databaseName

If (-Not $containers) {
    throw "Error: Found no containers to restore in the given database."
}

Write-Debug "Found $($containers.Length) containers under database $databaseName"

$latestRestorableTimestampForDatabase = [DateTime]::MaxValue
foreach ($container in $containers) {
    Write-Debug "Getting latest restorable timestamp for container: $($container.Name)"

    $latestRestorableTimestampForContainer = Get-AzCosmosDBSqlContainerBackupInformation -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $databaseName -Name $container.Name -Location $location

    Write-Debug "Latest Restorable Timestamp for container $($container.Name): $($latestRestorableTimestampForContainer.LatestRestorableTimestamp)"

    $latestRestorableTimestampForContainerDateTime = [DateTime]$latestRestorableTimestampForContainer.LatestRestorableTimestamp

    If ($latestRestorableTimestampForContainerDateTime -lt $latestRestorableTimestampForDatabase) {
        Write-Debug "Latest Restorable Timestamp for container $($container.Name) is less than current database restorable timestamp: $latestRestorableTimestampForDatabase"

        $latestRestorableTimestampForDatabase = $latestRestorableTimestampForContainerDateTime
    }

    Write-Debug "Latest Restorable Timestamp for database so far: $latestRestorableTimestampForDatabase"
}

if ($latestRestorableTimestampForDatabase -eq [DateTime]::MaxValue) {
    throw "Error: Failed to retrieve latest backup timestamp for database: $databaseName"
}

Write-Debug "Latest Restorable Timestamp in UTC for database $($databaseName): $latestRestorableTimestampForDatabase"

return $latestRestorableTimestampForDatabase
}
```

**Syntax:**

```powershell
Get-LatestRestorableTimestampForSqlDatabase `
  -ResourceGroupName <resourceGroup> `
  -AccountName <accountName> `
  -DatabaseName <databaseName> `
  -Location <location>
```

**Sample request:**

```powershell
Import-Module .\LatestRestorableDatabaseForSqlDatabase.ps1
Get-LatestRestorableTimestampForSqlDatabase `
  -ResourceGroupName rg `
  -AccountName sqlpitracc `
  -DatabaseName db1 `
  -Location eastus
```

**Sample response (In UTC Format):**

```console
Latest restorable timestamp for a database is minimum of restorable timestamps of all the underlying containers
Wednesday, November 3, 2021 8:02:44 PM
```

## SQL account

Use the following script to get the latest restorable timestamp for a SQL account. This script will iterate through all the containers within this account and returns the minimum of latest restorable timestamp of all its containers.

```powershell
Function Get-LatestRestorableTimestampForSqlAccount {
[CmdletBinding()]
param(
    [Parameter(Position = 0, mandatory = $true)]
    [string] $resourceGroupName,
    [Parameter(Position = 1, mandatory = $true)]
    [string] $accountName,
    [Parameter(Position = 2, mandatory = $true)]
    [string] $location)

Write-Host
Write-Host "Latest restorable timestamp for an account is minimum of restorable timestamps of all the underlying containers"
Write-Host

Write-Debug "Listing all databases in account: $accountName"

$databases = Get-AzCosmosDBSqlDatabase -ResourceGroupName $resourceGroupName -AccountName $accountName
Write-Debug "Found $($databases.Length) databases under account $accountName"

$latestRestorableTimestampForAccount = [DateTime]::MaxValue

foreach ($database in $databases) {
    Write-Debug "Listing all containers in database: $($database.Name)"
    $containers = Get-AzCosmosDBSqlContainer -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $($database.Name)

    If (-Not $containers) {
        throw "Error: Found no containers to restore in the given database."
    }

    Write-Debug "Found $($containers.Length) containers under database $($database.Name)"

    foreach ($container in $containers) {
        Write-Debug "Getting latest restorable timestamp for container: $($container.Name)"

        $latestRestorableTimestampForContainer = Get-AzCosmosDBSqlContainerBackupInformation -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $database.Name -Name $container.Name -Location $location

        Write-Debug "Latest Restorable Timestamp for container $($container.Name): $($latestRestorableTimestampForContainer.LatestRestorableTimestamp)"

        $latestRestorableTimestampForContainerDateTime = [DateTime]$latestRestorableTimestampForContainer.LatestRestorableTimestamp

        If ($latestRestorableTimestampForContainerDateTime -lt $latestRestorableTimestampForAccount) {
            Write-Debug "Latest Restorable Timestamp for container $($container.Name) is less than current database restorable timestamp: $latestRestorableTimestampForAccount"

            $latestRestorableTimestampForAccount = $latestRestorableTimestampForContainerDateTime
        }

        Write-Debug "Latest Restorable Timestamp for database so far: $latestRestorableTimestampForAccount"
    }
}

if ($latestRestorableTimestampForAccount -eq [DateTime]::MaxValue) {
    throw "Error: Failed to retrieve latest backup timestamp for account: $accountName"
}
    
Write-Debug "Latest Restorable Timestamp in UTC for account $($accountName): $latestRestorableTimestampForAccount"

return $latestRestorableTimestampForAccount
}
```

**Syntax:**

```powershell
Get-LatestRestorableTimestampForSqlAccount `
  -ResourceGroupName <resourceGroupName> `
  -AccountName <accountName> `
  -Location <location>
```

**Sample request:**

```powershell
Import-Module .\LatestRestorableTimestampForSqlAccount.ps1

Get-LatestRestorableTimestampForSqlAccount `
  -ResourceGroupName rg `
  -AccountName sqlpitracc `
  -location eastus
```

**Sample response (In UTC Format):**

```console
Latest restorable timestamp for an account is minimum of restorable timestamps of all the underlying containers
Wednesday, November 3, 2021 8:11:03 PM
```

## MongoDB collection

### PowerShell

```powershell
Get-AzCosmosDBMongoDBCollectionBackupInformation `
  -AccountName <System.String> `
  -DatabaseName <System.String> `
  [-DefaultProfile <Microsoft.Azure.Commands.Common.Authentication.Abstractions.Core.IAzureContextContainer>] `
  -Location <System.String> `
  -Name <System.String> `
  -ResourceGroupName <System.String> [<CommonParameters>]
```

**Sample request:**

```powershell
Get-AzCosmosDBMongoDBCollectionBackupInformation `
  -ResourceGroupName "rg" `
  -AccountName "mongodbpitracc" `
  -DatabaseName "db1" `
  -Name "col1" `
  -Location "eastus"
```

**Sample response (In UTC Format):**

```console
LatestRestorableTimestamp
-------------------------
10/26/2021 6:27:22 PM
```

### CLI

```azurecli
az cosmosdb mongodb retrieve-latest-backup-time \
  -g {resourcegroup} \
  -a {accountname} \
  -d {db_name} \
  -c {collection_name} \
  -l {location}
```

**Sample request:**

```azurecli
az cosmosdb mongodb retrieve-latest-backup-time \
  -g "rg" \
  -a "mongodbpitracc" \
  -d "db1" \
  -c "col1" \
  -l "eastus"
```

**Sample response:**

```console
{
  "continuousBackupInformation": {
    "latestRestorableTimestamp": "10/26/2021 5:27:45 PM"
  }
}
```

## MongoDB database

Use the following script to get the latest restorable timestamp for a database. This script will iterate through all the collections within this database and will return the minimum of latest restorable timestamp of all its collections.

```powershell
Function Get-LatestRestorableTimestampForMongoDBDatabase {
[CmdletBinding()]
param(
    [Parameter(Position = 0, mandatory = $true)]
    [string] $resourceGroupName,
    [Parameter(Position = 1, mandatory = $true)]
    [string] $accountName,
    [Parameter(Position = 2, mandatory = $true)]
    [string] $databaseName,
    [Parameter(Position = 3, mandatory = $true)]
    [string] $location)
    
Write-Host
Write-Host "Latest restorable timestamp for a database is minimum of restorable timestamps of all the underlying collections"
Write-Host

Write-Debug "Listing all collections in database: $databaseName"
$collections = Get-AzCosmosDBMongoDBCollection -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $databaseName

If (-Not $collections) {
    throw "Error: Found no collections to restore in the given database."
}

Write-Debug "Found $($collections.Length) collections under database $databaseName"

$latestRestorableTimestampForDatabase = [DateTime]::MaxValue
foreach ($collection in $collections) {
    Write-Debug "Getting latest restorable timestamp for collection: $($collection.Name)"

    $latestRestorableTimestampForCollection = Get-AzCosmosDBMongoDBCollectionBackupInformation -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $databaseName -Name $collection.Name -Location $location

    Write-Debug "Latest Restorable Timestamp for collection $($collection.Name): $($latestRestorableTimestampForCollection.LatestRestorableTimestamp)"

    $latestRestorableTimestampForCollectionDateTime = [DateTime]$latestRestorableTimestampForCollection.LatestRestorableTimestamp

    If ($latestRestorableTimestampForCollectionDateTime -lt $latestRestorableTimestampForDatabase) {
        Write-Debug "Latest Restorable Timestamp for collection $($collection.Name) is less than current database restorable timestamp: $latestRestorableTimestampForDatabase"

        $latestRestorableTimestampForDatabase = $latestRestorableTimestampForCollectionDateTime
    }

    Write-Debug "Latest Restorable Timestamp for database so far: $latestRestorableTimestampForDatabase"
}

if ($latestRestorableTimestampForDatabase -eq [DateTime]::MaxValue) {
    throw "Error: Failed to retrieve latest backup timestamp for database: $databaseName"
}

Write-Debug "Latest Restorable Timestamp in UTC for database $($databaseName): $latestRestorableTimestampForDatabase"

return $latestRestorableTimestampForDatabase
}
```

**Syntax:**

```powershell
Get-LatestRestorableTimestampForMongoDBDatabase `
  -ResourceGroupName <resourceGroup> `
  -AccountName <account> `
  -DatabaseName <database> `
  -Location <location>
```

**Sample request:**

```powershell
Import-Module .\LatestRestorableTimestampForMongoDBDatabase.ps1
Get-LatestRestorableTimestampForMongoDBDatabase -ResourceGroupName rg -accountName mongopitracc -databaseName db1 -location eastus
```

**Sample response (In UTC Format):**

```console
Latest restorable timestamp for a database is minimum of restorable timestamps of all the underlying collections
Wednesday, November 3, 2021 8:31:27 PM
```

## MongoDB account

Use can use the following script to get the latest restorable timestamp for a MongoDB account. This script will iterate through all the collections within this account and will return the minimum of latest restorable timestamp of all its collections.

```powershell
Function Get-LatestRestorableTimestampForMongoDBAccount {
[CmdletBinding()]
param(
    [Parameter(Position = 0, mandatory = $true)]
    [string] $resourceGroupName,
    [Parameter(Position = 1, mandatory = $true)]
    [string] $accountName,
    [Parameter(Position = 2, mandatory = $true)]
    [string] $location)

Write-Host
Write-Host "Latest restorable timestamp for an account is minimum of restorable timestamps of all the underlying collections"
Write-Host

Write-Debug "Listing all databases in account: $accountName"

$databases = Get-AzCosmosDBMongoDBDatabase -ResourceGroupName $resourceGroupName -AccountName $accountName
Write-Debug "Found $($databases.Length) databases under account $accountName"

$latestRestorableTimestampForAccount = [DateTime]::MaxValue

foreach ($database in $databases) {
    Write-Debug "Listing all collections in database: $($database.Name)"
    $collections = Get-AzCosmosDBMongoDBCollection -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $($database.Name)

    If (-Not $collections) {
        throw "Error: Found no collections to restore in the given database."
    }

    Write-Debug "Found $($collections.Length) collections under database $($database.Name)"

    foreach ($collection in $collections) {
        Write-Debug "Getting latest restorable timestamp for collection: $($collection.Name)"

        $latestRestorableTimestampForCollection = Get-AzCosmosDBMongoDBCollectionBackupInformation -ResourceGroupName $resourceGroupName -AccountName $accountName -DatabaseName $database.Name -Name $collection.Name -Location $location

        Write-Debug "Latest Restorable Timestamp for collection $($collection.Name): $($latestRestorableTimestampForCollection.LatestRestorableTimestamp)"

        $latestRestorableTimestampForCollectionDateTime = [DateTime]$latestRestorableTimestampForCollection.LatestRestorableTimestamp

        If ($latestRestorableTimestampForCollectionDateTime -lt $latestRestorableTimestampForAccount) {
            Write-Debug "Latest Restorable Timestamp for collection $($collection.Name) is less than current database restorable timestamp: $latestRestorableTimestampForAccount"

            $latestRestorableTimestampForAccount = $latestRestorableTimestampForCollectionDateTime
        }

        Write-Debug "Latest Restorable Timestamp for database so far: $latestRestorableTimestampForAccount"
    }
}

if ($latestRestorableTimestampForAccount -eq [DateTime]::MaxValue) {
    throw "Error: Failed to retrieve latest backup timestamp for account: $accountName"
}

Write-Debug "Latest Restorable Timestamp in UTC for account $($accountName): $latestRestorableTimestampForAccount"

return $latestRestorableTimestampForAccount
}
```

**Syntax:**

```powershell
Get-LatestRestorableTimestampForMongoDBAccount `
  -ResourceGroupName <resourceGroupName> `
  -AccountName <accountName> `
  -Location <location>
```

**Sample request:**

```powershell
Import-Module .\LatestRestorableTimestampForMongoDBAccount.ps1
Get-LatestRestorableTimestampForMongoDBAccount `
  -ResourceGroupName rg `
  -AccountName mongopitracc `
  -Location eastus
```

**Sample response (In UTC Format):**

```console
Latest restorable timestamp for an account is minimum of restorable timestamps of all the underlying collections
Wednesday, November 3, 2021 8:33:49 PM
```

## Next steps

* [Introduction to continuous backup mode with point-in-time restore.](continuous-backup-restore-introduction.md)

* [Continuous backup mode resource model.](continuous-backup-restore-resource-model.md)

* [Configure and manage continuous backup mode](continuous-backup-restore-portal.md) using Azure portal.
