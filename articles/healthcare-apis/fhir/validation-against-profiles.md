---
title: Validate FHIR resources against profiles in Azure Healthcare APIs
description: This article describes how to validate FHIR resources against profiles in the FHIR service.
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 12/22/2021
ms.author: cavoeg
---

# Validate FHIR resources against profiles in Azure Healthcare APIs

> [!IMPORTANT]
> Azure Healthcare APIs is currently in PREVIEW. The [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) include additional legal terms that apply to Azure features that are in beta, preview, or otherwise not yet released into general availability.

`$validate` is an operation in FHIR that allows you to ensure that a FHIR resource conforms to the base resource requirements or a specified profile. This is a valuable operation to ensure that the data in the FHIR server has the expected attributes and values.

In the [store profiles in the FHIR service](store-profiles-in-fhir.md) article, you walked through the basics of FHIR profiles and storing them. The FHIR service in the Azure Healthcare APIs (hereby called the FHIR service) allows validating resources against profiles to see if the resources conform to the profiles. This article will guide you through how to use `$validate` for validating resources against profiles. For more information about FHIR profiles outside of this article, visit 
[HL7.org](https://www.hl7.org/fhir/profiling.html).

## Validating resources against the profiles

FHIR resources can express their conformance to specific profiles. This allows the FHIR service to **validate** given resources against profiles. Validating a resource against a profile means checking if the resource conforms to the profile, including the specifications listed in `Resource.meta.profile` or in an Implementation Guide. There are two ways for you to validate your resource:

- You can use `$validate` operation against a resource that is already in the FHIR service. 
- You can include `$validate` when you create or update a resource. 

In both cases, you can decide by way of your FHIR service configuration what to do when the resource doesn't conform to your desired profile.

## Using $validate

The `$validate` operation checks whether the provided profile is valid, and whether the resource conforms to the specified profile. As mentioned in the [HL7 FHIR specifications](https://www.hl7.org/fhir/resource-operation-validate.html), you can also specify the mode for `$validate`, such as create and update:

- `create`: The server checks that the profile content is unique from the existing resources and that it's acceptable to be created as a new resource.
- `update`: Checks that the profile is an update against the nominated existing resource (that is no changes are made to the immutable fields).

The server will always return an `OperationOutcome` as the validation results.

## Validating an existing resource

To validate an existing resource, use `$validate` in a `GET` request:

`GET http://<your FHIR service base URL>/{resource}/{resource ID}/$validate`

For example:

`GET https://myworkspace-myfhirserver.fhir.azurehealthcareapis.com/Patient/a6e11662-def8-4dde-9ebc-4429e68d130e/$validate`

In this example, you're validating the existing Patient resource `a6e11662-def8-4dde-9ebc-4429e68d130e` against the base Patient resource. If it's valid, you'll get an `OperationOutcome` such as the following code example:

```json
{
    "resourceType": "OperationOutcome",
    "issue": [
        {
            "severity": "information",
            "code": "informational",
            "diagnostics": "All OK"
        }
    ]
}
```
If the resource isn't valid, you'll get an error code and an error message with details on why the resource is invalid. An example `OperationOutcome` gets returned with error messages and could look like the following code example:

```json
{
    "resourceType": "OperationOutcome",
    "issue": [
        {
            "severity": "error",
            "code": "invalid",
            "details": {
                "coding": [
                    {
                        "system": "http://hl7.org/fhir/dotnet-api-operation-outcome",
                        "code": "1028"
                    }
                ],
                "text": "Instance count for 'Patient.identifier.value' is 0, which is not within the specified cardinality of 1..1"
            },
            "location": [
                "Patient.identifier[1]"
            ]
        },
        {
            "severity": "error",
            "code": "invalid",
            "details": {
                "coding": [
                    {
                        "system": "http://hl7.org/fhir/dotnet-api-operation-outcome",
                        "code": "1028"
                    }
                ],
                "text": "Instance count for 'Patient.gender' is 0, which is not within the specified cardinality of 1..1"
            },
            "location": [
                "Patient"
            ]
        }
    ]
}
```

In this example, the resource didn't conform to the provided Patient profile, which required a patient identifier value and gender.

If you'd like to specify a profile as a parameter, you can specify the canonical URL for the profile to validate against, such as the following example for the HL7 base profile for `heartrate`:

`GET https://myworkspace-myfhirserver.fhir.azurehealthcareapis.com/Observation/12345678/$validate?profile=http://hl7.org/fhir/StructureDefinition/heartrate`

## Validating a new resource

If you'd like to validate a new resource that you are uploading to the server, you can do a `POST` request:

`POST http://<your FHIR service base URL>/{Resource}/$validate`

For example:

`POST https://myworkspace-myfhirserver.fhir.azurehealthcareapis.com/Patient/$validate`

This request will create the new resource you are specifying in the request payload and validate the uploaded resource. Then, it will return an `OperationOutcome` as a result of the validation on the new resource.

## Validate on resource CREATE or resource UPDATE

You can choose when you'd like to validate your resource, such as on resource `CREATE` or `UPDATE`. By default, the FHIR service is configured to opt out of validation on resource `Create/Update`. To validate on `Create/Update`, you can use the `x-ms-profile-validation` header set to true: `x-ms-profile-validation: true`.

> [!NOTE]
> In the open-source FHIR service, you can change the server configuration setting, under the CoreFeatures.

```json
{
   "FhirServer": {
      "CoreFeatures": {
            "ProfileValidationOnCreate": true,
            "ProfileValidationOnUpdate": false
        }
}
```

## Next steps

In this article, you learned how to validate resources against profiles using `$validate`. To learn about the other FHIR service supported features, see

>[!div class="nextstepaction"]
>[Supported FHIR features](fhir-features-supported.md)
