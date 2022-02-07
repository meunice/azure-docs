---
title: Introduction to Azure Arc-enabled data services with Active Directory authentication
description: Introduction to Azure Arc-enabled data services with Active Directory authentication
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 12/15/2021
ms.topic: how-to
---

# Introduction to Azure Arc-enabled SQL Managed Instance with Active Directory authentication 

This article describes Azure Arc-enabled SQL Managed Instance with Active Directory (AD) Authentication by bring your own keytab (BYOK) where the user is expected to provide a pre-created Active Directory account, Service Principal Names and Keytab.

## Background

In order to support Active Directory authentication for SQL Managed Instance, a SQL Managed Instance must be deployed in an environment that allows it to communicate with the Active Directory domain.
To facilitate this, Azure Arc introduces a new Kubernetes-native [custom resource definition (CRD)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) called `Active Directory Connector`. You can specify this kind of resource in the CRD. An Active Directory Connector custom resource stores the information needed to enable connections to DNS and AD for purposes of authenticating users and service accounts.

This custom resource deploys a DNS proxy service that mediates between the SQL Managed Instance DNS resolver and the two upstream DNS servers:

1. Kubernetes DNS servers
2. Active Directory DNS servers

When a SQL Managed Instance is deployed with Active Directory Authentication enabled, it will reference the Active Directory Connector instance it wants to use. Referencing the Active Directory Connector in SQL MI spec will automatically set up the needed environment in the SQL Managed Instance container for SQL MI to perform Active Directory authentication.

## Active Directory Connector and SQL Managed Instance

![Actice Directory Connector](media/active-directory-deployment/active-directory-connector-byok.png)

## Bring Your Own Keytab (BYOK) 

The following are the steps for user to set up:

1. Creating and providing an Active Directory account for each SQL Managed Instance that must accept AD authentication.
1. Providing a DNS name belonging to the Active Directory DNS domain for the SQL Managed Instance endpoint.
1. Creating a DNS record in Active Directory for the SQL endpoint.
1. Providing a port number for the SQL Managed Instance endpoint.
1. Registering Service Principal Names (SPNs) under the AD account in Active Directory domain for the SQL endpoint.
1. Creating and providing a keytab file for SQL Managed Instance containing entries for the AD account and SPNs.

## Next steps

* [Deploy Active Directory (AD) connector](deploy-active-directory-connector.md)
* [Deploy Azure Arc-enabled SQL Managed Instance in Active Directory (AD)](deploy-active-directory-sql-managed-instance.md)
* [Connect to AD-integrated Azure Arc-enabled SQL Managed Instance](connect-active-directory-sql-managed-instance.md)
