---
title: What is Azure Form Recognizer? (updated)
titleSuffix: Azure Applied AI Services
description: The Azure Form Recognizer service allows you to identify and extract key/value pairs and table data from your form documents, as well as extract major information from sales receipts and business cards.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 12/10/2021
ms.author: lajanuar
recommendations: false
keywords: automated data processing, document processing, automated data entry, forms processing
#Customer intent: As a developer of form-processing software, I want to learn what the Form Recognizer service does so I can determine if I should use it.
ms.custom: ignite-fall-2021
---
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD024 -->
# What is Azure Form Recognizer?

:::image type="content" source="media/form-recognizer-icon.png" alt-text="Form Recognizer icon from the Azure portal.":::

Azure Form Recognizer is a cloud-based [Azure Applied AI Service](../../applied-ai-services/index.yml) that uses machine-learning models to extract and analyze form fields, text, and tables from your documents. Form Recognizer analyzes your forms and documents, extracts text and data, maps field relationships as key-value pairs, and returns a structured JSON output. You quickly get accurate results that are tailored to your specific content without excessive manual intervention or extensive data science expertise. Use Form Recognizer to automate your data processing in applications and workflows, enhance data-driven strategies, and enrich document search capabilities.

Form Recognizer easily identifies, extracts, and analyzes the following document data:

* Table structure and content.
* Form elements and field values.
* Typed and handwritten alphanumeric text.
* Relationships between elements.
* Key/value pairs
* Element location with bounding box coordinates.

## Form Recognizer features and development options

### [Form Recognizer GA (v2.1)](#tab/v2-1)

The following features are supported by Form Recognizer v2.1. Use the links in the table to learn more about each feature and browse the API references.

| Feature | Description | Development options |
|----------|--------------|-------------------------|
|[**Layout API**](concept-layout.md) | Extraction and analysis of text, selection marks, and table structures,  along with their bounding box coordinates, from forms and documents. | <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#analyze-layout)</li><li>[**REST API**](quickstarts/get-started-sdk-rest-api.md#try-it-layout-model)</li><li>[**Client-library SDK**](quickstarts/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?branch=main&tabs=layout#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Custom model**](concept-custom.md) | Extraction and analysis of data from forms and documents specific to distinct business data and use cases.| <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#train-a-custom-form-model)</li><li>[**REST API**](quickstarts/try-sdk-rest-api.md)</li><li>[**Client-library SDK**](how-to-guides/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=custom#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Invoice model**](concept-invoice.md) | Automated data processing and extraction of key information from sales invoices. | <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#analyze-using-a-prebuilt-model)</li><li>[**REST API**](quickstarts/get-started-sdk-rest-api.md#try-it-prebuilt-model)</li><li>[**Client-library SDK**](quickstarts/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=invoice#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Receipt model**](concept-receipt.md) | Automated data processing and extraction of key information from sales receipts.| <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#analyze-using-a-prebuilt-model)</li><li>[**REST API**](quickstarts/get-started-sdk-rest-api.md#try-it-prebuilt-model)</li><li>[**Client-library SDK**](how-to-guides/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=receipt#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**ID document model**](concept-id-document.md) | Automated data processing and extraction of key information from US driver's licenses and international passports.| <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#analyze-using-a-prebuilt-model)</li><li>[**REST API**](quickstarts/get-started-sdk-rest-api.md#try-it-prebuilt-model)</li><li>[**Client-library SDK**](how-to-guides/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=id-document#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Business card model**](concept-business-card.md) | Automated data processing and extraction of key information from business cards.| <ul><li>[**Form Recognizer labeling tool**](quickstarts/try-sample-label-tool.md#analyze-using-a-prebuilt-model)</li><li>[**REST API**](quickstarts/get-started-sdk-rest-api.md#try-it-prebuilt-model)</li><li>[**Client-library SDK**](how-to-guides/try-sdk-rest-api.md)</li><li>[**Form Recognizer Docker container**](containers/form-recognizer-container-install-run.md?tabs=business-card#run-the-container-with-the-docker-compose-up-command)</li></ul>|

### [Form Recognizer preview (v3.0)](#tab/v3-0)

The following features  and development options are supported by the Form Recognizer service v3.0. Use the links in the table to learn more about each feature and browse the API references.

