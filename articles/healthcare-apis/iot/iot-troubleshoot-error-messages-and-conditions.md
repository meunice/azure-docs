---
title: Troubleshoot IoT connector error messages, conditions, and fixes - Azure Healthcare APIs
description: This article helps users troubleshoot IoT connector errors/conditions and provides fixes and solutions.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: troubleshooting
ms.date: 12/10/2021
ms.author: jasteppe
---

# Troubleshoot IoT connector error messages and conditions

> [!IMPORTANT]
> Azure Healthcare APIs is currently in PREVIEW. The [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) include additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability. 

This article provides steps for troubleshooting and fixing IoT connector error messages and conditions.

> [!IMPORTANT]
> Having access to IoT connector Metrics is essential for monitoring and troubleshooting.  IoT connector assists you to do these actions through [Metrics](./how-to-display-metrics.md).

> [!TIP]
> Check out the [IoMT Connector Data Mapper](https://github.com/microsoft/iomt-fhir/tree/master/tools/data-mapper) tool for editing, testing, and troubleshooting IoT connector Device and FHIR destination mappings. Export mappings for uploading to IoT connector in the Azure portal or use with the [open-source version](https://github.com/microsoft/iomt-fhir) of IoT connector.

> [!NOTE]
> When opening an [Azure Technical Support](https://azure.microsoft.com/support/create-ticket/) ticket for IoT connector, include [copies of your Device and FHIR destination mappings](./how-to-create-mappings-copies.md) to assist in the troubleshooting process.

## Error messages and conditions

### The operation being performed by IoT connector

This property represents the operation being performed by IoT connector when the error has occurred. An operation generally represents the data flow stage while processing a device message. Below is a list of possible values for this property.

> [!NOTE]
> For information about the different stages of data flow in IoT connector, see [IoT connector data flow](iot-data-flow.md).

|Data flow stage|Description|
|---------------|-----------|
|Setup|The setup data flow stage is the operation specific to setting up your instance of the IoT connector.|
|Normalization|Normalization is the data flow stage where the device data gets normalized.|
|Grouping|The grouping data flow stage where the normalized data gets grouped.|
|FHIRConversion|FHIRConversion is the data flow stage where the grouped-normalized data is transformed into an FHIR resource.|
|Unknown|Unknown is the operation type that's unknown when an error occurs.|

#### The severity of the error

This property represents the severity of the occurred error. Below is a list of possible values for this property.

|Severity|Description|
|---------------|-----------|
|Warning|Some minor issue exists in the data flow process, but processing of the device message doesn't stop.|
|Error|This message occurs when the processing of a specific device message has run into an error and other messages may continue to execute as expected.|
|Critical|This error is when some system level issue exists with the IoT connector and no messages are expected to process.|

#### The type of error

This property signifies a category for a given error, which it basically represents a logical grouping for similar types of errors. Below is a list of possible values for this property.

|Error type|Description|
|----------|-----------|
|`DeviceTemplateError`|This error type is related to the Device mapping.|
|`DeviceMessageError`|This error type occurs when processing a specific device message.|
|`FHIRTemplateError`|This error type is related to the FHIR destination mapping|
|`FHIRConversionError`|This error type occurs when transforming a message into a FHIR resource.|
|`FHIRResourceError`|This error type is related to existing resources in the FHIR service that are referenced by the IoT connector.|
|`FHIRServerError`|This error type occurs when communicating with the FHIR service.|
|`GeneralError`|This error type is about all other types of errors.|

#### The name of the error

This property provides the name for a specific error. Below is the list of all error names with their description and associated error type(s), severity, and data flow stage(s).

|Error name|Description|Error type(s)|Error severity|Data flow stage(s)|
|----------|-----------|-------------|--------------|------------------|
|`MultipleResourceFoundException`|This error occurs when multiple patient or device resources are found in the FHIR service for the respective identifiers present in the device message.|`FHIRResourceError`|Error|`FHIRConversion`|
|`TemplateNotFoundException`|A device or FHIR destination mapping that isn't configured with the instance of IoT connector.|`DeviceTemplateError`, `FHIRTemplateError`|Critical|`Normalization`, `FHIRConversion`|
|`CorrelationIdNotDefinedException`|The correlation ID isn't specified in the Device mapping. `CorrelationIdNotDefinedException` is a conditional error that occurs only when the FHIR Observation must group device measurements using a correlation ID because it's not configured correctly.|`DeviceMessageError`|Error|Normalization|
|`PatientDeviceMismatchException`|This error occurs when the device resource on the FHIR service has a reference to a patient resource. This error type means it doesn't match with the patient identifier present in the message.|`FHIRResourceError`|Error|`FHIRConversionError`|
|`PatientNotFoundException`|No Patient FHIR resource is referenced by the Device FHIR resource associated with the device identifier present in the device message. Note this error will only occur when the IoT connector instance is configured with the *Lookup* resolution type.|`FHIRConversionError`|Error|`FHIRConversion`|
|`DeviceNotFoundException`|No device resource exists on the FHIR service associated with the device identifier present in the device message.|`DeviceMessageError`|Error|Normalization|
|`PatientIdentityNotDefinedException`|This error occurs when expression to parse patient identifier from the device message isn't configured on the Device mapping or patient identifer isn't present in the device message. Note this error occurs only when IoT connector's resolution type is set to *Create*.|`DeviceTemplateError`|Critical|Normalization|
|`DeviceIdentityNotDefinedException`|This error occurs when the expression to parse device identifier from the device message isn't configured on the Device mapping or device identifer isn't present in the device message.|`DeviceTemplateError`|Critical|Normalization|
|`NotSupportedException`|Error occurred when device message with unsupported format is received.|`DeviceMessageError`|Error|Normalization|

### IoT connector resource

|Message|Displayed|Condition|Fix| 
|-------|---------|---------|---|
|The maximum number of resource type `iotconnectors` has been reached.|API and Azure portal|IoT connector subscription quota is reached (default is 10 IoT connectors per workspace and 10 workspaces per subscription).|Delete one of the existing instances of IoT connector. Use a different subscription that hasn't reached the subscription quota. Request a subscription quota increase.
|Invalid `deviceMapping` mapping. Validation errors: {List of errors}|API and Azure portal|The `properties.deviceMapping` provided in the IoT connector Resource provisioning request is invalid.|Correct the errors in the mapping JSON provided in the `properties.deviceMapping` property.
|`fullyQualifiedEventHubNamespace` is null, empty, or formatted incorrectly.|API and Azure portal|The IoT connector provisioning request `properties.ingestionEndpointConfiguration.fullyQualifiedEventHubNamespace` is not valid.|Update the IoT connector `properties.ingestionEndpointConfiguration.fullyQualifiedEventHubNamespace` to the correct format. Should be `{YOUR_NAMESPACE}.servicebus.windows.net`.
|Ancestor resources must be fully provisioned before a child resource can be provisioned.|API|The parent workspace is still provisioning.|Wait until the parent workspace provisioning has completed and submit the provisioning request again.
|`Location` property of child resources must match the `Location` property of parent resources.|API|The IoT connector provisioning request `location` property is different from the parent workspace `location` property.|Set the `location` property of the IoT connector in the provisioning request to the same value as the parent workspace `location` property.

### Destination resource

|Message|Displayed|Condition|Fix| 
|-------|---------|---------|---|
|The maximum number of resource type `iotconnectors/destinations` has been reached.|API and Azure portal|IoT connector Destination Resource quota is reached and the default is 1 per IoT connector).|Delete the existing instance of IoT connector Destination Resource. Only one Destination Resource is permitted per IoT connector.
|The `fhirServiceResourceId` provided is invalid.|API and Azure portal|The `properties.fhirServiceResourceId` provided in the Destination Resource provisioning request is not a valid resource ID for an instance of the Azure Healthcare APIs FHIR service.|Ensure the resource ID is formatted correctly, and make sure the resource ID is for an Azure Healthcare APIs FHIR service instance. The format should be: `/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP_NAME}/providers/Microsoft.HealthcareApis/workspaces/{workspace_NAME}/fhirservices/{FHIR_SERVICE_NAME}`
|Ancestor resources must be fully provisioned before a child resource can be provisioned.|API|The parent workspace or the parent IoT connector is still provisioning.|Wait until the parent workspace or the parent IoT connector provisioning completes, and then submit the provisioning request again.
|`Location` property of child resources must match the `Location` property of parent resources.|API|The Destination provisioning request `location` property is different from the parent IoT connector `location` property.|Set the `location` property of the Destination in the provisioning request to the same value as the parent IoT connector `location` property.

## Why is IoT connector data not showing up in the FHIR service?

|Potential issues|Fixes|
|----------------|-----|
|Data is still being processed.|Data is egressed to the FHIR service in batches (every ~5 minutes).  It’s possible the data is still being processed and extra time is needed for the data to be persisted in the FHIR service.|
|Device mapping hasn't been configured.|Configure and save conforming Device mapping.|
|FHIR destination mapping hasn't been configured.|Configure and save conforming FHIR destination mapping.|
|The device message doesn't contain an expected expression defined in the Device mapping.|Verify `JsonPath` expressions defined in the Device mapping match tokens defined in the device message.|
|A Device Resource hasn't been created in the FHIR service (Resolution Type: Lookup only)*.|Create a valid Device Resource in the FHIR service. Ensure the Device Resource contains an identifier that matches the device identifier provided in the incoming message.|
|A Patient Resource hasn't been created in the FHIR service (Resolution Type: Lookup only)*.|Create a valid Patient Resource in the FHIR service.|
|The `Device.patient` reference isn't set, or the reference is invalid (Resolution Type: Lookup only)*.|Make sure the Device Resource contains a valid [Reference](https://www.hl7.org/fhir/device-definitions.html#Device.patient) to a Patient Resource.| 

*Reference [Quickstart: Deploy IoT connector using Azure portal](deploy-iot-connector-in-azure.md) for a functional description of the IoT connector resolution types (For example: Lookup or Create).

## Next steps

In this article, you learned how to troubleshoot IoT connector error messages and conditions. To learn how to troubleshoot IoT connector Device and FHIR destination mappings, see

>[!div class="nextstepaction"]
>[Troubleshoot IoT connector Device and FHIR destination mappings](iot-troubleshoot-mappings.md)

(FHIR&#174;) is a registered trademark of [HL7](https://hl7.org/fhir/) and is used with the permission of HL7.
