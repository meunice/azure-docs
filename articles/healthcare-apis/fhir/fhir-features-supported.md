---
title: Supported FHIR features in the FHIR service 
description: This article explains which features of the FHIR specification that are implemented in Healthcare APIs
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 08/03/2021
ms.author: cavoeg
---

# Supported FHIR Features

> [!IMPORTANT]
> Azure Healthcare APIs is currently in PREVIEW. The [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) include additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

The FHIR&reg; service in the Azure Healthcare APIs (hereby called the FHIR service) provides a fully managed deployment of the [open-source FHIR Server](https://github.com/microsoft/fhir-server) and is an implementation of the [FHIR](https://hl7.org/fhir) standard. This document lists the main features of the FHIR service.

## FHIR version

Latest version supported: `4.0.1`

Previous versions also currently supported include: `3.0.2`

## REST API

| API    | Azure API for FHIR | FHIR service in Healthcare APIs | Comment |
|--------|--------------------|---------------------------------|---------|
| read   | Yes                | Yes                             |         |
| vread  | Yes                | Yes                             |         |
| update | Yes                | Yes                             |         | 
| update with optimistic locking | Yes       | Yes       |
| update (conditional)           | Yes       | Yes       |
| patch                          | Yes       | Yes       | Support for [JSON Patch](https://www.hl7.org/fhir/http.html#patch) only. We have included a workaround to use JSON Patch in a bundle in [this PR](https://github.com/microsoft/fhir-server/pull/2143).|
| patch (conditional)            | Yes       | Yes       |
| delete                         | Yes       | Yes       | See details in the delete section below |
| delete (conditional)           | Yes       | Yes       | See details in the delete section below |
| history                        | Yes       | Yes       |
| create                         | Yes       | Yes       | Support both POST/PUT |
| create (conditional)           | Yes       | Yes       | Issue [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Partial   | Partial   | See [Overview of FHIR Search](overview-of-search.md). |
| chained search                 | Yes       | Yes       | |
| reverse chained search         | Yes       | Yes       | |
| batch                          | Yes       | Yes       |
| transaction                    | No        | Yes       |
| paging                         | Partial   | Partial   | `self` and `next` are supported                     |
| intermediaries                 | No        | No        |

### Delete and conditional delete

Delete defined by the FHIR spec requires that after deleting, subsequent non-version specific reads of a resource returns a 410 HTTP status code and the resource is no longer found through searching. The Azure API for FHIR and the FHIR service also enable you to fully delete (including all history) the resource. To fully delete the resource, you can pass a parameter settings `hardDelete` to true (`DELETE {server}/{resource}/{id}?hardDelete=true`). If you do not pass this parameter or set `hardDelete` to false, the historic versions of the resource will still be available.

In addition to delete, the Azure API for FHIR and the FHIR service support conditional delete, which allow you to pass a search criteria to delete a resource. By default, the conditional delete will allow you to delete one item at a time. You can also specify the `_count` parameter to delete up to 100 items at a time. Below are some examples of using conditional delete.

To delete a single item using conditional delete, you must specify search criteria that returns a single item.
``` JSON
DELETE https://{{hostname}}/Patient?identifier=1032704
```

You can do the same search but include hardDelete=true to also delete all history.
```JSON 
DELETE https://{{hostname}}/Patient?identifier=1032704&hardDelete=true
```

If you want to delete multiple resources, you can include `_count=100`, which will delete up to 100 resources that match the search criteria. 
``` JSON
DELETE https://{{hostname}}/Patient?identifier=1032704&_count=100
```

## Extended Operations

All the operations that are supported that extend the RESTful API.

| Search parameter type | Azure API for FHIR | FHIR service in Healthcare APIs| Comment |
|------------------------|-----------|-----------|---------|
| $export (whole system) | Yes       | Yes       |         |
| Patient/$export        | Yes       | Yes       |         |
| Group/$export          | Yes       | Yes       |         |
| $convert-data          | Yes       | Yes       |         |
| $validate              | Yes       | Yes       |         |
| $member-match          | Yes       | Yes       |         |
| $patient-everything    | Yes       | Yes       |         |
| $purge-history         | Yes       | Yes       |         |

## Role-based access control

The FHIR service uses [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) for access control. 

## Service limits

* **Bundle size** - Each bundle is limited to 500 items.

* **Subscription Limit** - By default, each subscription is limited to a maximum of 10 FHIR services. The limit can be used in one or many workspaces. 

## Next steps

In this article, you've read about the supported FHIR features in the FHIR service. Next deploy the FHIR service.
 
>[!div class="nextstepaction"]
>[Deploy FHIR service](fhir-portal-quickstart.md)
