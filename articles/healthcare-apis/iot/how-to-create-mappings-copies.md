---
title: Create copies of IoT connector mappings templates - Azure Healthcare APIs
description: This article helps users create copies of their IoT connector Device and FHIR destination mappings templates.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 12/10/2021
ms.author: jasteppe
---

# How to create copies of Device and FHIR destination mappings

> [!IMPORTANT]
> Azure Healthcare APIs is currently in PREVIEW. The [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) include additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability. 

This article provides steps for creating copies of your IoT connector's Device and Fast Healthcare Interoperability Resources (FHIR&#174;) destination mappings that can be used outside of the Azure portal. These copies can be used for editing, troubleshooting, and archiving.

> [!TIP]
> Check out the [IoMT Connector Data Mapper](https://github.com/microsoft/iomt-fhir/tree/master/tools/data-mapper) tool for editing, testing, and troubleshooting IoT connector Device and FHIR destination mappings. Export mappings for uploading to IoT connector in the Azure portal or use with the [open-source version](https://github.com/microsoft/iomt-fhir) of IoT connector.

> [!NOTE]
> When opening an [Azure Technical Support](https://azure.microsoft.com/support/create-ticket/) ticket for the IoT connector, include [copies of your Device and FHIR destination mappings](./how-to-create-mappings-copies.md) to assist in the troubleshooting process.

## Copy creation process

1. Select **"IoT connectors"** on the left side of the Healthcare APIs workspace.

   :::image type="content" source="media/iot-troubleshoot/iot-connector-blade.png" alt-text="Select IoT connectors." lightbox="media/iot-troubleshoot/iot-connector-blade.png":::

2. Select the name of the **IoT connector** that you'll be copying the Device and FHIR destination mappings from.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="Select the IoT connector that you will be making mappings copies from" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

   > [!NOTE]
   > This process may also be used for copying and saving the contents of the **"Destination"** FHIR destination mappings.

3. Select the contents of the JSON and do a copy operation (for example: Press **Ctrl + C**). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="Select and copy contents of mappings" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

4. Do a paste operation (for example: Press **Ctrl + V**) into a new file within an editor like Microsoft Visual Studio Code or Notepad. Ensure to save the file with the **.json** extension.

## Next steps

In this article, you learned how to make file copies of IoT connector Device and FHIR destination mappings templates. To learn how to troubleshoot Destination and FHIR destination mappings, see

>[!div class="nextstepaction"]
>[Troubleshoot IoT connector Device and FHIR destination mappings](iot-troubleshoot-mappings.md)

(FHIR&#174;) is a registered trademark of [HL7](https://hl7.org/fhir/) and is used with the permission of HL7.
