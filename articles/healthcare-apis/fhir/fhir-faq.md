---
title: FAQs about FHIR services in Azure Healthcare APIs
description: Get answers to frequently asked questions about the FHIR service, such as the storage location of data behind FHIR APIs and version support.
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 12/30/2021
ms.author: cavoeg
ms.custom: references_regions
---

# Frequently asked questions about the FHIR service

> [!IMPORTANT]
> Azure Healthcare APIs is currently in PREVIEW. The [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) include additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

This section covers some of the frequently asked questions about the Azure Healthcare APIs FHIR service (hereby called the FHIR service).

## FHIR service: The Basics

### What is FHIR?

The Fast Healthcare Interoperability Resources (FHIR - Pronounced "fire") is an interoperability standard intended to enable the exchange of healthcare data between different health systems. This standard was developed by the HL7 organization and is being adopted by healthcare organizations around the world. The most current version of FHIR available is R4 (Release 4). The FHIR service supports R4 and the previous version STU3 (Standard for Trial Use 3). For more information on FHIR, visit [HL7.org](http://hl7.org/fhir/summary.html).

### Is the data behind the FHIR APIs stored in Azure?

Yes, the data is stored in managed databases in Azure. The FHIR service in the Azure Healthcare APIs does not provide direct access to the underlying data store.

## How can I get access to the underlying data?

In the managed service, you can't access the underlying data. This is to ensure that the FHIR service offers the privacy and compliance certifications needed for healthcare data. If you need access to the underlying data, you can use the [open-source FHIR server](https://github.com/microsoft/fhir-server).  

### What identity provider do you support?

We support Microsoft Azure Active Directory as the identity provider.

## Can I use Azure AD B2C with the FHIR service?

No, we don't support B2C in the FHIR service. If you need more granular access controls, we recommend looking at the [open-source FHIR proxy](https://github.com/microsoft/fhir-proxy). 

### What FHIR version do you support?

We support versions 4.0.0 and 3.0.1.

For more information, see [Supported FHIR features](fhir-features-supported.md). You can also read about what has changed between FHIR versions (STU3 to R4) in the [version history for HL7 FHIR](https://hl7.org/fhir/R4/history.html).

### What is the difference between Azure API for FHIR and the FHIR service in the Healthcare APIs?

The FHIR service is our implementation of the FHIR specification that sits in the Azure Healthcare APIs, which allows you to have a FHIR service and a DICOM service within a single workspace. The Azure API for FHIR was our initial GA product and is still available as a stand-alone product. The main feature differences are:

* The FHIR service has a limit of 4 TB and is in public preview while the Azure API for FHIR supports more than 4 TB and is GA.
* The FHIR service support [transaction bundles](https://www.hl7.org/fhir/http.html#transaction).
* The Azure API for FHIR has more platform features (such as private link, customer managed keys, and logging) that are not yet available in the FHIR service in the Azure Healthcare APIs. More details will follow on these features by GA.

### What's the difference between the FHIR service in the Azure Healthcare APIs and the open-source FHIR server?

The FHIR service in the Azure Healthcare APIs is a hosted and managed version of the open-source [Microsoft FHIR Server for Azure](https://github.com/microsoft/fhir-server). In the managed service, Microsoft provides all maintenance and updates.

When you run the FHIR Server for Azure, you have direct access to the underlying services, but we're responsible for maintaining and updating the server and all required compliance work if you're storing PHI data.

### In which regions is the FHIR service available?

The FHIR service is available in all regions that the Azure Healthcare APIs is available. You can see that on the [Products by Region](https://azure.microsoft.com/global-infrastructure/services/?products=azure-api-for-fhir) page.

### Where can I see what is releasing into the FHIR service?

The [release notes](../release-notes.md) page provides an overview of everything that has shipped to the managed service in the previous month. 

To see what will be releasing to the managed service, you can review the [releases page](https://github.com/microsoft/fhir-server/releases) of the open-source FHIR Server. We have worked to tag items with Azure Healthcare APIs if they will release to the managed service and are usually available two weeks after they are on the release page in open-source. We have also included instructions on how to [test the build](https://github.com/microsoft/fhir-server/blob/master/docs/Testing-Releases.md) if you'd like to test in your own environment. We are evaluating how to best share additional managed service updates.

To see what release package is currently in the managed service, you can view the capability statement for the FHIR service and under the `software.version` property. You'll see which package is deployed. 

### Where can I find what version of FHIR (R4/STU3) is running on my database?

You can find the exact FHIR version exposed in the capability statement under the `fhirVersion` property (FHIR URL/metadata).

### Can I switch my FHIR service from STU3 to R4?

No. We don't have a way to change the version of an existing database. You'll need to create a new FHIR service and reload the data. You can leverage the JSON to FHIR converter as a place to start with converting STU3 data into R4.

### Can I customize the URL for my FHIR service?

No. You can't change the URL for the FHIR service. 

## FHIR Implementations and Specifications

### What is SMART on FHIR?

SMART (Substitutable Medical Applications and Reusable Technology) on FHIR is a set of open specifications to integrate partner applications with FHIR Servers and other Health IT systems, such as Electronic Health Records and Health Information Exchanges. By creating a SMART on FHIR application, you can ensure that your application can be accessed and leveraged by a plethora of different systems. For more information about SMART, see [SMART Health IT](https://smarthealthit.org/).

### Does the FHIR service support SMART on FHIR?

We have a basic SMART on FHIR proxy as part of the managed service. If this doesn’t meet your needs, you can use the open-source FHIR proxy for more advanced SMART scenarios. 

### Can I create a custom FHIR resource?

We do not allow custom FHIR resources. If you need a custom FHIR resource, you can build a custom resource on top of the [Basic resource](http://www.hl7.org/fhir/basic.html) with extensions. 

### Are [extensions](https://www.hl7.org/fhir/extensibility.html) supported on the FHIR service?

We allow you to load any valid FHIR JSON data into the server. If you want to store the structure definition that defines the extension, you can save this as a structure definition resource. To search on extensions, you'll need to [define your own search parameters](how-to-do-custom-search.md).

### How do I see the FHIR service in XML?

In the managed service, we only support JSON. The open-source FHIR server supports JSON and XML. To view the XML version in open-source, use `_format= application/fhir+xml`. 

### What is the limit on _count?

The current limit on _count is 1000. If you set _count to more than 1000, you'll receive a warning in the bundle that only 1000 records will be shown.

### Can I post a bundle to the FHIR service?

We currently support posting [batch bundles](https://www.hl7.org/fhir/valueset-bundle-type.html) and posting [transaction bundles](https://www.hl7.org/fhir/http.html#transaction) in the FHIR service.

### How can I get all resources for a single patient in the FHIR service?

We support the [$patient-everything operation](patient-everything.md) which will get you all data related to a single patient. 

### What is the default sort when searching for resources in the FHIR service?

We support sorting by string and dateTime fields in the FHIR service. For more information about other supported search parameters, see [Overview of FHIR search](overview-of-search.md).

### Does the FHIR service support any terminology operations?

No, the FHIR service doesn't support terminology operations today.

### What are the differences between delete types in the FHIR service? 

There're two basic Delete types supported within the FHIR service. These are [Delete and Conditional Delete](././../fhir/fhir-rest-api-capabilities.md#delete-and-conditional-delete).


* With Delete, you can choose to do a soft delete (most common type) and still be able to recover historic versions of your record.
* With Conditional Delete, you can pass search criteria to delete a resource one item at a time or several at a time.
* If you passed the `hardDelete` parameter with either Delete or Conditional Delete, all the records and history are deleted and unrecoverable.

## Using the FHIR service

### Where can I see some examples of using the FHIR service within a workflow?

We have a collection of reference architectures available on the [Health Architecture GitHub page](https://github.com/microsoft/health-architectures).

## Next steps

In this article, you've learned the answers to frequently asked questions about the FHIR service. To see the frequently asked questions about the FHIR service in Azure API for FHIR, see
 
>[!div class="nextstepaction"]
>[FAQs about Azure API for FHIR](../azure-api-for-fhir/fhir-faq.yml)