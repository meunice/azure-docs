---
title: Known issues with Azure Data Lake Storage Gen2
description: Learn about limitations and known issues of Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/14/2021
ms.author: normesta
ms.reviewer: jamesbak
---

# Known issues with Azure Data Lake Storage Gen2

This article describes limitations and known issues for accounts that have the hierarchical namespace feature enabled.

> [!NOTE]
> Some of the features described in this article might not be supported in accounts that have Network File System (NFS) 3.0 support enabled. To view a table that shows the impact of feature support when various capabilities are enabled, see [Blob Storage feature support in Azure Storage accounts](storage-feature-support-in-storage-accounts.md).

## Supported Blob storage features

An increasing number of Blob storage features now work with accounts that have a hierarchical namespace. For a complete list, see [Blob Storage features available in Azure Data Lake Storage Gen2](./storage-feature-support-in-storage-accounts.md).

## Supported Azure service integrations

Azure Data Lake Storage Gen2 supports several Azure services that you can use to ingest data, perform analytics, and create visual representations. For a list of supported Azure services, see [Azure services that support Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

For more information, see [Azure services that support Azure Data Lake Storage Gen2](data-lake-storage-supported-azure-services.md).

## Supported open source platforms

Several open source platforms support Data Lake Storage Gen2. For a complete list, see [Open source platforms that support Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

For more information, see [Open source platforms that support Azure Data Lake Storage Gen2](data-lake-storage-supported-open-source-platforms.md).

## Blob storage APIs

Data Lake Storage Gen2 APIs, NFS 3.0, and Blob APIs can operate on the same data.

This section describes issues and limitations with using blob APIs, NFS 3.0, and Data Lake Storage Gen2 APIs to operate on the same data.

- You cannot use blob APIs, NFS 3.0, and Data Lake Storage APIs to write to the same instance of a file. If you write to a file by using Data Lake Storage Gen2 or APIs or NFS 3.0, then that file's blocks won't be visible to calls to the [Get Block List](/rest/api/storageservices/get-block-list) blob API. The only exception is when using you are overwriting. You can overwrite a file/blob using either API or with NFS 3.0 by using the zero-truncate option.

- When you use the [List Blobs](/rest/api/storageservices/list-blobs) operation without specifying a delimiter, the results will include both directories and blobs. If you choose to use a delimiter, use only a forward slash (`/`). This is the only supported delimiter.

- If you use the [Delete Blob](/rest/api/storageservices/delete-blob) API to delete a directory, that directory will be deleted only if it's empty. This means that you can't use the Blob API delete directories recursively.

These Blob REST APIs aren't supported:

- [Put Blob (Page)](/rest/api/storageservices/put-blob)
- [Put Page](/rest/api/storageservices/put-page)
- [Get Page Ranges](/rest/api/storageservices/get-page-ranges)
- [Incremental Copy Blob](/rest/api/storageservices/incremental-copy-blob)
- [Put Page from URL](/rest/api/storageservices/put-page-from-url)

Unmanaged VM disks are not supported in accounts that have a hierarchical namespace. If you want to enable a hierarchical namespace on a storage account, place unmanaged VM disks into a storage account that doesn't have the hierarchical namespace feature enabled.

<a id="api-scope-data-lake-client-library"></a>

## Support for setting access control lists (ACLs) recursively

The ability to apply ACL changes recursively from parent directory to child items is generally available. In the current release of this capability, you can apply ACL changes by using Azure Storage Explorer, PowerShell, Azure CLI, and the .NET, Java, and Python SDK. Support is not yet available for the Azure portal.

## Access control lists (ACL) and anonymous read access

If [anonymous read access](./anonymous-read-access-configure.md) has been granted to a container, then ACLs have no effect on that container or the files in that container.  This only affects read requests.  Write requests will still honor the ACLs.

<a id="known-issues-tools"></a>

## AzCopy

Use only the latest version of AzCopy ([AzCopy v10](../common/storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Earlier versions of AzCopy such as AzCopy v8.1, are not supported.

<a id="storage-explorer"></a>

## Azure Storage Explorer

Use only versions `1.6.0` or higher.

<a id="explorer-in-portal"></a>

## Storage Explorer in the Azure portal

ACLs are not yet supported.

<a id="third-party-apps"></a>

## Third party applications

Third party applications that use REST APIs to work will continue to work if you use them with Data Lake Storage Gen2. Applications that call Blob APIs will likely work.

## Storage Analytics logs (classic)

The setting for retention days is not yet supported, but you can delete logs manually by using any supported tool such as Azure Storage Explorer, REST or an SDK.

## Windows Azure Storage Blob (WASB) driver

Currently, the WASB driver, which was designed to work with the Blob API only, encounters problems in a few common scenarios. Specifically, when it is a client to a hierarchical namespace enabled storage account. Multi-protocol access on Data Lake Storage won't mitigate these issues.

Using the WASB driver as a client to a hierarchical namespace enabled storage account is not supported. Instead, we recommend that you use the [Azure Blob File System (ABFS)](data-lake-storage-abfs-driver.md) driver in your Hadoop environment. If you are trying to migrate off of an on-premise Hadoop environment with a version earlier than Hadoop branch-3, then please open an Azure Support ticket so that we can get in touch with you on the right path forward for you and your organization.

## Soft delete for blobs capability

If parent directories for soft-deleted files or directories are renamed, the soft-deleted items may not be displayed correctly in the Azure portal. In such cases you can use [PowerShell](soft-delete-blob-manage.md?tabs=dotnet#restore-soft-deleted-blobs-and-directories-by-using-powershell) or [Azure CLI](soft-delete-blob-manage.md?tabs=dotnet#restore-soft-deleted-blobs-and-directories-by-using-azure-cli) to list and restore the soft-deleted items.

## Events

If your account has an event subscription, read operations on the secondary endpoint will result in an error. To resolve this issue, remove event subscriptions.

> [!TIP]
> Read access to the secondary endpoint is available only when you enable read-access geo-redundant storage (RA-GRS) or read-access geo-zone-redundant storage (RA-GZRS).
