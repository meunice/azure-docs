---
title: Quickstart for managing Azure file shares
description: See how to create and manage Azure file shares with the Azure portal, Azure CLI, or Azure PowerShell module. Create a storage account, create an Azure file share, and use your Azure file share.
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 09/17/2021
ms.author: rogarana
ms.subservice: files
ms.custom: mode-ui
#Customer intent: As a < type of user >, I want < what? > so that < why? >.
---

# Quickstart: Create and manage Azure file shares
[Azure Files](storage-files-introduction.md) is Microsoft's easy-to-use cloud file system. Azure file shares can be mounted in Windows, Linux, and macOS. This guide walks you through the basics of working with Azure file shares using either the Azure portal, Azure CLI, or Azure PowerShell module.

## Applies to
| File share type | SMB | NFS |
|-|:-:|:-:|
| Standard file shares (GPv2), LRS/ZRS | ![Yes](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Standard file shares (GPv2), GRS/GZRS | ![Yes](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |
| Premium file shares (FileStorage), LRS/ZRS | ![Yes](../media/icons/yes-icon.png) | ![No](../media/icons/no-icon.png) |

## Pre-requisites

# [Portal](#tab/azure-portal)

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

# [PowerShell](#tab/azure-powershell)

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

If you would like to install and use the PowerShell locally, this guide requires the Azure PowerShell module Az version 0.7 or later. To find out which version of the Azure PowerShell module you are running, execute `Get-Module -ListAvailable Az`. If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-Az-ps). If you are running PowerShell locally, you also need to run `Login-AzAccount` to log in to your Azure account.

# [Azure CLI](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- This article requires version 2.0.4 or later of the Azure CLI. If using Azure Cloud Shell, the latest version is already installed.

- By default, Azure CLI commands return JavaScript Object Notation (JSON). JSON is the standard way to send and receive messages from REST APIs. To facilitate working with JSON responses, some of the examples in this article use the *query* parameter on Azure CLI commands. This parameter uses the [JMESPath query language](http://jmespath.org/) to parse JSON. To learn more about how to use the results of Azure CLI commands by following the JMESPath query language, see the [JMESPath tutorial](http://jmespath.org/tutorial.html).

---

## Create a storage account

# [Portal](#tab/azure-portal)
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]


# [PowerShell](#tab/azure-powershell)


### PowerShell - Create a resource group
A resource group is a logical container into which Azure resources are deployed and managed. If you don't already have an Azure resource group, you can create a new one with the [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet. You need a resource group to create a storage account.

The following example creates a resource group named *myResourceGroup* in the West US 2 region:

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$region = "westus2"

New-AzResourceGroup `
    -Name $resourceGroupName `
    -Location $region | Out-Null
```

### PowerShell - Create a storage account

A storage account is a shared pool of storage you can use to deploy Azure file shares.

This example creates a storage account using the [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) cmdlet. The storage account is named *mystorageaccount\<random number>* and a reference to that storage account is stored in the variable **$storageAcct**. Storage account names must be unique, so use `Get-Random` to append a number to the name to make it unique. 

```azurepowershell-interactive 
$storageAccountName = "mystorageacct$(Get-Random)"

$storageAcct = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -Location $region `
    -Kind StorageV2 `
    -SkuName Standard_LRS `
    -EnableLargeFileShare
```

# [Azure CLI](#tab/azure-cli)

### CLI - Create a resource group
A resource group is a logical container in which Azure resources are deployed and managed. If you don't already have an Azure resource group, you can use the [az group create](/cli/azure/group) command to create one. You need a resource group to create a storage account.

The following example creates a resource group named *myResourceGroup* in the *West US 2* location:

```azurecli-interactive 
export resourceGroupName="myResourceGroup"
region="westus2"

az group create \
    --name $resourceGroupName \
    --location $region \
    --output none
```

### CLI - Create a storage account
A storage account is a shared pool of storage in which you can deploy Azure file shares.

The following example creates a storage account using the [az storage account create](/cli/azure/storage/account) command. Storage account names must be unique, so use `$RANDOM` to append a number to the name to make it unique.

```azurecli-interactive 
export storageAccountName="mystorageacct$RANDOM"

az storage account create \
    --resource-group $resourceGroupName \
    --name $storageAccountName \
    --location $region \
    --kind StorageV2 \
    --sku Standard_LRS \
    --enable-large-file-share \
    --output none
```


---


## Create an Azure file share


# [Portal](#tab/azure-portal)

To create an Azure file share:

1. Select the storage account from your dashboard.
1. On the storage account page, in the **Services** section, select **Files**.
	
    ![A screenshot of the data storage section of the storage account; select file shares.](media/storage-how-to-use-files-portal/create-file-share-1.png)

1. On the menu at the top of the **File service** page, click **File share**. The **New file share** page drops down.
1. In **Name** type *myshare*, enter a quota, and leave **Transaction optimized** selected for **Tiers**.
1. Select **Create** to create the Azure file share.

Share names need to be all lower case letters, numbers, and single hyphens but cannot start with a hyphen. For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata).