| Feature | Description | Development options |
|----------|--------------|-------------------------|
|[🆕 **General document model**](concept-general-document.md)|Extract text, tables, structure, key-value pairs and, named entities.|<ul ><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#prebuilt-models)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md#try-it-general-document-model)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#general-document-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#general-document-model)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md#general-document-model)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md#general-document-model)</li></ul> |
|[**Layout model**](concept-layout.md) | Extract text, selection marks, and tables structures, along with their bounding box coordinates, from forms and documents.</br></br> Layout API has been updated to a prebuilt model. | <ul><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#layout)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md#try-it-layout-model)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#layout-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#layout-model)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md#layout-model)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md#layout-model)</li></ul>|
|[**Custom model (updated)**](concept-custom.md) | Extraction and analysis of data from forms and documents specific to distinct business data and use cases.</br></br>Custom model API v3.0 supports **signature detection for custom forms**.</li></ul>| <ul><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#custom-projects)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md)</li></ul>|
|[**Invoice model**](concept-invoice.md) | Automated data processing and extraction of key information from sales invoices. | <ul><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#prebuilt-models)</li><li>[**REST API**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#prebuilt-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#prebuilt-model)</li></ul>|
|[**Receipt model (updated)**](concept-receipt.md) | Automated data processing and extraction of key information from sales receipts.</br></br>Receipt model v3.0 supports processing of **single-page hotel receipts**.| <ul><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#prebuilt-models)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#prebuilt-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#prebuilt-model)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md#prebuilt-model)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md#prebuilt-model)</li></ul>|
|[**ID document model (updated)**](concept-id-document.md) |Automated data processing and extraction of key information from US driver's licenses and international passports.</br></br>Prebuilt ID document API supports the **extraction of endorsements, restrictions, and vehicle classifications from US driver's licenses**. |<ul><li> [**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#prebuilt-models)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#prebuilt-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#prebuilt-model)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md#prebuilt-model)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md#prebuilt-model)</li></ul>|
|[**Business card model**](concept-business-card.md) |Automated data processing and extraction of key information from business cards.| <ul><li>[**Form Recognizer Studio**](quickstarts/try-v3-form-recognizer-studio.md#prebuilt-models)</li><li>[**REST API**](quickstarts/try-v3-rest-api.md)</li><li>[**C# SDK**](quickstarts/try-v3-csharp-sdk.md#prebuilt-model)</li><li>[**Python SDK**](quickstarts/try-v3-python-sdk.md#prebuilt-model)</li><li>[**Java SDK**](quickstarts/try-v3-java-sdk.md#prebuilt-model)</li><li>[**JavaScript**](quickstarts/try-v3-javascript-sdk.md#prebuilt-model)</li></ul>|

---

## Which Form Recognizer feature should I use?

This section helps you decide which Form Recognizer feature you should use for your application.

| What type of document do you want to analyze?| How is the document formatted? | Your best solution |
| -----------------|-------------------| ----------|
|<ul><li>**Invoice**</li><li>**Receipt**</li><li>**Business card**</li></ul>| Is your invoice, receipt, or business card document composed of English-text? | <ul><li>If **Yes**, use the [**Invoice**](concept-invoice.md), [**Receipt**](concept-receipt.md), or [**Business Card**](concept-business-card.md) model.</li><li>If **No**, use the [**Layout**](concept-layout.md) or [**General document (preview)**](concept-general-document.md) model.</li></ul>|
|<ul><li>**ID document**</li></ul>| Is your ID document a US driver's license or an international passport?| <ul><li>If **Yes**, use the [**ID document**](concept-id-document.md) model.</li><li>If **No**, use the[**Layout**](concept-layout.md) or [**General document (preview)**](concept-general-document.md) model</li></ul>|
 |<ul><li>**Form** or **Document**</li></ul>| Is your form or document an industry-standard format commonly used in your business or industry?| <ul><li>If **Yes**, use the [**Layout**](concept-layout.md) or [**General document (preview)**](concept-general-document.md).</li><li>If **No**, you can [**Train and build a custom model**](quickstarts/try-sample-label-tool.md#train-a-custom-form-model).

## How to use Form Recognizer documentation

This documentation contains the following article types:

* [**Concepts**](concept-layout.md) provide in-depth explanations of the service functionality and features.
* [**Quickstarts**](quickstarts/try-sdk-rest-api.md) are getting-started instructions to guide you through making requests to the service.
* [**How-to guides**](how-to-guides/try-sdk-rest-api.md) contain instructions for using the service in more specific or customized ways.
* [**Tutorials**](tutorial-ai-builder.md) are longer guides that show you how to use the service as a component in broader business solutions.

## Data privacy and security

 As with all the cognitive services, developers using the Form Recognizer service should be aware of Microsoft policies on customer data. See our [Data, privacy, and security for Form Recognizer](/legal/cognitive-services/form-recognizer/fr-data-privacy-security) page.

## Next steps

### [Form Recognizer v2.1](#tab/v2-1)

> [!div class="checklist"]
>
> * Try our [**Sample Labeling online tool**](https://aka.ms/fott-2.1-ga/)
> * Follow our [**client library / REST API quickstart**](./quickstarts/try-sdk-rest-api.md) to get started extracting data from your documents. We recommend that you use the free service when you're learning the technology. Remember that the number of free pages is limited to 500 per month.
> * Explore the [**REST API reference documentation**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/AnalyzeWithCustomForm) to learn more.
> * If you're familiar with a previous version of the API, see the [**What's new**](./whats-new.md) article to learn of recent changes.

### [Form Recognizer v3.0](#tab/v3-0)

> [!div class="checklist"]
>
> * Try our [**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)
> * Explore the [**REST API reference documentation**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument) to learn more.
> * If you're familiar with a previous version of the API, see the [**What's new**](./whats-new.md) article to learn of recent changes.

---
