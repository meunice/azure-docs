---
title: Delete backups of an Azure NetApp Files volume  | Microsoft Docs
description: Describes how to delete individual backups that you no longer need to keep for a volume.
services: azure-netapp-files
documentationcenter: ''
author: b-hchen
manager: ''
editor: ''

ms.assetid:
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.topic: how-to
ms.date: 09/27/2021
ms.author: anfdocs
---
# Delete backups of a volume 

You can delete individual backups that you no longer need to keep for a volume. Deleting backups will delete the associated objects in your Azure Storage account, resulting in space saving.  

You cannot delete the latest backup.  

## Steps

1. Select **Volumes**.
2. Navigate to **Backups**.
3. From the backup list, select the backup to delete. Click the three dots (`…`) to the right of the backup, then click **Delete** from the Action menu.

    ![Screenshot that shows the Delete menu for backups.](../media/azure-netapp-files/backup-action-menu-delete.png)

## Next steps  

* [Understand Azure NetApp Files backup](backup-introduction.md)
* [Requirements and considerations for Azure NetApp Files backup](backup-requirements-considerations.md)
* [Resource limits for Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Configure policy-based backups](backup-configure-policy-based.md)
* [Configure manual backups](backup-configure-manual.md)
* [Manage backup policies](backup-manage-policies.md)
* [Search backups](backup-search.md)
* [Restore a backup to a new volume](backup-restore-new-volume.md)
* [Disable backup functionality for a volume](backup-disable.md)
* [Volume backup metrics](azure-netapp-files-metrics.md#volume-backup-metrics)
* [Azure NetApp Files backup FAQs](faq-backup.md)
