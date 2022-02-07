---
title: Azure direct routing certified Session Border Controllers - Azure Communication Services
description: The list of Session Border Controllers certified for Azure Communication Services direct routing, and known limitations
author: boris-bazilevskiy
manager: nmurav
services: azure-communication-services

ms.author: bobazile
ms.date: 06/30/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.subservice: pstn
---
# List of Session Border Controllers certified for Azure Communication Services direct routing
This document contains a list of Session Border Controllers certified for Azure Communication Services direct routing. It also includes known limitations.

Microsoft is working with the selected Session Border Controllers (SBC) vendors certified for Teams Direct Routing to work with Azure direct routing. You can watch the progress on this page. While the SBC, certified for Teams Direct Routing, can work with Azure direct routing, we encourage not to put any workload on the SBC until it appears on this page. We also do not support the uncertified SBC. While Azure direct routing is built on the same backend as Teams Direct Routing, there are some differences. The certification covers comprehensive validation of the SBC for the Azure direct routing.

Microsoft works with each vendor to:
- Jointly work on the SIP interconnection protocols.
- Perform intense tests using a third-party lab. Only devices that pass the tests are certified.
- Run daily tests with all certified devices in production and pre-production environments. Validating the devices in pre-production environments guarantees that new versions of Azure Communication Services code in the cloud will work with certified SBCs.
- Establish a joint support process with the SBC vendors.

[!INCLUDE [Public Preview](../../includes/public-preview-include-document.md)]

Media bypass is not yet supported by Azure Communication Services. 
Early media is not supported by a web-based client.
The table that follows list devices certified for Azure Communication Services direct routing.

If you have any questions about the SBC certification program for Communication Services direct routing, contact acsdrcertification@microsoft.com.

## Certified SBC vendors

|Vendor|Product|Software version|
|:--- |:--- |:--- 
|[AudioCodes](https://www.audiocodes.com/media/lbjfezwn/mediant-sbc-with-microsoft-azure-communication-services.pdf)|Mediant SBC|7.40A
|[Metaswitch](https://manuals.metaswitch.com/Perimeta/V4.9/AzureCommunicationServicesIntegrationGuide/Source/notices.html)|Perimeta SBC|4.9|
|[Oracle](https://www.oracle.com/technical-resources/documentation/acme-packet.html)|Oracle Acme Packet SBC|8.4|
|Ribbon Communications|[SBC SWe / SBC 5400 / SBC 7000](https://support.sonus.net/display/ALLDOC/Ribbon+Configurations+with+Azure+Communication+Services+Direct+Routing)|9.02|
||[SBC SWe Lite / SBC 1000 / SBC 2000](https://support.sonus.net/display/UXDOC90/Best+Practice+-+Configure+SBC+Edge+for+Azure+Communication+Services+Direct+Routing)|9.0

Note the certification granted to a major version. That means that firmware with any number in the SBC firmware following the major version is supported.

## Next steps

### Conceptual documentation

- [Phone number types in Azure Communication Services](./plan-solution.md)
- [Plan for Azure direct routing](./direct-routing-infrastructure.md)
- [Pair the Session Border Controller and configure voice routing](./direct-routing-provisioning.md)
- [Pricing](../pricing.md)

### Quickstarts

- [Call to Phone](../../quickstarts/telephony/pstn-call.md)
