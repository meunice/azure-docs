---
title: Cluster extensions for Azure Kubernetes Service (AKS) (preview)
description: Learn how to deploy and manage the lifecycle of extensions on Azure Kubernetes Service (AKS)
ms.service: container-service
ms.date: 10/13/2021
ms.topic: article
author: nickomang
ms.author: nickoman
---

# Deploy and manage cluster extensions for Azure Kubernetes Service (AKS) (preview)

Cluster extensions provides an Azure Resource Manager driven experience for installation and lifecycle management of services like Azure Machine Learning (ML) on an AKS cluster. This feature enables:

* Azure Resource Manager-based deployment of extensions, including at-scale deployments across AKS clusters.
* Lifecycle management of the extension (Update, Delete) from Azure Resource Manager.

In this article, you will learn about:
> [!div class="checklist"]

> * How to create an extension instance.
> * Available cluster extensions on AKS.
> * How to view, list, update, and delete extension instances.

A conceptual overview of this feature is available in [Cluster extensions - Azure Arc-enabled Kubernetes][arc-k8s-extensions] article.

[!INCLUDE [preview features note](./includes/preview/preview-callout.md)]

## Prerequisites

* An Azure subscription. If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free).
* [Azure CLI](/cli/azure/install-azure-cli) version >= 2.16.0 installed.

### Register provider for cluster extensions

#### [Azure CLI](#tab/azure-cli)

1. Enter the following commands:

    ```azurecli-interactive
    az provider register --namespace Microsoft.KubernetesConfiguration
    az provider register --namespace Microsoft.ContainerService
    ```

2. Monitor the registration process. Registration may take up to 10 minutes.

    ```azurecli-interactive
    az provider show -n Microsoft.KubernetesConfiguration -o table
    az provider show -n Microsoft.ContainerService -o table
    ```

    Once registered, you should see the `RegistrationState` state for these namespaces change to `Registered`.

#### [PowerShell](#tab/azure-powershell)

1. Enter the following commands:

    ```azurepowershell
    Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
    Register-AzResourceProvider -ProviderNamespace Microsoft.ContainerService
    ```

1. Monitor the registration process. Registration may take up to 10 minutes.

    ```azurepowershell
    Get-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
    Get-AzResourceProvider -ProviderNamespace Microsoft.ContainerService
    ```

    Once registered, you should see the `RegistrationState` state for these namespaces change to `Registered`.

---

### Register the `AKS-ExtensionManager` preview features

To create an AKS cluster that can use cluster extensions, you must enable the `AKS-ExtensionManager` feature flag on your subscription.

Register the `AKS-ExtensionManager` feature flag by using the [az feature register][az-feature-register] command, as shown in the following example:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "AKS-ExtensionManager"
```

It takes a few minutes for the status to show *Registered*. Verify the registration status by using the [az feature list][az-feature-list] command:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-ExtensionManager')].{Name:name,State:properties.state}"
```

When ready, refresh the registration of the *Microsoft.KubernetesConfiguration* and *Microsoft.ContainerService* resource providers by using the [az provider register][az-provider-register] command:

```azurecli-interactive
az provider register --namespace Microsoft.KubernetesConfiguration
az provider register --namespace Microsoft.ContainerService
```

### Setup the Azure CLI extension for cluster extensions

> [!NOTE]
> The minimum supported version for the `k8s-extension` Azure CLI extension is `1.0.0`. If you are unsure what version you have installed, run `az extension show --name k8s-extension` and look for the `version` field.

You will also need the `k8s-extension` Azure CLI extension. Install this by running the following commands:
  
```azurecli-interactive
az extension add --name k8s-extension
```

If the `k8s-extension` extension is already installed, you can update it to the latest version using the following command:

```azurecli-interactive
az extension update --name k8s-extension
```

## Currently available extensions

>[!NOTE]
> Cluster extensions provides a platform for different extensions to be installed and managed on an AKS cluster. If you are facing issues while using any of these extensions, please open a support ticket with the respective service.

| Extension | Description |
| --------- | ----------- |
| [Dapr][dapr-overview] | Dapr is a portable, event-driven runtime that makes it easy for any developer to build resilient, stateless and stateful applications that run on cloud and edge. |
| [Azure ML][azure-ml-overview] | Use Azure Kubernetes Service clusters to train, inference, and manage machine learning models in Azure Machine Learning. |
| [Flux (GitOps)][gitops-overview] | Use GitOps with Flux to manage cluster configuration and application deployment. |

