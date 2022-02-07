---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 08/26/2021
ms.author: larryfr
---

When using Azure Machine Learning __compute instance__ (with a public IP) or __compute cluster__, allow inbound traffic from Azure Batch management and Azure Machine Learning services. Compute instance with no public IP (preview) does not require this inbound communication. A Network Security Group allowing this traffic is dynamically created for you, however you may need to also create user-defined routes (UDR) if you have a firewall. When creating a UDR for this traffic, you can use either **IP Addresses** or **service tags** to route the traffic.

> [!IMPORTANT]
> Using service tags with user-defined routes is currently in preview and may not be fully supported. For more information, see [Virtual Network routing](../articles/virtual-network/virtual-networks-udr-overview.md#service-tags-for-user-defined-routes-preview).

> [!TIP]
> While a compute instance without a public IP (a preview feature) does not need a UDR for this inbound traffic, you will still need these UDRs if you also use a compute cluster or a compute instance with a public IP.


# [IP Address routes](#tab/ipaddress)

For the Azure Machine Learning service, you must add the IP address of both the __primary__ and __secondary__ regions. To find the secondary region, see the [Cross-region replication in Azure](../articles/availability-zones/cross-region-replication-azure.md). For example, if your Azure Machine Learning service is in East US 2, the secondary region is Central US. 

To get a list of IP addresses of the Batch service and Azure Machine Learning service, download the [Azure IP Ranges and Service Tags](https://www.microsoft.com/download/details.aspx?id=56519) and search the file for `BatchNodeManagement.<region>` and `AzureMachineLearning.<region>`, where `<region>` is your Azure region.

> [!IMPORTANT]
> The IP addresses may change over time.

When creating the UDR, set the __Next hop type__ to __Internet__. The following image shows an example IP address based UDR in the Azure portal:

:::image type="content" source="./media/machine-learning-compute-user-defined-routes/user-defined-route.png" alt-text="Image of a user-defined route configuration":::

# [Service tag (preview) routes](#tab/servicetag)

Create user-defined routes for the following service tags:

* `AzureMachineLearning`
* `BatchNodeManagement.<region>`, where `<region>` is your Azure region.

The following commands demonstrate adding routes for these service tags:

```azurecli
az network route-table route create -g MyResourceGroup --route-table-name MyRouteTable -n AzureMLRoute --address-prefix AzureMachineLearning --next-hop-type Internet
az network route-table route create -g MyResourceGroup --route-table-name MyRouteTable -n BatchRoute --address-prefix BatchNodeManagement.westus2 --next-hop-type Internet
```

---

For information on configuring UDR, see [Route network traffic with a routing table](../articles/virtual-network/tutorial-create-route-table-portal.md).