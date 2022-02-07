---
title: Configure Request and Response Buffers
description: Learn how to configure Request and Response buffers for your Azure Application Gateway.
services: application-gateway
author: jaesoni
ms.service: application-gateway
ms.topic: how-to
ms.date: 12/01/2021
ms.author: jaysoni
#Customer intent: As a user, I want to know how can I disable/enable proxy buffers.
---

# Configure Request and Response Proxy Buffers

Azure Application Gateway Standard v2 and WAF v2 SKUs support buffering Requests (from clients) or Responses (from the backend servers). Based on the processing capabilities of the clients that interact with your Application Gateway, you can use these buffers to configure the speed of packet delivery.
 
## Response Buffer 

Application Gateway's Response buffer can collect all or parts of the response packets sent by the backend server, before delivering them to the clients. By default, the Response buffering is enabled on Application Gateway which is useful to accommodate slow clients. This setting allows you to conserve the backend TCP connections as they can be closed once Application Gateway receives complete response and work according to the client's processing speed. This way, your Application Gateway will continue to deliver the response as per client’s pace. 

 
## Request Buffer 

In a similar way, Application Gateway's Request buffer can temporarily store the entire or parts of the request body, and then forward a larger upload request at once to the backend server. By default, Request buffering setting is enabled on Application Gateway and is useful to offload the processing function of re-assembling the smaller packets of data on the backend server.


>[!NOTE]
>By default, both Request and Response buffers are enabled on your Application Gateway resource but you can choose to configure them separately. Further, the settings are applied at a resource level and cannot be managed separately for each listener.
 
</br>

You can keep either the Request or Response buffer, enabled or disable, based on your requirements and/or the observed performance of the client systems that communicate with your Application Gateway. 

</br>

> [!WARNING]
>We strongly recommend that you test and evaluate the performance before rolling this out on the production gateways. 

## How to change the buffer setting? 

You can change this setting by using GlobalConfiguration in the ARM template as shown below.

```json
{
   "$schema":"https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
   "contentVersion":"1.0.0.0",
   "parameters":{      
   },
   "variables":{      
   },
   "resources":[
      {
         "type":"Microsoft.Network/applicationGateways",
         "apiVersion":"xxx-xx-xx",
         "name":"[parameters('applicationGateways_xxxx_x_xx_name')]",
         "location":"eastus",
         "tags":{            
         },
         "identity":{      
         },
         "properties":{
            "globalConfiguration":{
               "enableRequestBuffering":false,
               "enableResponseBuffering":false
            }
         }
      }
   ]
} 
```
For reference, visit [Azure SDK for .NET](/dotnet/api/microsoft.azure.management.network.models.applicationgatewayglobalconfiguration)
