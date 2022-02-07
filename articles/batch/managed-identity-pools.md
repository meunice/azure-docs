---
title: Configure managed identities in Batch pools
description: Learn how to enable user-assigned managed identities on Batch pools and how to use managed identities within the nodes.
ms.topic: conceptual
ms.date: 08/18/2021
ms.devlang: csharp
---
# Configure managed identities in Batch pools

[Managed identities for Azure resources](../active-directory/managed-identities-azure-resources/overview.md) eliminate the need for developers having to manage credentials by providing an identity for the Azure resource in Azure Active Directory (Azure AD) and using it to obtain Azure Active Directory (Azure AD) tokens.

This topic explains how to enable user-assigned managed identities on Batch pools and how to use managed identities within the nodes.

> [!IMPORTANT]
> Pools must be configured using [Virtual Machine Configuration](nodes-and-pools.md#virtual-machine-configuration) in order to use managed identities.
>
> Creating pools with managed identities can be done by using the [Batch .NET management library](/dotnet/api/overview/azure/batch#management-library), but is not currently supported with the [Batch .NET client library](/dotnet/api/overview/azure/batch#client-library).

## Create a user-assigned identity

First, [create your user-assigned managed identity](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity) in the same tenant as your Batch account. This managed identity does not need to be in the same resource group or even in the same subscription.

## Create a Batch pool with user-assigned managed identities

After you've created one or more user-assigned managed identities, you can create a Batch pool with that managed identity by using the [Batch .NET management library](/dotnet/api/overview/azure/batch#management-library).

```csharp
var poolParameters = new Pool(name: "yourPoolName")
    {
        VmSize = "standard_d1_v2",
        ScaleSettings = new ScaleSettings
        {
            FixedScale = new FixedScaleSettings
            {
                TargetDedicatedNodes = 1
            }
        },
        DeploymentConfiguration = new DeploymentConfiguration
        {
            VirtualMachineConfiguration = new VirtualMachineConfiguration(
                new ImageReference(
                    "Canonical",
                    "UbuntuServer",
                    "18.04-LTS",
                    "latest"),
                "batch.node.ubuntu 18.04")
        },
        Identity = new BatchPoolIdentity
        {
            Type = PoolIdentityType.UserAssigned,
            UserAssignedIdentities = new Dictionary<string, UserAssignedIdentities>
            {
                ["Your Identity Resource Id"] =
                    new UserAssignedIdentities()
            }
        }
    };

var pool = await managementClient.Pool.CreateWithHttpMessagesAsync(
    poolName:"yourPoolName",
    resourceGroupName: "yourResourceGroupName",
    accountName: "yourAccountName",
    parameters: poolParameters,
    cancellationToken: default(CancellationToken)).ConfigureAwait(false);    
```

## Use user-assigned managed identities in Batch nodes

Many Azure Batch technologies which access other Azure resources, such as Azure Storage or Azure Container Registry, support managed identities. For more information on using managed identities with Azure Batch, see the following links:

- [Resource files](resource-files.md)
- [Output files](batch-task-output-files.md#specify-output-files-using-managed-identity)
- [Azure Container Registry](batch-docker-container-workloads.md#managed-identity-support-for-acr)
- [Azure Blob container file system](virtual-file-mount.md#azure-blob-container)

You can also manually configure your tasks so that the managed identities can directly access [Azure resources that support managed identities](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md).

Within the Batch nodes, you can get managed identity tokens and use them to authenticate through Azure AD authentication via the [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md).

For Windows, the PowerShell script to get an access token to authenticate is:

```powershell
$Response = Invoke-RestMethod -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource={Resource App Id Url}' -Method GET -Headers @{Metadata="true"} 
```

For Linux, the Bash script is:

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource={Resource App Id Url}' -H Metadata:true
```

For more information, see [How to use managed identities for Azure resources on an Azure VM to acquire an access token](../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## Next steps

- Learn more about [Managed identities for Azure resources](../active-directory/managed-identities-azure-resources/overview.md).
- Learn how to use [customer-managed keys with user-managed identities](batch-customer-managed-key.md).
- Learn how to [enable automatic certificate rotation in a Batch pool](automatic-certificate-rotation.md).