## Supported regions and Kubernetes versions

Cluster extensions can be used on AKS clusters in the regions listed in [Azure Arc enabled Kubernetes region support][arc-k8s-regions].

For supported Kubernetes versions, refer to the corresponding documentation for each extension.

## Usage of cluster extensions

> [!NOTE]
> The samples provided in this article are not complete, and are only meant to showcase functionality. For a comprehensive list of commands and their parameters, please see the [az k8s-extension CLI reference][k8s-extension-reference].

### Create extensions instance

Create a new extension instance with `k8s-extension create`, passing in values for the mandatory parameters. The below command creates an Azure Machine Learning extension instance on your AKS cluster:

```azurecli
az k8s-extension create --name aml-compute --extension-type Microsoft.AzureML.Kubernetes --scope cluster --cluster-name <clusterName> --resource-group <resourceGroupName> --cluster-type managedClusters --configuration-settings enableInference=True allowInsecureConnections=True
```

> [!NOTE]
> The Cluster Extensions service is unable to retain sensitive information for more than 48 hours. If the cluster extension agents don't have network connectivity for more than 48 hours and cannot determine whether to create an extension on the cluster, then the extension transitions to `Failed` state. Once in `Failed` state, you will need to run `k8s-extension create` again to create a fresh extension instance.

#### Required parameters

| Parameter name | Description |
|----------------|------------|
| `--name` | Name of the extension instance |
| `--extension-type` | The type of extension you want to install on the cluster. For example: Microsoft.AzureML.Kubernetes | 
| `--cluster-name` | Name of the AKS cluster on which the extension instance has to be created |
| `--resource-group` | The resource group containing the AKS cluster |
| `--cluster-type` | The cluster type on which the extension instance has to be created. Specify `managedClusters` as it maps to AKS clusters|

#### Optional parameters

| Parameter name | Description |
|--------------|------------|
| `--auto-upgrade-minor-version` | Boolean property that specifies if the extension minor version will be upgraded automatically or not. Default: `true`.  If this parameter is set to true, you cannot set `version` parameter, as the version will be dynamically updated. If set to `false`, extension will not be auto-upgraded even for patch versions. |
| `--version` | Version of the extension to be installed (specific version to pin the extension instance to). Must not be supplied if auto-upgrade-minor-version is set to `true`. |
| `--configuration-settings` | Settings that can be passed into the extension to control its functionality. They are to be passed in as space separated `key=value` pairs after the parameter name. If this parameter is used in the command, then `--configuration-settings-file` can't be used in the same command. |
| `--configuration-settings-file` | Path to the JSON file having key value pairs to be used for passing in configuration settings to the extension. If this parameter is used in the command, then `--configuration-settings` can't be used in the same command. |
| `--configuration-protected-settings` | These settings are not retrievable using `GET` API calls or `az k8s-extension show` commands, and are thus used to pass in sensitive settings. They are to be passed in as space separated `key=value` pairs after the parameter name. If this parameter is used in the command, then `--configuration-protected-settings-file` can't be used in the same command. |
| `--configuration-protected-settings-file` | Path to the JSON file having key value pairs to be used for passing in sensitive settings to the extension. If this parameter is used in the command, then `--configuration-protected-settings` can't be used in the same command. |
| `--scope` | Scope of installation for the extension - `cluster` or `namespace` |
| `--release-namespace` | This parameter indicates the namespace within which the release is to be created. This parameter is only relevant if `scope` parameter is set to `cluster`. |
| `--release-train` |  Extension  authors can publish versions in different release trains such as `Stable`, `Preview`, etc. If this parameter is not set explicitly, `Stable` is used as default. This parameter can't be used when `autoUpgradeMinorVersion` parameter is set to `false`. |
| `--target-namespace` | This parameter indicates the namespace within which the release will be created. Permission of the system account created for this extension instance will be restricted to this namespace. This parameter is only relevant if the `scope` parameter is set to `namespace`. |

### Show details of an extension instance

View details of a currently installed extension instance with `k8s-extension show`, passing in values for the mandatory parameters:

```azurecli
az k8s-extension show --name azureml --cluster-name <clusterName> --resource-group <resourceGroupName> --cluster-type managedClusters
```

### List all extensions installed on the cluster

List all extensions installed on a cluster with `k8s-extension list`, passing in values for the mandatory parameters.