# [PowerShell](#tab/azure-powershell)

Now that you've created a storage account, you can create your first Azure file share. Create a file share by using the [New-AzRmStorageShare](/powershell/module/az.storage/New-AzRmStorageShare) cmdlet. This example creates a share named **myshare**.

```azurepowershell-interactive
$shareName = "myshare"

New-AzRmStorageShare `
    -StorageAccount $storageAcct `
    -Name $shareName `
    -EnabledProtocol SMB `
    -QuotaGiB 1024 | Out-Null
```

# [Azure CLI](#tab/azure-cli)

Now that you've created a storage account, you can create your first Azure file share. Create file shares by using the [az storage share-rm create](/cli/azure/storage/share-rm#az_storage_share_rm_create) command. This example creates an Azure file share named **myshare**: 

```azurecli-interactive
shareName="myshare"

az storage share-rm create \
    --resource-group $resourceGroupName \
    --storage-account $storageAccountName \
    --name $shareName \
    --quota 1024 \
    --enabled-protocols SMB \
    --output none
```

---

#### Create a directory

# [Portal](#tab/azure-portal)

To create a new directory named *myDirectory* at the root of your Azure file share:

1. On the **File Service** page, select the **myshare** file share. The page for your file share opens.
1. On the menu at the top of the page, select **+ Add directory**. The **New directory** page drops down.
1. Type *myDirectory* and then click **OK**.

# [PowerShell](#tab/azure-powershell)

To create a new directory named **myDirectory** at the root of your Azure file share, use the [New-AzStorageDirectory](/powershell/module/az.storage/New-AzStorageDirectory) cmdlet.

```azurepowershell-interactive
New-AzStorageDirectory `
   -Context $storageAcct.Context `
   -ShareName $shareName `
   -Path "myDirectory"
```

# [Azure CLI](#tab/azure-cli)

To create a new directory named **myDirectory** at the root of your Azure file share, use the [`az storage directory create`](/cli/azure/storage/directory) command:

```azurecli-interactive
az storage directory create \
   --account-name $storageAccountName \
   --account-key $storageAccountKey \
   --share-name $shareName \
   --name "myDirectory" \
   --output none
```

---


#### Upload a file 
# [Portal](#tab/azure-portal)


To demonstrate uploading a file, you first need to create or select a file to be uploaded. You may do this by whatever means you see fit. Once you've selected the file you would like to upload:

1. Select the **myDirectory** directory. The **myDirectory** panel opens.
1. In the menu at the top, select **Upload**. The **Upload files** panel opens.  
	
    ![A screenshot of the upload files panel](media/storage-how-to-use-files-portal/upload-file-1.png)

1. Select the folder icon to open a window to browse your local files. 
1. Select a file and then select **Open**. 
1. In the **Upload files** page, verify the file name and then select **Upload**.
1. When finished, the file should appear in the list on the **myDirectory** page.

# [PowerShell](#tab/azure-powershell)

To demonstrate how to upload a file using the [Set-AzStorageFileContent](/powershell/module/az.storage/Set-AzStorageFileContent) cmdlet, we first need to create a file inside your PowerShell Cloud Shell's scratch drive to upload. 

This example puts the current date and time into a new file on your scratch drive, then uploads the file to the file share.

```azurepowershell-interactive
# this expression will put the current date and time into a new file on your scratch drive
cd "~/CloudDrive/"
Get-Date | Out-File -FilePath "SampleUpload.txt" -Force

