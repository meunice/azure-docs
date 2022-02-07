---
title: Review and take action on admin consent requests
titleSuffix: Azure AD
description: Learn how to review and take action on admin consent requests that were created after you were designated as a reviewer.
services: active-directory
author: eringreenlee
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 11/17/2021
ms.author: ergreenl
ms.reviewer: ergreenl


#customer intent: As an admin, I want to review and take action on admin consent requests.
---
# Review and take action on admin consent requests

In this article, you learn how to review and take action on admin consent requests. To review and act on consent requests, you must be designated as a reviewer. As a reviewer, you only see admin consent requests that were created after you were designated as a reviewer.

## Prerequisites

To review and take action on admin consent requests, you need:

- An Azure account. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- One of the following roles: Global Administrator, Cloud Application Administrator, Application Administrator, or owner of the service principal.

## Review and take action on admin consent requests

To review the admin consent requests and take action:

1. Sign in to the [Azure portal](https://portal.azure.com) as one of the registered reviewers of the admin consent workflow.
2. Select **All services** at the top of the left-hand navigation menu.
3. In the filter search box, type "**Azure Active Directory**" and select **Azure Active Directory**.
4. From the navigation menu, select **Enterprise applications**.
5. Under **Activity**, select **Admin consent requests**.
6. Select the application that is being requested.
7. Review details about the request:  

   - To see who is requesting access and why, select the **Requested by** tab.
   - To see what permissions are being requested by the application, select **Review permissions and consent**.

8. Evaluate the request and take the appropriate action:

   - **Approve the request**. To approve a request, grant admin consent to the application. Once a request is approved, all requestors are notified that they have been granted access. Approving a request allows all users in your tenant to access the application unless otherwise restricted with user assignment.
  
   - **Deny the request**. To deny a request, you must provide a justification that will be provided to all requestors. Once a request is denied, all requestors are notified that they have been denied access to the application. Denying a request won't prevent users from requesting admin consent to the app again in the future.  
   - **Block the request**. To block a request, you must provide a justification that will be provided to all requestors. Once a request is blocked, all requestors are notified they've been denied access to the application. Blocking a request creates a service principal object for the application in your tenant in a disabled state. Users won't be able to request admin consent to the application in the future.
