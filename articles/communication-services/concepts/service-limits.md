---
title: Service limits for Azure Communication Services
titleSuffix: An Azure Communication Services how-to document
description: Learn how to
author: manoskow
manager: shahen
services: azure-communication-services

ms.author: manoskow
ms.date: 11/01/2021
ms.topic: how-to
ms.service: azure-communication-services
ms.subservice: data
---
# Service limits for Azure Communication Services

This document explains some of the limitations of Azure Communication Services and what to do if you are running into these limitations. 

## Throttling patterns and architecture
When you hit service limitations you will generally receive an HTTP status code 429 (Too many requests). In general, the following are best practices for handling throttling:

- Reduce the number of operations per request.
- Reduce the frequency of calls.
- Avoid immediate retries, because all requests accrue against your usage limits.

You can find more general guidance on how to set up your service architecture to handle throttling and limitations in the [Azure Architecture](/azure/architecture) documentation for [throttling patterns](/azure/architecture/patterns/throttling).

## Acquiring phone numbers
Before trying to acquire a phone number, make sure your subscription meets the [geographic and subscription](./telephony/plan-solution.md) requirements, otherwise you can't purchase a phone number. The below limitations apply to purchasing numbers through the [Phone Numbers SDK](./reference.md) and the [Azure portal](https://portal.azure.com/).

| Operation | Scope | Timeframe | Limit (number of requests) |
| --- | -- | -- | -- |
| Purchase phone number | Azure tenant | - | 1 |
| Search for phone numbers | Azure tenant | 1 week | 5 |

### Action to take

For more information, see the [phone number types](./telephony/plan-solution.md) concept page and the [telephony concept](./telephony/telephony-concept.md) overview page.

If you would like to purchase more phone numbers or put in a special order, follow the [instructions here](https://github.com/Azure/Communication/blob/master/special-order-numbers.md). If you would like to port toll-free phone numbers from external accounts to their Azure Communication Services account follow the [instructions here](https://github.com/Azure/Communication/blob/master/port-numbers.md).

## Identity

| Operation | Timeframes (seconds) | Limit (number of requests) |
|---|--|--|
| **Create identity** | 30 | 500|
| **Delete identity** | 30 | 500|
| **Issue access token** | 30 | 500|
| **Revoke access token**  | 1 | 100|
| **createUserAndToken**| 30 | 1000 |
| **exchangeTokens**| 30 | 500 |

### Action to take
We always recommend you acquire identities and tokens in advance of starting other transactions like creating chat threads or starting calls, for example, right when your webpage is initially loaded or when the app is starting up. 

For more information, see the [identity concept overview](./authentication.md) page.

## SMS
When sending or receiving a high volume of messages, you might receive a ```429``` error. This indicates you are hitting the service limitations and your messages will be queued to be sent once the number of requests is below the threshold.

Rate Limits for SMS:

|Operation|Scope|Timeframe (seconds)| Limit (number of requests) | Message units per minute|
|---------|-----|-------------|-------------------|-------------------------|
|Send Message|Per Number|60|200|200|

### Action to take
If you require sending an amount of messages that exceeds the rate-limits, please email us at phone@microsoft.com.

For more information on the SMS SDK and service, see the [SMS SDK overview](./sms/sdk-features.md) page or the [SMS FAQ](./sms/sms-faq.md) page.

## Chat

### Size Limits

| **Name**         | Limit  |
|--|--|
|Number of participants in thread|250 |
|Batch of participants - CreateThread|200 |
|Batch of participants - AddParticipant|200 |

## Voice and video calling

### Call maximum limitations

| **Name**         | Limit  |
|--|--|
|Number of participants | 350 

### Calling SDK streaming support
The Communication Services Calling SDK supports the following streaming configurations:

| Limit                                                         | Web                         | Windows/Android/iOS        |
| ------------------------------------------------------------- | --------------------------- | -------------------------- |
| **Maximum # of outgoing local streams that can be sent simultaneously**     | 1 video or 1 screen sharing | 1 video + 1 screen sharing |
| **Maximum # of incoming remote streams that can be rendered simultaneously** | 4 videos + 1 screen sharing | 6 videos + 1 screen sharing |

While the Calling SDK won't enforce these limits, your users may experience performance degradation if they're exceeded.

### Calling SDK timeouts

The following timeouts apply to the Communication Services Calling SDKs:

| Action                                                                      | Timeout in seconds |
| --------------------------------------------------------------------------- | ------------------ |
| Reconnect/removal participant                                               | 120                |
| Add or remove new modality from a call (Start/stop video or screen sharing) | 40                 |
| Call Transfer operation timeout                                             | 60                 |
| 1:1 call establishment timeout                                              | 85                 |
| Group call establishment timeout                                            | 85                 |
| PSTN call establishment timeout                                             | 115                |
| Promote 1:1 call to a group call timeout                                    | 115                |


### Action to take

For more information about the voice and video calling SDK and service, see the [calling SDK overview](./voice-video-calling/calling-sdk-features.md) page or [known issues](./known-issues.md).

## Teams Interoperability and Microsoft Graph
If you are using a Teams interoperability scenario, you will likely end up using some Microsoft Graph APIs to create [meetings](/graph/cloud-communications-online-meetings).  

Each service offered through Microsoft Graph has different limitations; service-specific limits are [described here](/graph/throttling) in more detail.

### Action to take
When you implement error handling, use the HTTP error code 429 to detect throttling. The failed response includes the ```Retry-After``` response header. Backing off requests using the ```Retry-After``` delay is the fastest way to recover from throttling because Microsoft Graph continues to log resource usage while a client is being throttled.

You can find more information on Microsoft Graph [throttling](/graph/throttling) limits in the [Microsoft Graph](/graph/overview) documentation.

## Network Traversal

| Operation | Timeframes (seconds) | Limit (number of requests) |
|---|--|--|
| **Issue TURN Credentials** | 5 | 30000|
| **Issue Relay Configuration** | 5 | 30000|

### Action to take
We always recommend you acquire tokens in advance of starting other transactions like creating a relay connection. 

For more information, see the [network traversal concept overview](./network-traversal.md) page.

## Still need help?
See the [help and support](../support.md) options available to you.

## Next steps
