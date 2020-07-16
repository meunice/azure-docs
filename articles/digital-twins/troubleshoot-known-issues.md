---
title: Known issues - Azure Digital Twins
description: Get help recognizing and mitigating known issues with Azure Digital Twins.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
---

# Known issues in Azure Digital Twins

This article provides information about known issues associated with Azure Digital Twins.

## Managing event routes in the Azure portal

If you are signed into the portal with a personal [**Microsoft account (MSA)**](https://account.microsoft.com/account/Account), such as an *@outlook.com* account, you will see a screen saying *You need permission to view event routes* when attempting to manage event routes in the portal, regardless of your permission level.

:::image type="content" source="media/troubleshoot-known-issues/event-route-need-permission.png" alt-text="Screenshot from the Azure portal of the permission error when trying to create event routes on an Azure Digital Twins instance":::

### Troubleshooting steps

Users who are currently unable to manage event routes in the portal can still manage event routes using the Azure Digital Twins APIs or CLI. Switching to one of these tools for event route management is the recommended strategy to mitigate this issue.

The instructions for this can be found in [*How-to: Manage endpoints and routes*](how-to-manage-routes.md).

### Possible causes

You are signed into the portal with a personal [Microsoft account (MSA)](https://account.microsoft.com/account/Account), such as an *@outlook.com* account. Managing event routes in the Azure portal is currently only available to Azure users on corporate-domain accounts.

## "400 Client Error: Bad Request" in Cloud Shell

Commands in Cloud Shell may intermittently fail with the error "400 Client Error: Bad Request for url: http://localhost:50342/oauth2/token" followed by full stack trace.

### Troubleshooting steps

This can be resolved by re-running the `az login` command and completing subsequent login steps.

After this, you should be able to re-run the command.

### Possible causes

This is the result of a known issue in Cloud Shell: [*Getting token from Cloud Shell intermittently fails with 400 Client Error: Bad Request*](https://github.com/Azure/azure-cli/issues/11749).

## Next steps

Read more about security and permissions on Azure Digital Twins:
* [*Concepts: Security for Azure Digital Twins solutions*](concepts-security.md)