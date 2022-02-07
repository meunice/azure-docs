---
title: Complete an access review of groups & applications - Azure AD
description: Learn how to complete an access review of group members or application access in Azure Active Directory access reviews.
services: active-directory
documentationcenter: ''
author: ajburnle
manager: karenhoran
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 08/20/2021
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
---
 
# Complete an access review of groups and applications in Azure AD access reviews
 
As an administrator, you [create an access review of groups or applications](create-access-review.md) and reviewers [perform the access review](perform-access-review.md). This article describes how to see the results of the access review and apply them.
 
[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]
 
## Prerequisites
 
- Azure AD Premium P2
- Global administrator, User administrator, or Identity Governance administrator to manage access of reviews on groups and applications. Global administrators and Privileged Role administrators can manage reviews of role-assignable groups See [Use Azure AD groups to manage role assignments](../roles/groups-concept.md)
- Security readers have read access.
 
For more information, see [License requirements](access-reviews-overview.md#license-requirements).

 
## View the status of an access review
 
You can track the progress of access reviews as they are completed.
 
1. Sign in to the Azure portal and open the [Identity Governance page](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).
 
1. In the left menu, click **Access reviews**.
 
1. In the list, click an access review.
 
 
    On the **Overview** page, you can see the progress of the **Current** instance of the review. If there is not an active instance open at the time, you will see information on the previous instance. No access rights are changed in the directory until the review is completed.
 
     ![Review of All company group](./media/complete-access-review/all-company-group.png)
 
    All blades under **Current** are only viewable during the duration of each review instance. 
    > [!NOTE]
    > While the **Current** access review only shows information about the active review instance, you can get information about reviews yet to take place in the **Series** under the **Scheduled review** section.
 
    The Results page provides more information on each user under review in the instance, including the ability to Stop, Reset, and Download results.
 
    ![Review guest access across Microsoft 365 groups](./media/complete-access-review/all-company-group-results.png)
 
    If you are viewing an access review that reviews guest access across Microsoft 365 groups, the Overview blade lists each group in the review. 
   
    ![review guest access across Microsoft 365 groups](./media/complete-access-review/review-guest-access-across-365-groups.png)
 
    Click on a group to see the progress of the review on that group, also to Stop, Reset, Apply, and Delete.
 
   ![review guest access across Microsoft 365 groups in detail](./media/complete-access-review/progress-group-review.png)
 
1. If you want to stop an access review before it has reached the scheduled end date, click the **Stop** button.
 
    When you stop a review, reviewers will no longer be able to give responses. You can't restart a review after it's stopped.
 
1. If you're no longer interested in the access review, you can delete it by clicking the **Delete** button.
 
## Retrieve the results
 
To view the results for a review, click the **Results** page. To view just a user's access, in the Search box, type the display name or user principal name of a user whose access was reviewed.
 
![Retrieve results for an access review](./media/complete-access-review/retrieve-results.png) 
 
 
To view the results of a completed instance of an access review that is recurring, click **Review history**, then select the specific instance from the list of completed access review instances, based on the instance's start and end date. The results of this instance can be obtained from the **Results** page. Recurring access reviews allow you to have a constant picture of access to resources that may need to be updated more often than one-time access reviews.
 
To retrieve the results of an access review, both in-progress or completed, click the **Download** button. The resulting CSV file can be viewed in Excel or in other programs that open UTF-8 encoded CSV files.


 

## Apply the changes
 
If **Auto apply results to resource** was enabled based on your selections in **Upon completion settings**, auto-apply will be executed once a review instance completes, or earlier if you manually stop the review.
 
If **Auto apply results to resource** wasn't enabled for the review, navigate to **Review History** under **Series** after the review duration ends or the review was stopped early, and click on the instance of the review you’d like to Apply.
 
![Apply access review changes](./media/complete-access-review/apply-changes.png)
 
Click **Apply** to manually apply the changes. If a user's access was denied in the review, when you click **Apply**, Azure AD removes their membership or application assignment.
 
![Apply access review changes button](./media/complete-access-review/apply-changes-button.png)
 
The status of the review will change from **Completed** through intermediate states such as **Applying** and finally to state **Result applied**. You should expect to see denied users, if any, being removed from the group membership or application assignment in a few minutes.
 
Manually or automatically applying results doesn't have an effect on a group that originates in an on-premises directory. If you want to change a group that originates on-premises, download the results and apply those changes to the representation of the group in that directory.

> [!NOTE]
> Some denied users are unable to have results applied to them. Scenarios where this could happen include:
> - Reviewing members of a synced on-premises Windows AD group: If the group is synced from on-premises  Windows AD, the group cannot be managed in Azure AD and therefore membership cannot be changed.
> - Reviewing a resource (role, group, application) with nested groups assigned: For users who have membership through a nested group, we will not remove their membership to the nested group and therefore they will retain access to the resource being reviewed.
> - User not found / other errors can also result in an apply result not being supported.
 

## Actions taken on denied guest users in an access review
 
On review creation, the creator can choose between two options for denied guest users in an access review. 
 - Denied guest users can have their access to the resource removed. This is the default.
 - The denied guest user can be blocked from signing in for 30 days, then deleted from the tenant. During the 30-day period the guest user is able to be restored access to the tenant by an administrator. After the 30-day period is completed, if the guest user has not had access to the resource granted to them again, they will be removed from the tenant permanently. In addition, using the Azure Active Directory portal, a Global Administrator can explicitly [permanently delete a recently deleted user](../fundamentals/active-directory-users-restore.md) before that time period is reached. Once a user has been permanently deleted, the data about that guest user will be removed from active access reviews. Audit information about deleted users remains in the audit log.


## Next steps
 
- [Manage access reviews](manage-access-review.md) 
- [Create an access review of groups or applications](create-access-review.md)
- [Create an access review of users in an Azure AD administrative role](../privileged-identity-management/pim-create-azure-ad-roles-and-resource-roles-review.md)

