---
title: Supported FHIR features in Azure - Azure API for FHIR 
description: This article explains which features of the FHIR specification that are implemented in Azure API for FHIR
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 6/16/2021
ms.author: cavoeg
---

# Features

Azure API for FHIR provides a fully managed deployment of the Microsoft FHIR Server for Azure. The server is an implementation of the [FHIR](https://hl7.org/fhir) standard. This document lists the main features of the FHIR Server.

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
| chained search                 | Yes       | Yes       | See Note below. |
| reverse chained search         | Yes       | Yes       | See Note below. |
| batch                          | Yes       | Yes       |
| transaction                    | No        | Yes       |
| paging                         | Partial   | Partial   | `self` and `next` are supported                     |
| intermediaries                 | No        | No        |

> [!Note] 
> In the Azure API for FHIR and the open-source FHIR server backed by Cosmos, the chained search and reverse chained search is an MVP implementation. To accomplish chained search on Cosmos DB, the implementation walks down the search expression and issues sub-queries to resolve the matched resources. This is done for each level of the expression. If any query returns more than 1000 results, an error will be thrown.

### Delete and conditional delete

Delete defined by the FHIR spec requires that after deleting, subsequent non-version specific reads of a resource return a 410 HTTP status code and the resource is no longer found through searching. The Azure API for FHIR and the FHIR service also enable you to fully delete (including all history) the resource. To fully delete the resource, you can pass a parameter settings `hardDelete` to true (`DELETE {server}/{resource}/{id}?hardDelete=true`). If you do not pass this parameter or set `hardDelete` to false, the historic versions of the resource will still be available.

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

## Persistence

The Microsoft FHIR Server has a pluggable persistence module (see [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Currently the FHIR Server open-source code includes an implementation for [Azure Cosmos DB](../../cosmos-db/index-overview.md) and [SQL Database](https://azure.microsoft.com/services/sql-database/).

Cosmos DB is a globally distributed multi-model (SQL API, MongoDB API, etc.) database. It supports different [consistency levels](../../cosmos-db/consistency-levels.md). The default deployment template configures the FHIR Server with `Strong` consistency, but the consistency policy can be modified (generally relaxed) on a request by request basis using the `x-ms-consistency-level` request header.

## Role-based access control

The FHIR Server uses [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) for access control. Specifically, role-based access control (RBAC) is enforced, if the `FhirServer:Security:Enabled` configuration parameter is set to `true`, and all requests (except `/metadata`) to the FHIR Server must have `Authorization` request header set to `Bearer <TOKEN>`. The token must contain one or more roles as defined in the `roles` claim. A request will be allowed if the token contains a role that allows the specified action on the specified resource.

Currently, the allowed actions for a given role are applied *globally* on the API.

## Service limits

* [**Request Units (RUs)**](../../cosmos-db/concepts-limits.md) - You can configure up to 10,000 RUs in the portal for Azure API for FHIR. You will need a minimum of 400 RUs or 40 RUs/GB, whichever is larger. If you need more than 10,000 RUs, you can put in a support ticket to have the RUs increased. The maximum available is 1,000,000.

* **Bundle size** - Each bundle is limited to 500 items.

* **Data size** - Data/Documents must each be slightly less than 2 MB.

* **Subscription Limit** - By default, each subscription is limited to a maximum of 10 FHIR Server Instances. If you need more instances per subscription, open a support ticket and provide details about your needs.

* **Concurrent connections and Instances** - By default, you have 15 concurrent connections on two instances in the cluster (for a total of 30 concurrent requests). If you need more concurrent requests, open a support ticket and provide details about your needs.

## Next steps

In this article, you've read about the supported FHIR features in Azure API for FHIR. Next deploy the Azure API for FHIR.
 
>[!div class="nextstepaction"]
>[Deploy Azure API for FHIR](fhir-paas-portal-quickstart.md)
