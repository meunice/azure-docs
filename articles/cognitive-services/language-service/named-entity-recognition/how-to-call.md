---
title: How to perform Named Entity Recognition (NER)
titleSuffix: Azure Cognitive Services
description: This article will show you how to extract named entities from text.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: conceptual
ms.date: 12/10/2021
ms.author: aahi
ms.custom: language-service-ner, ignite-fall-2021
---


# How to use Named Entity Recognition(NER)

The NER feature can evaluate unstructured text, and extract named entities from text in several pre-defined categories, for example: person, location, event, product, and organization.  

## Determine how to process the data (optional)

### Specify the NER model

By default, this feature will use the latest available AI model on your text. You can also configure your API requests to use a specific [model version](../concepts/model-lifecycle.md).


### Input languages

When you submit documents to be processed, you can specify which of [the supported languages](language-support.md) they're written in. if you don't specify a language, key phrase extraction will default to English. The API may return offsets in the response to support different [multilingual and emoji encodings](../concepts/multilingual-emoji-support.md). 

## Submitting data

Analysis is performed upon receipt of the request. For information on the size and number of requests you can send per minute and second, see the data limits section below.

Using the NER feature synchronously is stateless. No data is stored in your account, and results are returned immediately in the response.

[!INCLUDE [asynchronous-result-availability](../includes/async-result-availability.md)]

The API will attempt to detect the [defined entity categories](concepts/named-entity-categories.md) for a given document language. 

## Getting NER results

When you get results from NER, you can stream the results to an application or save the output to a file on the local system. The API response will include [recognized entities](concepts/named-entity-categories.md), including their categories and sub-categories, and confidence scores. 

## Data limits

> [!NOTE]
> * If you need to analyze larger documents than the limit allows, you can break the text into smaller chunks of text before sending them to the API. 
> * A document is a single string of text characters.  

| Limit | Value |
|------------------------|---------------|
| Maximum size of a single document (synchronous) | 5,120 characters as measured by [StringInfo.LengthInTextElements](/dotnet/api/system.globalization.stringinfo.lengthintextelements).  |
| Maximum number of characters per request (asynchronous)  | 125K characters across all submitted documents, as measured by [StringInfo.LengthInTextElements](/dotnet/api/system.globalization.stringinfo.lengthintextelements).  |
| Maximum size of entire request | 1 MB. |
| Max documents per request | 5 |

If a document exceeds the character limit, the API will behave differently depending on the feature you're using:

* Asynchronous:
  * The API will reject the entire request and return a `400 bad request` error if any document within it exceeds the maximum size.
* Synchronous:  
  * The API won't process a document that exceeds the maximum size, and will return an invalid document error for it. If an API request has multiple documents, the API will continue processing them if they are within the character limit.

Exceeding the maximum number of documents you can send in a single request will generate an HTTP 400 error code.

### Rate limits

Your rate limit will vary with your [pricing tier](https://aka.ms/unifiedLanguagePricing).

| Tier          | Requests per second | Requests per minute |
|---------------|---------------------|---------------------|
| S / Multi-service | 1000                | 1000                |
| S0 / F0         | 100                 | 300                 |

## Next steps

[Named Entity Recognition overview](overview.md)