```azurecli
az k8s-extension list --cluster-name <clusterName> --resource-group <resourceGroupName> --cluster-type managedClusters
```

### Update extension instance

> [!NOTE]
> Refer to documentation of the extension type (Eg: Azure ML) to learn about the specific settings under ConfigurationSetting and ConfigurationProtectedSettings that are allowed to be updated. For ConfigurationProtectedSettings, all settings are expected to be provided during an update of a single setting. If some settings are omitted, those settings would be considered obsolete and deleted.

Update an existing extension instance with `k8s-extension update`, passing in values for the mandatory parameters. The below command updates the auto-upgrade setting for an Azure Machine Learning extension instance:

```azurecli
az k8s-extension update --name azureml --extension-type Microsoft.AzureML.Kubernetes --scope cluster --cluster-name <clusterName> --resource-group <resourceGroupName> --cluster-type managedClusters
```

**Required parameters**

| Parameter name | Description |
|----------------|------------|
| `--name` | Name of the extension instance |
| `--extension-type` | The type of extension you want to install on the cluster. For example: Microsoft.AzureML.Kubernetes | 
| `--cluster-name` | Name of the AKS cluster on which the extension instance has to be created |
| `--resource-group` | The resource group containing the AKS cluster |
| `--cluster-type` | The cluster type on which the extension instance has to be created. Specify `managedClusters` as it maps to AKS clusters|

**Optional parameters**

| Parameter name | Description |
|--------------|------------|
| `--auto-upgrade-minor-version` | Boolean property that specifies if the extension minor version will be upgraded automatically or not. Default: `true`.  If this parameter is set to true, you cannot set `version` parameter, as the version will be dynamically updated. If set to `false`, extension will not be auto-upgraded even for patch versions. |
| `--version` | Version of the extension to be installed (specific version to pin the extension instance to). Must not be supplied if auto-upgrade-minor-version is set to `true`. |
| `--configuration-settings` | Settings that can be passed into the extension to control its functionality. Only the settings that require an update need to be provided. The provided settings would be replaced with the provided values. They are to be passed in as space separated `key=value` pairs after the parameter name. If this parameter is used in the command, then `--configuration-settings-file` can't be used in the same command. |
| `--configuration-settings-file` | Path to the JSON file having key value pairs to be used for passing in configuration settings to the extension. If this parameter is used in the command, then `--configuration-settings` can't be used in the same command. |
| `--configuration-protected-settings` | These settings are not retrievable using `GET` API calls or `az k8s-extension show` commands, and are thus used to pass in sensitive settings. When updating a setting, all settings are expected to be provided. If some settings are omitted, those settings would be considered obsolete and deleted. They are to be passed in as space separated `key=value` pairs after the parameter name. If this parameter is used in the command, then `--configuration-protected-settings-file` can't be used in the same command. |
| `--configuration-protected-settings-file` | Path to the JSON file having key value pairs to be used for passing in sensitive settings to the extension. If this parameter is used in the command, then `--configuration-protected-settings` can't be used in the same command. |
| `--scope` | Scope of installation for the extension - `cluster` or `namespace` |
| `--release-train` |  Extension  authors can publish versions in different release trains such as `Stable`, `Preview`, etc. If this parameter is not set explicitly, `Stable` is used as default. This parameter can't be used when `autoUpgradeMinorVersion` parameter is set to `false`. |

### Delete extension instance

>[!NOTE]
> The Azure resource representing this extension gets deleted immediately. The Helm release on the cluster associated with this extension is only deleted when the agents running on the Kubernetes cluster have network connectivity and can reach out to Azure services again to fetch the desired state.

Delete an extension instance on a cluster with `k8s-extension delete`, passing in values for the mandatory parameters.

```azurecli
az k8s-extension delete --name azureml --cluster-name <clusterName> --resource-group <resourceGroupName> --cluster-type managedClusters
```

<!-- LINKS -->
<!-- INTERNAL -->
[arc-k8s-extensions]: ../azure-arc/kubernetes/conceptual-extensions.md
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[azure-ml-overview]: ../machine-learning/how-to-attach-arc-kubernetes.md
[dapr-overview]: ./dapr.md
[gitops-overview]: ../azure-arc/kubernetes/conceptual-gitops-flux2.md
[k8s-extension-reference]: /cli/azure/k8s-extension

<!-- EXTERNAL -->
[arc-k8s-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc&regions=all