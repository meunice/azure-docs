---
title: Azure Blob storage trigger and bindings for Azure Functions
description: Learn to use the Azure Blob storage trigger and bindings in Azure Functions.
author: craigshoemaker

ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
---

# Azure Blob storage bindings for Azure Functions overview

Azure Functions integrates with [Azure Storage](../storage/index.yml) via [triggers and bindings](./functions-triggers-bindings.md). Integrating with Blob storage allows you to build functions that react to changes in blob data as well as read and write values.

| Action | Type |
|---------|---------|
| Run a function as blob storage data changes | [Trigger](./functions-bindings-storage-blob-trigger.md) |
| Read blob storage data in a function | [Input binding](./functions-bindings-storage-blob-input.md) |
| Allow a function to write blob storage data |[Output binding](./functions-bindings-storage-blob-output.md) |

## Add to your Functions app

### Functions 2.x and higher

Working with the trigger and bindings requires that you reference the appropriate package. The NuGet package is used for .NET class libraries while the extension bundle is used for all other application types.

| Language                                        | Add by...                                   | Remarks 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installing the [NuGet package], version 3.x | |
| C# Script, Java, JavaScript, Python, PowerShell | Registering the [extension bundle]          | The [Azure Tools extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) is recommended to use with Visual Studio Code. |
| C# Script (online-only in Azure portal)         | Adding a binding                            | To update existing binding extensions without having to republish your function app, see [Update your extensions]. |

#### Storage extension 5.x and higher

A new version of the Storage bindings extension is now available. It introduces the ability to [connect using an identity instead of a secret](./functions-reference.md#configure-an-identity-based-connection). For a tutorial on configuring your function apps with managed identities, see the tutorial [creating a function app with identity-based connections](./functions-identity-based-connections-tutorial.md). For .NET applications, the new extension version also changes the types that you can bind to, replacing the types from `WindowsAzure.Storage` and `Microsoft.Azure.Storage` with newer types from [Azure.Storage.Blobs](/dotnet/api/azure.storage.blobs). Learn more about these new types are different and how to migrate to them from the [Azure.Storage.Blobs Migration Guide](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/storage/Azure.Storage.Blobs/AzureStorageNetMigrationV12.md).

This extension version is available by installing the [NuGet package], version 5.x, or it can be added from the extension bundle v3 by adding the following in your `host.json` file:

```json
{
  "version": "2.0",
  "extensionBundle": {
    "id": "Microsoft.Azure.Functions.ExtensionBundle",
    "version": "[3.3.0, 4.0.0)"
  }
}
```

[!INCLUDE [functions-bindings-storage-extension-v5-tables-note](../../includes/functions-bindings-storage-extension-v5-tables-note.md)]

To learn more, see [Update your extensions].

[core tools]: ./functions-run-local.md
[extension bundle]: ./functions-bindings-register.md#extension-bundles
[NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage
[Update your extensions]: ./functions-bindings-register.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### Functions 1.x

Functions 1.x apps automatically have a reference the [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet package, version 2.x.

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## host.json settings

This section describes the function app configuration settings available for functions that this binding. These settings only apply when using [extension version 5.0.0 and higher](#storage-extension-5x-and-higher). The example host.json file below contains only the version 2.x+ settings for this binding. For more information about function app configuration settings in versions 2.x and later versions, see [host.json reference for Azure Functions](functions-host-json.md).

> [!NOTE]
> This section doesn't apply to extension versions before 5.0.0. For those earlier versions, there aren't any function app-wide configuration settings for blobs.

```json
{
    "version": "2.0",
    "extensions": {
        "blobs": {
            "maxDegreeOfParallelism": "4"
        }
    }
}
```

|Property  |Default | Description |
|---------|---------|---------|
|maxDegreeOfParallelism|8 * (the number of available cores)|The integer number of concurrent invocations allowed for each blob-triggered function. The minimum allowed value is 1.|

## Next steps

- [Run a function when blob storage data changes](./functions-bindings-storage-blob-trigger.md)
- [Read blob storage data when a function runs](./functions-bindings-storage-blob-input.md)
- [Write blob storage data from a function](./functions-bindings-storage-blob-output.md)