# this expression will upload that newly created file to your Azure file share
Set-AzStorageFileContent `
   -Context $storageAcct.Context `
   -ShareName $shareName `
   -Source "SampleUpload.txt" `
   -Path "myDirectory\SampleUpload.txt"
```   

If you're running PowerShell locally, substitute `~/CloudDrive/` with a path that exists on your machine.

After uploading the file, you can use [Get-AzStorageFile](/powershell/module/Az.Storage/Get-AzStorageFile) cmdlet to check to make sure that the file was uploaded to your Azure file share. 

```azurepowershell-interactive
Get-AzStorageFile `
    -Context $storageAcct.Context `
    -ShareName $shareName `
    -Path "myDirectory\" 
```


# [Azure CLI](#tab/azure-cli)

To demonstrate how to upload a file by using the [`az storage file upload`](/cli/azure/storage/file) command, first create a file to upload on the Cloud Shell scratch drive. In the following example, you create and then upload the file:

```azurecli-interactive
cd ~/clouddrive/
date > SampleUpload.txt

az storage file upload \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --share-name $shareName \
    --source "SampleUpload.txt" \
    --path "myDirectory/SampleUpload.txt"
```

If you're running Azure CLI locally, substitute `~/clouddrive` with a path that exists on your machine.

After you upload the file, you can use the [`az storage file list`](/cli/azure/storage/file) command to make sure that the file was uploaded to your Azure file share:

```azurecli-interactive
az storage file list \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --share-name $shareName \
    --path "myDirectory" \
    --output table
```


---

#### Download a file
# [Portal](#tab/azure-portal)

You can download a copy of the file you uploaded by right-clicking on the file. After selecting the download button, the exact experience will depend on the operating system and browser you're using.


# [PowerShell](#tab/azure-powershell)

You can use the [Get-AzStorageFileContent](/powershell/module/az.storage/Get-AzStorageFilecontent) cmdlet to download a copy of the file you uploaded to the scratch drive of your Cloud Shell.

```azurepowershell-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists because you've run this example before.
Remove-Item `
    -Path "SampleDownload.txt" `
    -Force `
    -ErrorAction SilentlyContinue

Get-AzStorageFileContent `
    -Context $storageAcct.Context `
    -ShareName $shareName `
    -Path "myDirectory\SampleUpload.txt" `
    -Destination "SampleDownload.txt"
```

After downloading the file, you can use the `Get-ChildItem` to see that the file has been downloaded to your PowerShell Cloud Shell's scratch drive.

```azurepowershell-interactive
Get-ChildItem | Where-Object { $_.Name -eq "SampleDownload.txt" }
``` 

# [Azure CLI](#tab/azure-cli)

You can use the [`az storage file download`](/cli/azure/storage/file) command to download a copy of the file that you uploaded to the Cloud Shell scratch drive:

```azurecli-interactive
# Delete an existing file by the same name as SampleDownload.txt, if it exists, because you've run this example before
rm -f SampleDownload.txt

az storage file download \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --share-name $shareName \
    --path "myDirectory/SampleUpload.txt" \
    --dest "SampleDownload.txt" \
    --output none
```

---

## Clean up resources
# [Portal](#tab/azure-portal)

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

# [PowerShell](#tab/azure-powershell)

When you are done, you can use the [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet to delete the resource group and all resources contained in the resource group. 

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

# [Azure CLI](#tab/azure-cli)

When you are done, you can use the [`az group delete`](/cli/azure/group) command to delete the resource group and all resources contained in the resource group: 

```azurecli-interactive 
az group delete --name $resourceGroupName
```


---

## Next steps

> [!div class="nextstepaction"]
> [What is Azure Files?](storage-files-introduction.md)
