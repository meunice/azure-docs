---
title: Embed player widget in Power BI
description: You can use Azure Video Analyzer for continuous video recording or event-based recording. This article talks about how to embed videos in Microsoft Power BI to provide a customizable UI for your users.
ms.service: azure-video-analyzer
ms.topic: how-to
ms.date: 11/04/2021
ms.custom: ignite-fall-2021
---

# Embed player widget in Power BI


Azure Video Analyzer enables you to [record](detect-motion-record-video-clips-cloud.md) video and associated inference metadata to your Video Analyzer cloud resource. Video Analyzer has a [Player Widget](player-widget.md) - an easy-to-embed widget allowing client apps to playback video and inference metadata.

Dashboards are an insightful way to monitor your business and view all your most important metrics at a glance. A Power BI dashboard is a powerful tool to combine video with multiple sources of data including telemetry from IoT Hub. In this tutorial, you will learn how to add one or more player widgets to a dashboard using [Microsoft Power BI](https://powerbi.microsoft.com/) web service.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/power-bi/embed-block-diagram.svg" alt-text="Block diagram to embed Azure Video Analyzer player widget in Microsoft Power BI.":::

## Suggested pre-reading

- Azure Video Analyzer [player widget](player-widget.md)
- Introduction to [Power BI dashboards](/power-bi/create-reports/service-dashboards)

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) if you don't already have one.
- Complete either [Detect motion in a (simulated) live video, record the video to the Video Analyzer account](detect-motion-record-video-clips-cloud.md) or [Continuous video recording](continuous-video-recording.md) - a pipeline with video sink is required.

  > [!NOTE]
  > Your video analyzer account should have a minimum of one video recorded to proceed. Check for list of videos by logging into your Azure Video Analyzer account > Videos > Video Analyzer section.

- A [Power BI](https://powerbi.microsoft.com/) account.

## Create a token

1. Follow steps to [create a token](access-policies.md#creating-a-token).
2. Make sure to save values generated for _Issuer, Audience, Key Type, Algorithm, Key Id, RSA Key Modulus, RSA Key Exponent, Token_. You will need these values when creating an access policy below.

## Get embed code for player widget

1. Login to [Azure portal](https://portal.azure.com/) with your credentials. Locate your Video Analyzer account used to complete the prerequisites and open the Video Analyzer account pane.
2. Follow steps to [Create an access policy](access-policies.md#creating-an-access-policy).
3. Select **Videos** in the **Video Analyzer** section.
4. Select any video from the list.
5. Click on **Widget** setup. A pane **Use widget in your application** opens on the right-hand side. Scroll down to **Option 2 – using HTML** and copy the code and paste it in a text editor. Click the **Close** button.

   :::image type="content" source="./media/power-bi/widget-code.png" alt-text="Screenshot of copy widget HTML code from AVA portal and save for later.":::

6. Edit the HTML code copied in step 5 to replace values for
   - Token **AVA-API-JWT-TOKEN** - replace with the value of Token that you saved in the “Create a token” step. Ensure to remove the angular brackets.
   - Optional – you can make other UI changes in this code for example - changing the header from “Example Player widget” to “Continuous Video Recording”.

## Add widget in Power BI dashboard

1. Open the [Power BI service](http://app.powerbi.com/) in your browser. From the navigation pane, select **My Workspace**

   :::image type="content" source="./media/power-bi/powerbi-ws.png" alt-text="Screenshot of Power BI workspace home page.":::

2. Create a new dashboard by clicking **New** > **Dashboard** or open an existing dashboard. Select the **Edit** drop down arrow and then **Add a tile**. Select **Web content** > **Next**.
3. In **Add web content tile**, enter your **Embed code** from previous section. Click **Apply**.

   :::image type="content" source="./media/power-bi/embed-code.png" alt-text="Screenshot of embedding the html code in a Power BI dashboard tile.":::

4. You will see a player widget pinned to the dashboard with a video.

   :::image type="content" source="./media/power-bi/one-player.png" alt-text="Screenshot of one video player widget added.":::

5. To add more videos from Azure Video Analyzer Videos section, follow the same steps in this section.

> [!NOTE]
> To add multiple videos from the same Video Analyzer account, a single set of access policy and token is sufficient.

Here is a sample of multiple videos pinned to a single Power BI dashboard.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/power-bi/two-players.png" alt-text="Screenshot of two video player widgets added as an example.":::

## Next steps

- [Real-time visualization of AI inference events in Power BI](visualize-ai-events-power-bi.md)
- Learn more about the [widget API](https://github.com/Azure/video-analyzer-widgets)
