---
title: Micro agent configurations (Preview)
description: The collector sends all current data immediately after any configuration change is made. The changes are then applied.
ms.date: 12/22/2021
ms.topic: conceptual
---

# Micro agent configurations (Preview)

This article describes the different types of configurations that the micro agent supports. Customers can configure the micro agent to fit the needs of their devices, and network environments.  

The micro agent's behavior is configured by a set of module twin properties. You can configure the micro agent to best suit your needs. For example, you can exclude events automatically, minimize power consumption, and reduce network bandwidth.

After any change in configuration, the collector will immediately send all unsent event data. After the data is sent, the changes will be applied, and all the collectors will restart.

> [!Note]
> Aggregation mode is supported, but it is not configurable.

## Event-based collectors configurations

These configurations include process, and network activity collectors.

| Setting Name | Setting option | Description | Default setting |
|--|--|--|--|
| **Interval** | High, Medium, or Low | Define frequency of sending. | Medium |
| **Aggregation mode** | True, or False | Whether to process event aggregation for an identical event.  | True |
| **Cache size** | cycle FIFO | The number of events collected in between the data is sent. | 256 |
| **Disable collector** | True, or False | Whether or not the collector is operational. | False |

## Trigger-based collectors configurations

These configurations include system information, and baseline collectors.

| Setting Name | Setting option | Description | Default setting |
|--|--|--|--|
| **Interval** | High, Medium, or Low | The frequency in which data is sent. | Low |
| **Disable collector** | True, or False | Whether or not the collector is operational. | False |

## Network activity collector specific settings

| Setting Name | Setting option | Description | Default setting |
|--|--|--|--|
| Devices | A list of the network devices separated by a comma. For example, "eth0,eth1" | The list of network devices (interfaces) that the agent will use to monitor the traffic. If a network device is not listed, the Network Raw events will not be recorded for the missing device.| "eth0" |

## General configuration

Define the frequency in which messages are sent for each priority level. The default values are listed below:

| Frequency | Time period (in minutes) |
|--|--|
| Low | 1440 (24 hour) |
| Medium | 120 (2 hours) |
| High | 30 (.5 hours) |

To reduce the number of messages sent to cloud, each priority should be set as a multiple of the one below it. For example, High: 60 minutes, Medium: 120 minutes, Low: 480 minutes.

The syntax for configuring the frequencies is as follows:

`"CollectorsCore_PriorityIntervals"` : `"<High>,<Medium>,<Low>"`

For example:

`"CollectorsCore_PriorityIntervals"` : `"30,120,1440"`

Please note, that you must specify all 3 values.

## Next steps

Learn about the [Micro agent event collection (Preview)](concept-event-aggregation.md).
