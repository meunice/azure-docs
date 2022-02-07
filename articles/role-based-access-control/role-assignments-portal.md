---
title: Assign Azure roles using the Azure portal - Azure RBAC
description: Learn how to grant access to Azure resources for users, groups, service principals, or managed identities using the Azure portal and Azure role-based access control (Azure RBAC).
services: active-directory
author: rolyon
manager: karenhoran
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 10/15/2021
ms.author: rolyon
ms.custom: contperf-fy21q3-portal,subject-rbac-steps
---

# Assign Azure roles using the Azure portal

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control/definition-grant.md)] This article describes how to assign roles using the Azure portal.

If you need to assign administrator roles in Azure Active Directory, see [Assign Azure AD roles to users](../active-directory/roles/manage-roles-portal.md).

## Prerequisites

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

#### [Current](#tab/current/)

## Step 1: Identify the needed scope

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)] For more information, see [Understand scope](scope-overview.md).

![Diagram showing the scope levels for Azure RBAC.](../../includes/role-based-access-control/media/scope-levels.png)

1. Sign in to the [Azure portal](https://portal.azure.com).

1. In the Search box at the top, search for the scope you want to grant access to. For example, search for **Management groups**, **Subscriptions**, **Resource groups**, or a specific resource.

1. Click the specific resource for that scope.

    The following shows an example resource group.

    ![Screenshot of resource group overview page.](./media/shared/rg-overview.png)

## Step 2: Open the Add role assignment page

**Access control (IAM)** is the page that you typically use to assign roles to grant access to Azure resources. It's also known as identity and access management (IAM) and appears in several locations in the Azure portal.

1. Click **Access control (IAM)**.

    The following shows an example of the Access control (IAM) page for a resource group.

    ![Screenshot of Access control (IAM) page for a resource group.](./media/shared/rg-access-control.png)

1. Click the **Role assignments** tab to view the role assignments at this scope.

1. Click **Add** > **Add role assignment**.

    If you don't have permissions to assign roles, the Add role assignment option will be disabled.

    ![Screenshot of Add > Add role assignment menu.](./media/shared/add-role-assignment-menu.png)

    The Add role assignment page opens.

## Step 3: Select the appropriate role

1. On the **Roles** tab, select a role that you want to use.

    You can search for a role by name or by description. You can also filter roles by type and category.

   ![Screenshot of Add role assignment page with Roles tab.](./media/shared/roles.png)

1. In the **Details** column, click **View** to get more details about a role.

   ![Screenshot of View role details pane with Permissions tab.](./media/role-assignments-portal/select-role-permissions.png)

1. Click **Next**.

## Step 4: Select who needs access

1. On the **Members** tab, select **User, group, or service principal** to assign the selected role to one or more Azure AD users, groups, or service principals (applications).

   ![Screenshot of Add role assignment page with Members tab.](./media/shared/members.png)

1. Click **Select members**.

1. Find and select the users, groups, or service principals.

    You can type in the **Select** box to search the directory for display name or email address.

   ![Screenshot of Select members pane.](./media/shared/select-members.png)

1. Click **Select** to add the users, groups, or service principals to the Members list.

1. To assign the selected role to one or more managed identities, select **Managed identity**.

1. Click **Select members**.

1. In the **Select managed identities** pane, select whether the type is [user-assigned managed identity](../active-directory/managed-identities-azure-resources/overview.md) or [system-assigned managed identity](../active-directory/managed-identities-azure-resources/overview.md).

1. Find and select the managed identities.

    For system-assigned managed identities, you can select managed identities by Azure service instance.

   ![Screenshot of Select managed identities pane.](./media/role-assignments-portal/select-managed-identity.png)

1. Click **Select** to add the managed identities to the Members list.

1. In the **Description** box enter an optional description for this role assignment.

    Later you can show this description in the role assignments list.

1. Click **Next**.

## Step 5: (Optional) Add condition (preview)

If you selected a role that supports conditions, a **Conditions (optional)** tab will appear and you have the option to add a condition to your role assignment. A [condition](conditions-overview.md) is an additional check that you can optionally add to your role assignment to provide more fine-grained access control.

Currently, conditions can be added to built-in or custom role assignments that have [storage blob data actions](conditions-format.md#actions). These include the following built-in roles:


- [Storage Blob Data Contributor](built-in-roles.md#storage-blob-data-contributor)
- [Storage Blob Data Owner](built-in-roles.md#storage-blob-data-owner)
- [Storage Blob Data Reader](built-in-roles.md#storage-blob-data-reader)

1. Click **Add condition** if you want to further refine the role assignments based on storage blob attributes. For more information, see [Add or edit Azure role assignment conditions](conditions-role-assignments-portal.md).

   ![Screenshot of Add role assignment page with Add condition tab.](./media/shared/condition.png)

1. Click **Next**.

## Step 6: Assign role

1. On the **Review + assign** tab, review the role assignment settings.

   ![Screenshot of Assign a role page with Review + assign tab.](./media/role-assignments-portal/review-assign.png)

1. Click **Review + assign** to assign the role.

   After a few moments, the security principal is assigned the role at the selected scope.

    ![Screenshot of role assignment list after assigning role.](./media/role-assignments-portal/rg-role-assignments.png)

1. If you don't see the description for the role assignment, click **Edit columns** to add the **Description** column.

#### [Classic](#tab/classic/)

## Step 1: Identify the needed scope (classic)

[!INCLUDE [Scope for Azure RBAC introduction](../../includes/role-based-access-control/scope-intro.md)] For more information, see [Understand scope](scope-overview.md).

![Diagram showing the scope levels for Azure RBAC for classic experience.](../../includes/role-based-access-control/media/scope-levels.png)

1. Sign in to the [Azure portal](https://portal.azure.com).

1. In the Search box at the top, search for the scope you want to grant access to. For example, search for **Management groups**, **Subscriptions**, **Resource groups**, or a specific resource.

1. Click the specific resource for that scope.

    The following shows an example resource group.

    ![Screenshot of resource group overview page for classic experience.](./media/shared/rg-overview.png)

## Step 2: Open the Add role assignment pane (classic)

**Access control (IAM)** is the page that you typically use to assign roles to grant access to Azure resources. It's also known as identity and access management (IAM) and appears in several locations in the Azure portal.

1. Click **Access control (IAM)**.

    The following shows an example of the Access control (IAM) page for a resource group.

    ![Screenshot of Access control (IAM) page for a resource group for classic experience.](./media/shared/rg-access-control.png)

1. Click the **Role assignments** tab to view the role assignments at this scope.

1. Click **Add** > **Add role assignment**.
   If you don't have permissions to assign roles, the Add role assignment option will be disabled.

   ![Screenshot of Add > Add role assignment menu for classic experience.](./media/shared/add-role-assignment-menu.png)

1. On the Add role assignment page, click **Use classic experience**.

   ![Screenshot of Add role assignment page with Use classic experience link for classic experience.](./media/role-assignments-portal/add-role-assignment-page-use-classic.png)

    The Add role assignment pane opens.

   ![Screenshot of Add role assignment page with Role, Assign access to, and Select options for classic experience.](./media/role-assignments-portal/add-role-assignment-page.png)

## Step 3: Select the appropriate role (classic)

1. In the **Role** list, search or scroll to find the role that you want to assign.

    To help you determine the appropriate role, you can hover over the info icon to display a description for the role. For additional information, you can view the [Azure built-in roles](built-in-roles.md) article.

   ![Screenshot of Select a role list in Add role assignment for classic experience.](./media/role-assignments-portal/add-role-assignment-role.png)

1. Click to select the role.

## Step 4: Select who needs access (classic)

1. In the **Assign access to** list, select the type of security principal to assign access to.

    | Type | Description |
    | --- | --- |
    | **User, group, or service principal** | If you want to assign the role to a user, group, or service principal (application), select this type. |
    | **User-assigned managed identity** | If you want to assign the role to a [user-assigned managed identity](../active-directory/managed-identities-azure-resources/overview.md), select this type. |
    | *System-assigned managed identity* | If you want to assign the role to a [system-assigned managed identity](../active-directory/managed-identities-azure-resources/overview.md), select the Azure service instance where the managed identity is located. |

   ![Screenshot of selecting a security principal in Add role assignment for classic experience.](./media/role-assignments-portal/add-role-assignment-type.png)

1. If you selected a user-assigned managed identity or a system-assigned managed identity, select the **Subscription** where the managed identity is located.

1. In the **Select** section, search for the security principal by entering a string or scrolling through the list.

   ![Screenshot of selecting a user in Add role assignment for classic experience.](./media/role-assignments-portal/add-role-assignment-user.png)

1. Once you have found the security principal, click to select it.

## Step 5: Assign role (classic)

1. To assign the role, click **Save**.

   After a few moments, the security principal is assigned the role at the selected scope.

1. On the **Role assignments** tab, verify that you see the role assignment in the list.

    ![Screenshot of role assignment list after assigning role for classic experience.](./media/role-assignments-portal/rg-role-assignments.png)

---

## Next steps

- [Assign a user as an administrator of an Azure subscription](role-assignments-portal-subscription-admin.md)
- [Remove Azure role assignments](role-assignments-remove.md)
- [Troubleshoot Azure RBAC](troubleshooting.md)
