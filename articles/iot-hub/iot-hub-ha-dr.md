---
title: Azure IoT Hub high availability and disaster recovery | Microsoft Docs
description: Describes the Azure and IoT Hub features that help you to build highly available Azure IoT solutions with disaster recovery capabilities.
author: eross-msft
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 11/22/2021
ms.author: lizross
---

# IoT Hub high availability and disaster recovery

As a first step towards implementing a resilient IoT solution, architects, developers, and business owners must define the uptime goals for the solutions they're building. These goals can be defined primarily based on specific business objectives for each scenario. In this context, the article [Azure Business Continuity Technical Guidance](/azure/architecture/framework/resiliency/app-design) describes a general framework to help you think about business continuity and disaster recovery. The [Disaster recovery and high availability for Azure applications](/azure/architecture/reliability/disaster-recovery) paper provides architecture guidance on strategies for Azure applications to achieve High Availability (HA) and Disaster Recovery (DR).

This article discusses the HA and DR features offered specifically by the IoT Hub service. The broad areas discussed in this article are:

- Intra-region HA
- Cross region DR
- Achieving cross region HA

Depending on the uptime goals you define for your IoT solutions, you should determine which of the options outlined below best suit your business objectives. Incorporating any of these HA/DR alternatives into your IoT solution requires a careful evaluation of the trade-offs between the:

- Level of resiliency you require 
- Implementation and maintenance complexity
- COGS impact

## Intra-region HA

