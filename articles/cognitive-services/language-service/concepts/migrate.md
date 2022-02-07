---
title: "Migrate to Azure Cognitive Service for Language from: LUIS, QnA Maker, and Text Analytics"
titleSuffix: Azure Cognitive Services
description: Use this article to learn if you need to migrate your applications from LUIS, QnA Maker, and Text Analytics.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: conceptual
ms.date: 12/03/2021
ms.author: aahi
ms.custom: ignite-fall-2021
---

# Migrating to Azure Cognitive Service for Language

On November 2nd 2021, Azure Cognitive Service for Language was released into public preview. This language service unifies the Text Analytics, QnA Maker, and LUIS service offerings, and provides several new features as well. 

## Do I need to migrate to the language service if I am using Text Analytics?

Text Analytics has been incorporated into the language service, and its features are still available. If you were using Text Analytics, your applications should continue to work without breaking changes. You can also see the [Text Analytics migration guide](migrate-language-service-latest.md), if you need to update an older application. 

Consider using one of the available quickstart articles to see the latest information on service endpoints, and API calls. 

## How do I migrate to the language service if I am using LUIS?

If you're using Language Understanding (LUIS), you can [import your LUIS JSON file](../conversational-language-understanding/concepts/backwards-compatibility.md) to the new Conversational language understanding feature. 

## How do I migrate to the language service if I am using QnA Maker?

If you're using QnA Maker, see the [migration guide](../question-answering/how-to/migrate-qnamaker.md) for information on migrating knowledge bases from QnA Maker to question answering.

## See also

* [Azure Cognitive Service for Language overview](../overview.md)
* [Conversational language understanding overview](../conversational-language-understanding/overview.md)
* [Question answering overview](../question-answering/overview.md)
