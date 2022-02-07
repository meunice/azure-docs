---
title: Compare load testing runs to find regressions
titleSuffix: Azure Load Testing
description: 'Learn how you can visually compare multiple test runs with Azure Load Testing to better understand performance regressions.'
services: load-testing
ms.service: load-testing
ms.author: nicktrog
author: ntrogh
ms.date: 11/30/2021
ms.topic: how-to

---

# Identify performance regressions by comparing load test runs

In this article, you'll learn how to identify performance regressions by visually comparing multiple load test runs in the Azure Load Testing Preview dashboard.

A test run contains client-side and server-side metrics. The test engine reports client-side metrics, such as the number of virtual users. The server-side metrics provide application- and resource-specific information.

By overlaying multiple metrics charts, you can more easily pinpoint performance changes and identify which application component is causing problems.

There are two entry points for comparing load test runs in the Azure portal:

- Starting from the test runs page, select multiple results to compare.
- Starting from a specific test run, select other results to compare the runs with.

> [!IMPORTANT]
> Azure Load Testing is currently in preview. For legal terms that apply to Azure features that are in beta, in preview, or otherwise not yet released into general availability, see the [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## Prerequisites

- An Azure account with an active subscription. If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.  

- An Azure Load Testing resource with a test plan that has multiple test runs. To create a Load Testing resource, see [Create and run a load test](./quickstart-create-and-run-load-test.md).

    > [!NOTE]
    > If you want to compare a test run, it needs to be in a *Done*, *Stopped*, or *Failed* state.
    
## Compare test runs from the test runs page

In this section, you'll compare multiple results by selecting runs from the test runs page.

1. Sign in to the [Azure portal](https://portal.azure.com) by using the credentials for your Azure subscription.

1. Go to your Azure Load Testing resource and then, on the left pane, select **Tests**.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/choose-test-from-list.png" alt-text="Screenshot that shows the list of tests for a Load Testing resource.":::

    You can also use the filters to find your load test.

1. In the list of tests, select the test whose runs you want to compare.

1. Select two or more test runs, and then select **Compare**.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/compare-test-results-from-list.png" alt-text="Screenshot that shows a list of test runs and the 'Compare' button.":::

    > [!NOTE]
    > You can choose a maximum of five test runs to compare.

    The selected test runs are presented in the dashboard. Each run is shown as an overlay in the different charts.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/compare-screen.png" alt-text="Screenshot of the 'Compare' page, displaying a comparison of two test runs.":::

    You can use filters to customize the graphs. There are separate filters for the client and server metrics.

    > [!TIP]
    > The time filter is based on the relative duration of the tests. A value of zero indicates the beginning of the test, and the maximum value marks the duration of the longest test run. For client-side metrics, test runs show only data for the duration of the test.

## Compare test runs from the run details page

In this section, you'll use the test run details page and add other test runs to compare.

1. Go to the test run details page, and then select **Compare**.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/test-run-details.png" alt-text="Screenshot of the 'Test run details' page, displaying the 'Compare' button.":::

1. On the **Compare** page, select two or more test runs that you want to compare.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/choose-runs-to-compare.png" alt-text="Screenshot of the 'Compare' page, displaying test runs to be compared.":::

    > [!NOTE]
    > You can choose a maximum of five test runs to compare.

1. Select **Compare**.

    The selected test runs are presented in the dashboard. Each run is shown as an overlay in the different charts.

    :::image type="content" source="media/how-to-compare-multiple-test-runs/compare-screen.png" alt-text="Screenshot of the 'Compare' page, displaying a comparison of two test runs.":::

    You can use filters to customize the graphs. There are separate filters for the client and server metrics.

## Next steps

- For information about high-scale load tests, see [Set up a high-scale load test](./how-to-high-scale-load.md).

- To learn about performance test automation, see [Configure automated performance testing](./tutorial-cicd-azure-pipelines.md).