The IoT Hub service provides intra-region HA by implementing redundancies in almost all layers of the service. The [SLA published by the IoT Hub service](https://azure.microsoft.com/support/legal/sla/iot-hub) is achieved by making use of these redundancies. No additional work is required by the developers of an IoT solution to take advantage of these HA features. Although IoT Hub offers a reasonably high uptime guarantee, transient failures can still be expected as with any distributed computing platform. If you're just getting started with migrating your solutions to the cloud from an on-premises solution, your focus needs to shift from optimizing "mean time between failures" to "mean time to recover". In other words, transient failures are to be considered normal while operating with the cloud in the mix. Appropriate [retry policies](iot-hub-reliability-features-in-sdks.md) must be built in to the components interacting with a cloud application to deal with transient failures.

## Availability Zones

There is a 99.9% Service Level Agreement for IoT Hub, and you can [read the SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/). The full [Azure SLA](https://azure.microsoft.com/support/legal/sla/) explains the guaranteed availability of Azure as a whole.

IoT Hub supports [Availability Zones](../availability-zones/az-overview.md). An Availability Zone is a high-availability offering that protects your applications and data from datacenter failures. A region with Availability Zone support is comprised of three zones supporting that region. Each zone provides one or more datacenters each in a unique physical location with independent power, cooling, and networking. This provides replication and redundancy within the region. Availability Zone support for IoT Hub is enabled automatically for new IoT Hub resources created in the following Azure regions:

- Australia East
- Brazil South
- Canada Central
- Central US
- France Central
- West Us 2
- Japan East
- North Europe
- Southeast Asia
- UK South

## Cross region DR

There could be some rare situations when a datacenter experiences extended outages due to power failures or other failures involving physical assets. Such events are rare during which the intra region HA capability described above may not always help. IoT Hub provides multiple solutions for recovering from such extended outages. 

The recovery options available to customers in such a situation are [Microsoft-initiated failover](#microsoft-initiated-failover) and [manual failover](#manual-failover). The fundamental difference between the two is that Microsoft initiates the former and the user initiates the latter. Also, manual failover provides a lower recovery time objective (RTO) compared to the Microsoft-initiated failover option. The specific RTOs offered with each option are discussed in the sections below. When either of these options to perform failover of an IoT hub from its primary region is exercised, the hub becomes fully functional in the corresponding [Azure geo-paired region](../best-practices-availability-paired-regions.md).

Both these failover options offer the following recovery point objectives (RPOs):

| Data type | Recovery point objectives (RPO) |
| --- | --- |
| Identity registry |0-5 mins data loss |
| Device twin data |0-5 mins data loss |
| Cloud-to-device messages<sup>1</sup> |0-5 mins data loss |
| Parent<sup>1</sup> and device jobs |0-5 mins data loss |
| Device-to-cloud messages |All unread messages are lost |
| Operations monitoring messages |All unread messages are lost |
| Cloud-to-device feedback messages |All unread messages are lost |

<sup>1</sup>Cloud-to-device messages and parent jobs do not get recovered as a part of manual failover.

Once the failover operation for the IoT hub completes, all operations from the device and back-end applications are expected to continue working without requiring a manual intervention. This means that your device-to-cloud messages should continue to work, and the entire device registry is intact. Events emitted via Event Grid can be consumed via the same subscription(s) configured earlier as long as those Event Grid subscriptions continue to be available. No additional handling is required for custom endpoints.

> [!CAUTION]
> - The Event Hub-compatible name and endpoint of the IoT Hub built-in Events endpoint change after failover. When receiving telemetry messages from the built-in endpoint using either the Event Hub client or event processor host, you should [use the IoT hub connection string](iot-hub-devguide-messages-read-builtin.md#read-from-the-built-in-endpoint) to establish the connection. This ensures that your back-end applications continue to work without requiring manual intervention post failover. If you use the Event Hub-compatible name and endpoint in your application directly, you will need to [fetch the new Event Hub-compatible endpoint](iot-hub-devguide-messages-read-builtin.md#read-from-the-built-in-endpoint) after failover to continue operations. For more information, see [Manual failover and Event Hub](#manual-failover-and-event-hub).
>
> - If you use Azure Functions or Azure Stream Analytics to connect the built-in Events endpoint, you might need to perform a **Restart**. This is because during failover previous offsets are no longer valid.
>
> - When routing to storage, we recommend listing the blobs or files and then iterating over them, to ensure all blobs or files are read without making any assumptions of partition. The partition range could potentially change during a Microsoft-initiated failover or manual failover. You can use the [List Blobs API](/rest/api/storageservices/list-blobs) to enumerate the list of blobs or [List ADLS Gen2 API](/rest/api/storageservices/datalakestoragegen2/filesystem/list) for the list of files. To learn more, see [Azure Storage as a routing endpoint](iot-hub-devguide-messages-d2c.md#azure-storage-as-a-routing-endpoint).

## Microsoft-initiated failover

Microsoft-initiated failover is exercised by Microsoft in rare situations to failover all the IoT hubs from an affected region to the corresponding geo-paired region. This process is a default option (no way for users to opt out) and requires no intervention from the user. Microsoft reserves the right to make a determination of when this option will be exercised. This mechanism doesn't involve a user consent before the user's hub is failed over. Microsoft-initiated failover has a recovery time objective (RTO) of 2-26 hours. 

The large RTO is because Microsoft must perform the failover operation on behalf of all the affected customers in that region. If you are running a less critical IoT solution that can sustain a downtime of roughly a day, it is ok for you to take a dependency on this option to satisfy the overall disaster recovery goals for your IoT solution. The total time for runtime operations to become fully operational once this process is triggered, is described in the "Time to recover" section.

## Manual failover

If your business uptime goals aren't satisfied by the RTO that Microsoft initiated failover provides, consider using manual failover to trigger the failover process yourself. The RTO using this option could be anywhere between 10 minutes to a couple of hours. The RTO is currently a function of the number of devices registered against the IoT hub instance being failed over. You can expect the RTO for a hub hosting approximately 100,000 devices to be in the ballpark of 15 minutes. The total time for runtime operations to become fully operational once this process is triggered, is described in the "Time to recover" section.

The manual failover option is always available for use irrespective of whether the primary region is experiencing downtime or not. Therefore, this option could potentially be used to perform planned failovers. One example usage of planned failovers is to perform periodic failover drills. A word of caution though is that a planned failover operation results in a downtime for the hub for the period defined by the RTO for this option, and also results in a data loss as defined by the RPO table above. You could consider setting up a test IoT hub instance to exercise the planned failover option periodically to gain confidence in your ability to get your end-to-end solutions up and running when a real disaster happens.

Manual failover is available at no additional cost for IoT hubs created after May 18, 2017

For step-by-step instructions, see [Tutorial: Perform manual failover for an IoT hub](tutorial-manual-failover.md)

## Manual failover and Event Hub

As noted previously in the **Caution** section, the Event Hub-compatible name and endpoint of the IoT Hub built-in Events endpoint change after manual failover. This is because the Event Hub client doesn't have visibility into IoT Hub events. The same is true for other cloud-based clients such as Functions and Azure Stream Analytics. To retrieve the endpoint and name, you can use the Azure portal or leverage an included sample.

### Use the portal

For more information about using the portal to retrieve the Event Hub-compatible endpoint and the Event Hub-compatible name, see [Read from the built-in endpoint](iot-hub-devguide-messages-read-builtin.md#read-from-the-built-in-endpoint).

### Use the included sample

To use the IoT Hub connection string to recapture the Event Hub-compatible endpoint, leverage a sample located at [https://github.com/Azure/azure-sdk-for-net/tree/main/samples/iothub-connect-to-eventhubs](https://github.com/Azure/azure-sdk-for-net/tree/main/samples/iothub-connect-to-eventhubs) that shows how to use the IoT Hub connection string to recapture the EventHub compatible endpoint. The code example uses the connection string to get the new Event Hub endpoint and re-establish the connection. You must have Visual Studio installed.

### Running test drills

Test drills should not be performed on IoT hubs that are being used in your production environments.

### Don't use manual failover to migrate IoT hub to a different region

Manual failover should *not* be used as a mechanism to permanently migrate your hub between the Azure geo paired regions. Doing so increases latency for the operations being performed against the IoT hub from devices homed in the old primary region.

## Failback

Failing back to the old primary region can be achieved by triggering the failover action another time. If the original failover operation was performed to recover from an extended outage in the original primary region, we recommended that the hub should be failed back to the original location once that location has recovered from the outage situation.

> [!IMPORTANT]
> - Users are only allowed to perform 2 successful failover and 2 successful failback operations per day.
>
> - Back to back failover/failback operations are not allowed. You must wait for 1 hour between these operations.

## Time to recover

While the FQDN (and therefore the connection string) of the IoT hub instance remains the same post failover, the underlying IP address changes. Therefore the overall time for the runtime operations being performed against your IoT hub instance to become fully operational after the failover process is triggered can be expressed using the following function.

Time to recover = RTO [10 min - 2 hours for manual failover | 2 - 26 hours for Microsoft-initiated failover] + DNS propagation delay + Time taken by the client application to refresh any cached IoT Hub IP address.

> [!IMPORTANT]
> The IoT SDKs do not cache the IP address of the IoT hub. We recommend that user code interfacing with the SDKs should not cache the IP address of the IoT hub.

## Achieve cross region HA

If your business uptime goals aren't satisfied by the RTO that either Microsoft-initiated failover or manual failover options provide, you should consider implementing a per-device automatic cross region failover mechanism.
A complete treatment of deployment topologies in IoT solutions is outside the scope of this article. The article discusses the *regional failover* deployment model for the purpose of high availability and disaster recovery.

In a regional failover model, the solution back end runs primarily in one datacenter location. A secondary IoT hub and back end are deployed in another datacenter location. If the IoT hub in the primary region suffers an outage or the network connectivity from the device to the primary region is interrupted, devices use a secondary service endpoint. You can improve the solution availability by implementing a cross-region failover model instead of staying within a single region. 

At a high level, to implement a regional failover model with IoT Hub, you need to take the following steps:

* **A secondary IoT hub and device routing logic**: If service in your primary region is disrupted, devices must start connecting to your secondary region. Given the state-aware nature of most services involved, it's common for solution administrators to trigger the inter-region failover process. The best way to communicate the new endpoint to devices, while maintaining control of the process, is to have them regularly check a *concierge* service for the current active endpoint. The concierge service can be a web application that is replicated and kept reachable using DNS-redirection techniques (for example, using [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)).

   > [!NOTE]
   > IoT hub service is not a supported endpoint type in Azure Traffic Manager. The recommendation is to integrate the proposed concierge service with Azure traffic manager by making it implement the endpoint health probe API.

* **Identity registry replication**: To be usable, the secondary IoT hub must contain all device identities that can connect to the solution. The solution should keep geo-replicated backups of device identities, and upload them to the secondary IoT hub before switching the active endpoint for the devices. The device identity export functionality of IoT Hub is useful in this context. For more information, see [IoT Hub developer guide - identity registry](iot-hub-devguide-identity-registry.md).

* **Merging logic**: When the primary region becomes available again, all the state and data that have been created in the secondary site must be migrated back to the primary region. This state and data mostly relate to device identities and application metadata, which must be merged with the primary IoT hub and any other application-specific stores in the primary region. 

To simplify this step, you should use idempotent operations. Idempotent operations minimize the side-effects from the eventual consistent distribution of events, and from duplicates or out-of-order delivery of events. In addition, the application logic should be designed to tolerate potential inconsistencies or slightly out-of-date state. This situation can occur due to the additional time it takes for the system to heal based on recovery point objectives (RPO).

## Choose the right HA/DR option

Here's a summary of the HA/DR options presented in this article that can be used as a frame of reference to choose the right option that works for your solution.

| HA/DR option | RTO | RPO | Requires manual intervention? | Implementation complexity | Additional cost impact|
| --- | --- | --- | --- | --- | --- |
| Microsoft-initiated failover |2 - 26 hours|Refer RPO table above|No|None|None|
| Manual failover |10 min - 2 hours|Refer RPO table above|Yes|Very low. You only need to trigger this operation from the portal.|None|
| Cross region HA |< 1 min|Depends on the replication frequency of your custom HA solution|No|High|> 1x the cost of 1 IoT hub|

## Next steps

* [What is Azure IoT Hub?](about-iot-hub.md)
* [Get started with IoT Hubs (Quickstart)](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-csharp)
* [Tutorial: Perform manual failover for an IoT hub](tutorial-manual-failover.md)
