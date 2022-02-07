---
title: Safe rollout for online endpoints 
titleSuffix: Azure Machine Learning
description: Roll out newer versions of ML models without disruption.
services: machine-learning
ms.service: machine-learning
ms.subservice: mlops
ms.author: seramasu
ms.reviewer: laobri
author: rsethur
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: how-to, devplatv2
---

# Safe rollout for online endpoints (preview)

You've an existing model deployed in production and you want to deploy a new version of the model. How do you roll out your new ML model without causing any disruption? A good answer is blue-green deployment, an approach in which a new version of a web service is introduced to production by rolling out the change to a small subset of users/requests before rolling it out completely. This article assumes you're using online endpoints; for more information, see [What are Azure Machine Learning endpoints (preview)?](concept-endpoints.md).

In this article, you'll learn to:

> [!div class="checklist"]
> * Deploy a new online endpoint called "blue" that serves version 1 of the model
> * Scale this deployment so that it can handle more requests
> * Deploy version 2 of the model to an endpoint called "green" that accepts no live traffic
> * Test the green deployment in isolation 
> * Send 10% of live traffic to the green deployment
> * Fully cut-over all live traffic to the green deployment
> * Delete the now-unused v1 blue deployment

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

## Prerequisites

* To use Azure machine learning, you must have an Azure subscription. If you don't have an Azure subscription, create a free account before you begin. Try the [free or paid version of Azure Machine Learning](https://azure.microsoft.com/free/) today.

* You must install and configure the Azure CLI and ML extension. For more information, see [Install, set up, and use the CLI (v2) (preview)](how-to-configure-cli.md). 

* You must have an Azure Resource group, in which you (or the service principal you use) need to have `Contributor` access. You'll have such a resource group if you configured your ML extension per the above article. 

* You must have an Azure Machine Learning workspace. You'll have such a workspace if you configured your ML extension per the above article.

* If you've not already set the defaults for Azure CLI, you should save your default settings. To avoid having to repeatedly pass in the values, run:

   ```azurecli
   az account set --subscription <subscription id>
   az configure --defaults workspace=<azureml workspace name> group=<resource group>
   ```

* An existing online endpoint and deployment. This article assumes that your deployment is as described in [Deploy and score a machine learning model with an online endpoint (preview)](how-to-deploy-managed-online-endpoints.md).

* If you haven't already set the environment variable $ENDPOINT_NAME, do so now:

   :::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="set_endpoint_name":::

* (Recommended) Clone the samples repository and switch to the repository's `cli/` directory: 

   ```azurecli
   git clone https://github.com/Azure/azureml-examples
   cd azureml-examples/cli
   ```

The commands in this tutorial are in the file `deploy-safe-rollout-online-endpoints.sh` and the YAML configuration files are in the `endpoints/online/managed/sample/` subdirectory.

## Confirm your existing deployment is created

You can view the status of your existing endpoint and deployment by running: 

```azurecli
az ml online-endpoint show --name $ENDPOINT_NAME 

az ml online-deployment show --name blue --endpoint $ENDPOINT_NAME 
```

You should see the endpoint identified by `$ENDPOINT_NAME` and, a deployment called `blue`. 

## Scale your existing deployment to handle more traffic

In the deployment described in [Deploy and score a machine learning model with an online endpoint (preview)](how-to-deploy-managed-online-endpoints.md), you set the `instance_count` to the value `1` in the deployment yaml file. You can scale out using the `update` command :

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="scale_blue" :::

> [!Note]
> Notice that in the above command we use `--set` to override the deployment configuration. Alternatively you can update the yaml file and pass it as an input to the `update` command using the `--file` input.

## Deploy a new model, but send it no traffic yet

Create a new deployment named `green`: 

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="create_green" :::

Since we haven't explicitly allocated any traffic to green, it will have zero traffic allocated to it. You can verify that using the command:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="get_traffic" :::

### Test the new deployment

Though `green` has 0% of traffic allocated, you can invoke it directly by specifying the `--deployment` name:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="test_green" :::

If you want to use a REST client to invoke the deployment directly without going through traffic rules, set the following HTTP header: `azureml-model-deployment: <deployment-name>`. The below code snippet uses `curl` to invoke the deployment directly. The code snippet should work in Unix/WSL environments:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="test_green_using_curl" :::

## Test the new deployment with a small percentage of live traffic

Once you've tested your `green` deployment, allocate a small percentage of traffic to it:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="green_10pct_traffic" :::

Now, your `green` deployment will receive 10% of requests. 

## Send all traffic to your new deployment

Once you're satisfied that your `green` deployment is fully satisfactory, switch all traffic to it.

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="green_100pct_traffic" :::

## Remove the old deployment

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="delete_blue" :::

## Delete the endpoint and deployment

If you aren't going use the deployment, you should delete it with:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-safe-rollout-online-endpoints.sh" ID="delete_endpoint" :::


## Next steps
- [Deploy models with REST (preview)](how-to-deploy-with-rest.md)
- [Create and use online endpoints (preview) in the studio](how-to-use-managed-online-endpoint-studio.md)
- [Access Azure resources with a online endpoint and managed identity (preview)](how-to-access-resources-from-endpoints-managed-identities.md)
- [Monitor managed online endpoints (preview)](how-to-monitor-online-endpoints.md)
- [Manage and increase quotas for resources with Azure Machine Learning](how-to-manage-quotas.md#azure-machine-learning-managed-online-endpoints-preview)
- [View costs for an Azure Machine Learning managed online endpoint (preview)](how-to-view-online-endpoints-costs.md)
- [Managed online endpoints SKU list (preview)](reference-managed-online-endpoints-vm-sku-list.md)
- [Troubleshooting  online endpoints deployment and scoring (preview)](how-to-troubleshoot-managed-online-endpoints.md)
- [Managed online endpoints (preview) YAML reference](reference-yaml-endpoint-managed-online.md)
