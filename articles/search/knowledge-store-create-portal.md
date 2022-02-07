---
title: "Quickstart: Create a knowledge store in the Azure portal"
titleSuffix: Azure Cognitive Search
description: Use the Import data wizard to create a knowledge store used for persisting enriched content. Connect to a knowledge store for analysis from other apps, or send enriched content to downstream processes.
author: HeidiSteen
ms.author: heidist
manager: nitinme
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 10/28/2021
ms.custom: mode-ui
---

# Quickstart: Create a knowledge store in the Azure portal

[Knowledge store](knowledge-store-concept-intro.md) is a feature of Azure Cognitive Search that accepts output from an [AI enrichment pipeline](cognitive-search-concept-intro.md) and makes it available in Azure Storage for downstream apps and workloads. Enrichments created by the pipeline - such as translated text, OCR text, tagged images, and recognized entities - are projected into tables or blobs, where they can be accessed by any app or workload that connects to Azure Storage.

In this quickstart, you'll set up your data and then run the **Import data** wizard to create an enrichment pipeline that also generates a knowledge store. The knowledge store will contain original text content pulled from the source (customer reviews of a hotel), plus AI-generated content that includes a sentiment label, key phrase extraction, and text translation of non-English customer comments.

> [!NOTE]
> This quickstart shows you the fastest route to a finished knowledge store in Azure Storage. For more detailed explanations of each step, see [Create a knowledge store in REST](knowledge-store-create-rest.md) instead.

## Prerequisites

This quickstart uses the following services:

+ An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/).

