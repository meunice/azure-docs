---
title: "Create, view, and manage Metric Alerts Using Azure Monitor"
description: Learn how to use Azure portal or CLI to create, view, and manage metric alert rules.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 11/07/2021
---
# Create, view, and manage metric alerts using Azure Monitor

Metric alerts in Azure Monitor provide a way to get notified when one of your metrics crosses a threshold. Metric alerts work on a range of multi-dimensional platform metrics, custom metrics, Application Insights standard and custom metrics. In this article, we will describe how to create, view, and manage metric alert rules through Azure portal and Azure CLI. You can also create metric alert rules using Azure Resource Manager templates, which are described in [a separate article](./alerts-metric-create-templates.md).

You can learn more about how metric alerts work from [metric alerts overview](./alerts-metric-overview.md).

## Create with Azure portal

The following procedure describes how to create a metric alert rule in Azure portal:

1. In [Azure portal](https://portal.azure.com), click on **Monitor**. The Monitor blade consolidates all your monitoring settings and data in one view.

2. Click **Alerts**, then expand the **+ Create** menu and select **Alert rule**.

    > [!TIP]
    > Most resource blades also have **Alerts** in their resource menu under **Monitoring**, you could create alert rules from there as well.

3. In the **Scope** tab, click **Select scope**. Then, in the context pane that loads, select the target resource(s) that you want to alert on. Use **Filter by subscription**, **Filter by resource type**, and **Filter by location** drop-downs to find the resourcevirtual you want to monitor. You can also use the search bar to find your resource.

4. If the selected resource has metrics you can create alert rules on, **Available signal types** on the bottom right will include metrics. You can view the full list of resource types supported for metric alerts in this [article](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

5. Once you have selected a target resource, click **Done**.

6. Proceed to the **Condition** tab. Then, in the context pane that loads, you will see a list of signals supported for the resource. Select the metric you want to create an alert on.

7. You will see a chart showing the metric's behavior for the last six hours. Use the **Chart period** dropdown to select to see longer history for the metric.

8. If the metric has dimensions, you will see a dimensions table presented. Optionally, select one or more values per dimension.
    - The displayed dimension values are based on metric data from the last day.
    - If the dimension value you're looking for isn't displayed, click "Add custom value" to add a custom dimension value.
    - You can also choose **Select all current and future values** for any of the dimensions. This will dynamically scale the selection to all current and future values for the dimension.

    The metric alert rule will evaluate the condition for all combinations of values selected. [Learn more about how alerting on multi-dimensional metrics works](./alerts-metric-overview.md).
    
    > [!NOTE]
    > Using "All" as a dimension value is equivalent to choosing "Select all current and future values".

9. Select the **Threshold** type, **Operator**, and **Aggregation type**. This will determine the logic that the metric alert rule will evaluate.
    - If you are using a **Static** threshold, continue to define a **Threshold value**. The metric chart can help determine what might be a reasonable threshold.
    - If you are using a **Dynamic** threshold, continue to define the **Threshold sensitivity**. The metric chart will display the calculated thresholds based on recent data. [Learn more about Dynamic Thresholds condition type and sensitivity options](../alerts/alerts-dynamic-thresholds.md).

10. Optionally, refine the condition by adjusting **Aggregation granularity** and **Frequency of evaluation**. 

11. Click **Done**.

12. Optionally, add another criteria if you want to monitor a complex alert rule. Currently users can have alert rules with Dynamic Thresholds criteria as a single criterion.

13. Proceed to the **Actions** tab, where you can define what actions and notifications are triggered when the alert rule generates an alert. You can add an action group to the alert rule either by selecting an existing action group or by creating a new action group.

14. Proceed to the **Details** tab. Under **Project details**, select the subscription and resource group in which the alert rule resource will be saved. Under **Alert rule details**, specify the **Severity** and **Alert rule name**. You can also provide an **Alert rule description**, select if the alert rule should be enabled when created, and if it should **Automatically resolve alerts** (instructs the alert rule to maintain a state, and not fire continuously if there's already a fired alert on the same condition).

15. Proceed to the **Tags**, where you can set tags on the alert rule you're creating.
16. Proceed to the **Review + create** tab, where you can review your selections before creating the alert rule. A quick automatic validation will also be performed, notifying you in case any information or missing or needs to be correct. Once you're ready to create the alert rule, Click **Create**.


## View and manage with Azure portal

You can view and manage metric alert rules using the Manage Rules blade under Alerts. The procedure below shows you how to view your metric alert rules and edit one of them.

1. In Azure portal, navigate to **Monitor**.

2. Click on **Alerts**, and then on **Alert rules**.

3. In the **Alert rules** blade, you can view all your alert rules across subscriptions. You can further filter the rules using  **Resource group**, **Resource type**, and **Resource**. If you want to see only metric alerts, select **Signal type** as *Metrics*.

    > [!TIP]
    > In the **Alert rules** blade, you can select multiple alert rules and enable/disable them. This might be useful when certain target resources need to be put under maintenance.

4. Click on the name of the metric alert rule you want to edit.

5. In this page, you can change various settingd of the alert rule.

    > [!NOTE]
    > You can't edit the **Alert Rule Name** after the metric alert rule is created.

6. Click **Save** to save your edits.


## With Azure CLI

The previous sections described how to create, view, and manage metric alert rules using Azure portal. This section will describe how to do the same using cross-platform [Azure CLI](/cli/azure/get-started-with-azure-cli). Quickest way to start using Azure CLI is through [Azure Cloud Shell](../../cloud-shell/overview.md). For this article, we will use Cloud Shell.

1. Go to Azure portal, click on **Cloud Shell**.

2. At the prompt, you can use commands with ``--help`` option to learn more about the command and how to use it. For example, the following command shows you the list of commands available for creating, viewing, and managing metric alerts

    ```azurecli
    az monitor metrics alert --help
    ```

3. You can create a simple metric alert rule that monitors if average Percentage CPU on a VM is greater than 90

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. You can view all the metric alerts in a resource group using the following command

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. You can see the details of a particular metric alert rule using the name or the resource ID of the rule.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. You can disable a metric alert rule using the following command.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. You can delete a metric alert rule using the following command.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## With PowerShell

Metric alert rules have dedicated PowerShell cmdlets available:

- [Add-AzMetricAlertRuleV2](/powershell/module/az.monitor/add-azmetricalertrulev2): Create a new metric alert rule or update an existing one.
- [Get-AzMetricAlertRuleV2](/powershell/module/az.monitor/get-azmetricalertrulev2): Get one or more metric alert rules.
- [Remove-AzMetricAlertRuleV2](/powershell/module/az.monitor/remove-azmetricalertrulev2): Delete a metric alert rule.

## With REST API

- [Create Or Update](/rest/api/monitor/metricalerts/createorupdate): Create a new metric alert rule or update an existing one.
- [Get](/rest/api/monitor/metricalerts/get): Get a specific metric alert rule.
- [List By Resource Group](/rest/api/monitor/metricalerts/listbyresourcegroup): Get a list of metric alert rules in a specific resource group.
- [List By Subscription](/rest/api/monitor/metricalerts/listbysubscription): Get a list of metric alert rules in a specific subscription.
- [Update](/rest/api/monitor/metricalerts/update): Update a metric alert rule.
- [Delete](/rest/api/monitor/metricalerts/delete): Delete a metric alert rule.

## Next steps

- [Create metric alerts using Azure Resource Manager Templates](./alerts-metric-create-templates.md)
- [Understand how metric alerts work](./alerts-metric-overview.md)
- [Understand how metric alerts with Dynamic Thresholds condition work](../alerts/alerts-dynamic-thresholds.md)
- [Understand the web hook schema for metric alerts](./alerts-metric-near-real-time.md#payload-schema)
- [Troubleshooting problems in metric alerts](./alerts-troubleshoot-metric.md)
