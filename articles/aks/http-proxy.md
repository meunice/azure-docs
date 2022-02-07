---
title: Configuring Azure Kubernetes Service (AKS) nodes with an HTTP proxy
description: Use the HTTP proxy configuration feature for Azure Kubernetes Service (AKS) nodes.
services: container-service
author: nickomang
ms.topic: article
ms.date: 09/09/2021
ms.author: nickoman
---

# HTTP proxy support in Azure Kubernetes Service (preview)

Azure Kubernetes Service (AKS) clusters, whether deployed into a managed or custom virtual network, have certain outbound dependencies necessary to function properly. Previously, in environments requiring internet access to be routed through HTTP proxies, this was a problem. Nodes had no way of bootstrapping the configuration, environment variables, and certificates necessary to access internet services.

This feature adds HTTP proxy support to AKS clusters, exposing a straightforward interface that cluster operators can use to secure AKS-required network traffic in proxy-dependent environments.

Some more complex solutions may require creating a chain of trust to establish secure communications across the network. The feature also enables installation of a trusted certificate authority onto the nodes as part of bootstrapping a cluster.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## Limitations and other details

The following scenarios are **not** supported:
- Monitoring addon
- Different proxy configurations per node pool
- Updating proxy settings post cluster creation
- User/Password authentication
- Custom CAs for API server communication
- Windows-based clusters
- Node pools using Virtual Machine Availability Sets (VMAS)

By default, *httpProxy*, *httpsProxy*, and *trustedCa* have no value.

## Prerequisites

* An Azure subscription. If you don't have an Azure subscription, you can create a [free account](https://azure.microsoft.com/free).
* [Azure CLI installed](/cli/azure/install-azure-cli).

### Install the `aks-preview` Azure CLI

You also need the *aks-preview* Azure CLI extension version 0.5.25 or later. Install the *aks-preview* Azure CLI extension by using the [az extension add][az-extension-add] command. Or install any available updates by using the [az extension update][az-extension-update] command.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview
# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### Register the `HTTPProxyConfigPreview` preview feature

To use the feature, you must also enable the `HTTPProxyConfigPreview` feature flag on your subscription.

Register the `HTTPProxyConfigPreview` feature flag by using the [az feature register][az-feature-register] command, as shown in the following example:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "HTTPProxyConfigPreview"
```

It takes a few minutes for the status to show *Registered*. Verify the registration status by using the [az feature list][az-feature-list] command:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/HTTPProxyConfigPreview')].{Name:name,State:properties.state}"
```

When ready, refresh the registration of the *Microsoft.ContainerService* resource provider by using the [az provider register][az-provider-register] command:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## Configuring an HTTP proxy using Azure CLI 

Using AKS with an HTTP proxy is done at cluster creation, using the [az aks create][az-aks-create] command and passing in configuration as a JSON file.

The schema for the config file looks like this:

```json
{
  "httpProxy": "string",
  "httpsProxy": "string",
  "noProxy": [
    "string"
  ],
  "trustedCa": "string"
}
```

`httpProxy`: A proxy URL to use for creating HTTP connections outside the cluster. The URL scheme must be `http`.
`httpsProxy`: A proxy URL to use for creating HTTPS connections outside the cluster. If this is not specified, then `httpProxy` is used for both HTTP and HTTPS connections.
`noProxy`: A list of destination domain names, domains, IP addresses or other network CIDRs to exclude proxying.
`trustedCa`: A string containing the `base64 encoded` alternative CA certificate content. For now we only support `PEM` format. Another thing to note is that, for compatibility with Go-based components that are part of the Kubernetes system, the certificate MUST support `Subject Alternative Names(SANs)` instead of the deprecated Common Name certs.

Example input:
Note the CA cert should be the base64 encoded string of the PEM format cert content.

```json
{
  "httpProxy": "http://myproxy.server.com:8080/", 
  "httpsProxy": "https://myproxy.server.com:8080/", 
  "noProxy": [
    "localhost",
    "127.0.0.1"
  ],
  "trustedCA": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUgvVENDQmVXZ0F3SUJB...b3Rpbk15RGszaWFyCkYxMFlscWNPbWVYMXVGbUtiZGkvWG9yR2xrQ29NRjNURHg4cm1wOURCaUIvCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0="
}
```

Create a file and provide values for *httpProxy*, *httpsProxy*, and *noProxy*. If your environment requires it, also provide a *trustedCa* value. Next, deploy a cluster, passing in your filename via the `http-proxy-config` flag.

```azurecli
az aks create -n $clusterName -g $resourceGroup --http-proxy-config aks-proxy-config.json
```

Your cluster will initialize with the HTTP proxy configured on the nodes.

## Configuring an HTTP proxy using Azure Resource Manager (ARM) templates

Deploying an AKS cluster with an HTTP proxy configured via ARM template is straightforward. The same schema used for CLI deployment exists in the `Microsoft.ContainerService/managedClusters` definition under properties:

```json
"properties": {
    ...,
    "httpProxyConfig": {
        "httpProxy": "string",
        "httpsProxy": "string",
        "noProxy": [
            "string"
        ],
        "trustedCa": "string"
    }
}
```

In your template, provide values for *httpProxy*, *httpsProxy*, and *noProxy*. If necessary, also provide a value for `*trustedCa*. Deploy the template, and your cluster should initialize with your HTTP proxy configured on the nodes.

## Handling CA rollover

Values for *httpProxy*, *httpsProxy*, and *noProxy* cannot be changed after cluster creation. However, to support rolling CA certs, the value for *trustedCa* can be changed and applied to the cluster with the [az aks update][az-aks-update] command.

For example, assuming a new file has been created with the base64 encoded string of the new CA cert called *aks-proxy-config-2.json*, the following action will update the cluster:

```azurecli
az aks update -n $clusterName -g $resourceGroup --http-proxy-config aks-proxy-config-2.json
```

## Next steps
- For more on the network requirements of AKS clusters, see [control egress traffic for cluster nodes in AKS][aks-egress].


<!-- LINKS - internal -->
[aks-egress]: ./limit-egress-traffic.md
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-update]: /cli/azure/aks#az_aks_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az-extension-update