+ Azure Cognitive Search. [Create a service](search-create-service-portal.md) or [find an existing service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in your account. You can use a free service for this quickstart. 

+ Azure Storage. [Create an account](../storage/common/storage-account-create.md) or [find an existing account](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/). The account type must be **StorageV2 (general purpose V2)**.

+ Sample data. This quickstart uses hotel review data saved in a CSV file (originates from Kaggle.com) and contains 19 pieces of customer feedback about a single hotel.

  [Download HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?sp=r&st=2019-11-04T01:23:53Z&se=2025-11-04T16:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=siQgWOnI%2FDamhwOgxmj11qwBqqtKMaztQKFNqWx00AY%3D) and then [upload it to a blob container](../storage/blobs/storage-quickstart-blobs-portal.md) in Azure Storage.

This quickstart also uses [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) for AI enrichment. Because the workload is so small, Cognitive Services is tapped behind the scenes for free processing for up to 20 transactions. This means that you can complete this exercise without having to create an additional Cognitive Services resource.

## Start the wizard

1. Sign in to the [Azure portal](https://portal.azure.com/) with your Azure account.

1. [Find your search service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) and on the Overview page, click **Import data** on the command bar to create a knowledge store in four steps.

   :::image type="content" source="media/search-import-data-portal/import-data-cmd.png" alt-text="Screenshot of the Import data command" border="true":::

### Step 1: Create a data source

Because the data is multiple rows in one CSV file, set the *parsing mode* to get one search document for each row.

1. In **Connect to your data**, choose **Azure Blob Storage**, selecting the account and container you created. 

1. For the **Name**, enter "hotel-reviews-ds".

1. For **Parsing mode**, select **Delimited text**, and then select the **First Line Contains Header** checkbox. Make sure the **Delimiter character** is a comma (,).

1. In **Connection String**, paste in a connection string to your Azure Storage account. 

   A connection string has the following format: `DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

1. In **Containers**, enter the name of the blob container holding the data ("hotel-reviews").

    Your page should look similar to the following screenshot.

   :::image type="content" source="media/knowledge-store-create-portal/hotel-reviews-ds.png" alt-text="Screenshot of data source definition" border="true":::

1. Continue to the next page.

### Step 2: Add skills

In this wizard step, add skills for AI enrichment. The source data consists of customer reviews in English and French. Skills that are relevant for this data set include key phrase extraction, sentiment detection, and text translation. In a later step, these enrichments will be "projected" into a knowledge store as Azure tables.

1. Expand **Attach Cognitive Services**. **Free (Limited enrichments)** is selected by default. You can use this resource because the number of records in HotelReviews-Free.csv is 19 and this free resource allows up to 20 transactions a day.

1. Expand **Add enrichments**.

1. For **Skillset name**, enter "hotel-reviews-ss".

1. For **Source data field**, select **reviews_text**.

1. For **Enrichment granularity level**, select **Pages (5000 characters chunks)**.

1. For **Text Cognitive Skills**, select the following skills:

    + **Extract key phrases**
    + **Translate text**
    + **Language detection**
    + **Detect sentiment**

   Your page should look like the following screenshot:

   :::image type="content" source="media/knowledge-store-create-portal/hotel-reviews-ss.png" alt-text="Screenshot of the skillset definition" border="true":::

1. Scroll down and expand **Save enrichments to knowledge store**.

1. Select **Choose an existing connection** and then select an Azure Storage account. The Containers page will appear so that you can create a container for projections. We recommend adopting a prefix naming convention, such as "kstore-hotel-reviews" to distinguish between source content and knowledge store content.

1. Returning to the Import data wizard, select the following **Azure table projections**. The wizard always offers the **Documents** projection. Other projections will be offered depending on the skills you select (such as **Key phrases**), or the enrichment granularity (**Pages**):

    + **Documents**
    + **Pages**
    + **Key phrases**

   The following screenshot shows the table projection selections in the wizard.

   :::image type="content" source="media/knowledge-store-create-portal/hotel-reviews-ks.png" alt-text="Screenshot of the knowledge store definition" border="true":::

1. Continue to the next page.

### Step 3: Configure the index

In this wizard step, configure an index for optional full-text search queries. The wizard will sample your data source to infer fields and data types. You only need to select the attributes for your desired behavior. For example, the **Retrievable** attribute will allow the search service to return a field value while the **Searchable** will enable full text search on the field.

1. For **Index name**, enter "hotel-reviews-idx".

1. For attributes, accept the default selections: **Retrievable** and **Searchable** for the new fields that the pipeline is creating.

    Your index should look similar to the following image. Because the list is long, not all fields are visible in the image.

   :::image type="content" source="media/knowledge-store-create-portal/hotel-reviews-idx.png" alt-text="Screenshot of the index definition" border="true":::

1. Continue to the next page.

### Step 4: Configure and run the indexer

In this wizard step, configure an indexer that will pull together the data source, skillset, and the index you defined in the previous wizard steps.

1. For **Name**, enter "hotel-reviews-idxr".

1. For **Schedule**, keep the default **Once**.

1. Select **Submit** to run the indexer. Data extraction, indexing, application of cognitive skills all happen in this step.

### Step 5: Check status

In the **Overview** page, open the **Indexers** tab in the middle of the page, and then select **hotels-reviews-idxr**. Within a minute or two, status should progress from "In progress" to "Success" with zero errors and warnings.

## Check tables in Storage Browser

In the Azure portal, switch to your Azure Storage account and use **Storage Browser** to view the new tables. You should see three tables, one for each projection that was offered in the "Save enrichments" section of the "Add enrichments" page.

+ "hotelReviewssDocuments" contains all of the first-level nodes of a document's enrichment tree that are not collections. 

+ "hotelReviewssKeyPhrases" contains a long list of just the key phrases extracted from all reviews. Skills that output collections (arrays), such as key phrases and entities, will have output sent to a standalone table.

+ "hotelReviewssPages" contains enriched fields created over each page that was split from the document. In this skillset and data source, page-level enrichments consisting of sentiment labels and translated text. A pages table (or a sentences table if you specify that particular level of granularity) is created when you choose "pages" granularity in the skillset definition. 

All of these tables contain ID columns to support table relationships in other tools and apps. When you open a table, scroll past these fields to view the content fields added by the pipeline.

In this quickstart, the table for "hotelReviewssPages" should look similar to the following screenshot:

   :::image type="content" source="media/knowledge-store-create-portal/azure-table-hotel-reviews.png" alt-text="Screenshot of the generated tables in Storage Browser" border="true":::

## Clean up

When you're working in your own subscription, it's a good idea at the end of a project to identify whether you still need the resources you created. Resources left running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.

You can find and manage resources in the portal, using the **All resources** or **Resource groups** link in the left-navigation pane.

If you are using a free service, remember that you are limited to three indexes, indexers, and data sources. You can delete individual items in the portal to stay under the limit.

> [!TIP]
> If you want to repeat this exercise or try a different AI enrichment walkthrough, delete the **hotel-reviews-idxr** indexer and the related objects to recreate them. Deleting the indexer resets the free daily transaction counter to zero.

## Next steps

Now that you've been introduced to a knowledge store, take a closer look at each step by switching over to the REST API walkthrough. Tasks that the wizard handled internally are explained in the REST walkthrough.

> [!div class="nextstepaction"]
> [Create a knowledge store using REST and Postman](knowledge-store-create-rest.md)
