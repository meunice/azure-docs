---
title: Entity Linking cognitive skill
titleSuffix: Azure Cognitive Search
description: Extract different linked entities from text in an enrichment pipeline in Azure Cognitive Search.

manager: jennmar
author: ayokande
ms.author: aakande
ms.service: cognitive-search
ms.topic: reference
ms.date: 12/09/2021
---

# Entity Linking cognitive skill

The **Entity Linking** skill returns a list of recognized entities with links to articles in a well-known knowledge base (Wikipedia).

> [!NOTE]
> This skill is bound to the [Entity Linking](../cognitive-services/language-service/entity-linking/overview.md) machine learning models in [Azure Cognitive Services for Language](../cognitive-services/language-service/overview.md) and requires [a billable resource](cognitive-search-attach-cognitive-services.md) for transactions that exceed 20 documents per indexer per day. Execution of built-in skills is charged at the existing [Cognitive Services pay-as-you go price](https://azure.microsoft.com/pricing/details/cognitive-services/).
>

## @odata.type

Microsoft.Skills.Text.V3.EntityLinkingSkill

## Data limits

The maximum size of a record should be 50,000 characters as measured by [`String.Length`](/dotnet/api/system.string.length). If you need to break up your data before sending it to the EntityLinking skill, consider using the [Text Split skill](cognitive-search-skill-textsplit.md).

## Skill parameters

Parameter names are case-sensitive and are all optional.

| Parameter name     | Description |
|--------------------|-------------|
| `defaultLanguageCode` |    Language code of the input text. If the default language code is not specified,  English (en) will be used as the default language code. <br/> See the [full list of supported languages](../cognitive-services/language-service/entity-linking/language-support.md). |
| `minimumPrecision` | A value between 0 and 1. If the confidence score (in the `entities` output) is lower than this value, the entity is not returned. The default is 0. |
| `modelVersion` | (Optional) Specifies the [version of the model](../cognitive-services/language-service/entity-linking/how-to/call-api.md#determine-how-to-process-the-data-optional) to use when calling entity linking. It will default to the latest available when not specified. We recommend you do not specify this value unless it's necessary.|

## Skill inputs

| Input name      | Description                   |
|---------------|-------------------------------|
| `languageCode`    | A string indicating the language of the records. If this parameter is not specified, the default language code will be used to analyze the records. <br/>See the [full list of supported languages](../cognitive-services/language-service/entity-linking/language-support.md). |
| `text`          | The text to analyze.          |

## Skill outputs

| Output name      | Description                   |
|---------------|-------------------------------|
| `entities` | An array of complex types that contains the following fields: <ul><li>`"name"` (The actual entity name as it appears in the text)</li> <li>`"id"` </li> <li>`"language"` (The language of the text as determined by the skill)</li> <li>`"url"` (The linked url to this entity)</li> <li>"`bingId`" (The bingId for this linked entity)</li> <li>`"dataSource"` (The data source associated with the url) </li> <li>`"matches"` (An array of complex types that contains: `text`, `offset`, `length` and `confidenceScore`)</li></ul>|

## Sample definition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.V3.EntityLinkingSkill",
    "context": "/document",
    "defaultLanguageCode": "en", 
    "minimumPrecision": 0.5, 
    "inputs": [
        {
            "name": "text", 
            "source": "/document/content"
        },
        {
            "name": "languageCode", 
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "entities", 
            "targetName": "entities" 
        }
    ]
}
```

## Sample input

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Microsoft is liked by many.",
             "languageCode": "en"
           }
      }
    ]
}
```

## Sample output

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "entities": [
          {
            "name": "Microsoft", 
            "id": "Microsoft",
            "language": "en", 
            "url": "https://en.wikipedia.org/wiki/Microsoft", 
            "bingId": "a093e9b9-90f5-a3d5-c4b8-5855e1b01f85", 
            "dataSource": "Wikipedia", 
            "matches": [
                {
                    "text": "Microsoft", 
                    "offset": 0, 
                    "length": 9, 
                    "confidenceScore": 0.13 
                }
            ]
          }
        ],
      }
    }
  ]
}
```

The offsets returned for entities in the output of this skill are directly returned from the [Language Service APIs](../cognitive-services/language-service/overview.md), which means if you are using them to index into the original string, you should use the [StringInfo](/dotnet/api/system.globalization.stringinfo) class in .NET in order to extract the correct content. For more information, see [Multilingual and emoji support in Language service features](../cognitive-services/language-service/concepts/multilingual-emoji-support.md).

## Warning cases

If the language code for the document is unsupported, a warning is returned and no entities are extracted.

## See also

+ [Built-in skills](cognitive-search-predefined-skills.md)
+ [How to define a skillset](cognitive-search-defining-skillset.md)