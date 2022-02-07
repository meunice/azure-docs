---
title: 'Tutorial: Anomaly detection with Cognitive Services'
description: Learn how to use Cognitive Services for anomaly detection in Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: sngun, garye
ms.date: 07/01/2021
author: nelgson
ms.author: negust
ms.custom: ignite-fall-2021
---

# Tutorial: Anomaly detection with Cognitive Services

In this tutorial, you'll learn how to easily enrich your data in Azure Synapse Analytics with [Azure Cognitive Services](../../cognitive-services/index.yml). You'll use [Anomaly Detector](../../cognitive-services/anomaly-detector/index.yml) to find anomalies. A user in Azure Synapse can simply select a table to enrich for detection of anomalies.

This tutorial covers:

> [!div class="checklist"]
> - Steps for getting a Spark table dataset that contains time series data.
> - Use of a wizard experience in Azure Synapse to enrich data by using Anomaly Detector in Cognitive Services.

If you don't have an Azure subscription, [create a free account before you begin](https://azure.microsoft.com/free/).

## Prerequisites

- [Azure Synapse Analytics workspace](../get-started-create-workspace.md) with an Azure Data Lake Storage Gen2 storage account configured as the default storage. You need to be the *Storage Blob Data Contributor* of the Data Lake Storage Gen2 file system that you work with.
- Spark pool in your Azure Synapse Analytics workspace. For details, see [Create a Spark pool in Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Completion of the pre-configuration steps in the [Configure Cognitive Services in Azure Synapse](tutorial-configure-cognitive-services-synapse.md) tutorial.

## Sign in to the Azure portal

Sign in to the [Azure portal](https://portal.azure.com/).

## Create a Spark table

You need a Spark table for this tutorial.

1. Download the following notebook file that contains code to generate a Spark table: [prepare_anomaly_detector_data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149577).

1. Upload the file to your Azure Synapse workspace.

   ![Screenshot that shows selections for uploading a notebook.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00a.png)

1. Open the notebook file and select **Run All** to run all cells.

   ![Screenshot that shows selections for creating a Spark table.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00b.png)

1. A Spark table named **anomaly_detector_testing_data** should now appear in the default Spark database.

## Open the Cognitive Services wizard

1. Right-click the Spark table created in the previous step. Select **Machine Learning** > **Predict with a model** to open the wizard.

   ![Screenshot that shows selections for opening the scoring wizard.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00g.png)

2. A configuration panel appears, and you're asked to select a Cognitive Services model. Select **Anomaly Detector**.

   ![Screenshot that shows selection of Anomaly Detector as a model.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00c.png)

## Configure Anomaly Detector

Provide the following details to configure Anomaly Detector:

- **Azure Cognitive Services linked service**: As part of the prerequisite steps, you created a linked service to your [Cognitive Services](tutorial-configure-cognitive-services-synapse.md) . Select it here.

- **Granularity**: The rate at which your data is sampled. Choose **monthly**. 

- **Timestamp column**: The column that represents the time of the series. Choose **timestamp (string)**.

- **Time series value column**: The column that represents the value of the series at the time specified by the Timestamp column. Choose **value (double)**.

- **Grouping column**: The column that groups the series. That is, all rows that have the same value in this column should form one time series. Choose **group (string)**.

When you're done, select **Open notebook**. This will generate a notebook for you with PySpark code that uses Azure Cognitive Services to detect anomalies.

![Screenshot that shows configuration details for Anomaly Detector.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-config.png)

## Run the notebook

The notebook that you just opened uses the [SynapseML library](https://github.com/microsoft/SynapseML) to connect to Cognitive Services. The Azure Cognitive Services linked service that you provided allow you to securely reference your cognitive service from this experience without revealing any secrets.

You can now run all cells to perform anomaly detection. Select **Run All**. [Learn more about Anomaly Detector in Cognitive Services](../../cognitive-services/anomaly-detector/index.yml).

![Screenshot that shows anomaly detection.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-notebook.png)

## Next steps

- [Tutorial: Sentiment analysis with Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Machine learning model scoring in Azure Synapse dedicated SQL pools](tutorial-sql-pool-model-scoring-wizard.md)
- [Machine Learning capabilities in Azure Synapse Analytics](what-is-machine-learning.md)
