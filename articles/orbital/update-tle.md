---
title: 'How to update the spacecraft TLE on Azure Orbital Earth Observation service' 
description: 'Update the spacecraft TLE'
author: wamota
ms.service: orbital
ms.topic: how-to
ms.custom: public-preview
ms.date: 11/16/2021
ms.author: wamota
# Customer intent: As a satellite operator, I want to ingest data from my satellite into Azure.
---

# Update the spacecraft TLE

Update the TLE of an existing spacecraft resource.

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- A registered spacecraft. Learn more on how to [register spacecraft](register-spacecraft.md).

## Sign in to Azure

Sign in to the [Azure portal - Orbital Preview](https://aka.ms/orbital/portal).

## Update the spacecraft TLE

1.	In the Azure portal search box, enter **Spacecrafts**. Select **Spacecrafts** in the search results.
2.	In the **Spacecrafts** page, select the name of the spacecraft for which to update the ephemeris.
3.	Select **Ephemeris** on the left menu bar of the spacecraft’s overview.
4.	In Ephemeris, enter this information in each of the required fields:

    | **Field** | **Value** |
    | --- | --- |
    | TLE title line | Spacecraft updated TLE Title Line |
    | TLE Line 1 | Spacecraft updated TLE Line 1 |
    | TLE Line 2 | Spacecraft updated TLE Line 2 |

    :::image type="content" source="media/orbital-eos-ephemeris.png" alt-text="Spacecraft TLE update" lightbox="media/orbital-eos-ephemeris.png":::

5. Select the **Submit** button.

## Next steps

- [How-to: Schedule a contact](schedule-contact.md)
- [How-to: Cancel a scheduled contact](delete-contact.md)