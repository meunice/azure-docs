---
title: include file
description: include file
services: azure-communication-services
author: tomaschladek
manager: nmurav
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 11/17/2021
ms.topic: include
ms.custom: include file
ms.author: tchladek
---

## Prerequisites

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/azure/developer/java/fundamentals/java-jdk-install) version 8 or later.
- [Apache Maven](https://maven.apache.org/download.cgi).
- An active Communication Services resource and connection string. [Create a Communication Services resource](../../create-communication-resource.md).

## Final code
Find the finalized code for this quickstart on [GitHub](https://github.com/Azure-Samples/communication-services-java-quickstarts/tree/main/access-token-quickstart).

## Set up your environment

### Create a new Java application

In a terminal or Command Prompt window, go to the directory where you want to create your Java application. To generate a Java project from the maven-archetype-quickstart template, run the following code:

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

You'll notice that the `generate` task creates a directory with the same name as `artifactId`. Under this directory, the *src/main/java* directory contains the project source code, the *src/test/java* directory contains the test source, and the *pom.xml* file is the project's Project Object Model, or POM. This file is used for project configuration parameters.

### Install the Communication Services packages

Open the *pom.xml* file in your text editor. Add the following dependency element to the group of dependencies:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-identity</artifactId>
    <version>1.0.0</version>
</dependency>
```

This code instructs Maven to install the Communication Services Identity SDK, which you'll use later.

### Set up the app framework

In the project directory, do the following:

1. Go to the */src/main/java/com/communication/quickstart* directory.
1. Open the *App.java* file in your editor.
1. Replace the `System.out.println("Hello world!");` statement.
1. Add `import` directives.

Use the following code to begin:

```java
package com.communication.quickstart;

import com.azure.communication.common.*;
import com.azure.communication.identity.*;
import com.azure.communication.identity.models.*;
import com.azure.core.credential.*;

import java.io.IOException;
import java.time.*;
import java.util.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Access Tokens Quickstart");
        // Quickstart code goes here
    }
}
```

## Authenticate the client

Instantiate a `CommunicationIdentityClient` with your resource's access key and endpoint. For more information, see the "Store your connection string" section of [Create and manage Communication Services resources](../../create-communication-resource.md#store-your-connection-string). 

In addition, you can initialize the client with any custom HTTP client that implements the `com.azure.core.http.HttpClient` interface.

In the *App.java* file, add the following code to the `main` method:

```java
// You can find your endpoint and access key from your resource in the Azure portal
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";
String accessKey = "SECRET";

CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
        .endpoint(endpoint)
        .credential(new AzureKeyCredential(accessKey))
        .buildClient();
```

Instead of providing the endpoint and access key, you can provide the entire connection string by using the `connectionString()` method.

```java
// You can find your connection string from your Communication Services resource in the Azure portal
String connectionString = "<connection_string>";

CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
    .connectionString(connectionString)
    .buildClient();
```

If you've already set up an Azure Active Directory (Azure AD) application, you can [authenticate by using Azure AD](../../identity/service-principal.md).

```java
String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";
TokenCredential credential = new DefaultAzureCredentialBuilder().build();

CommunicationIdentityClient communicationIdentityClient = new CommunicationIdentityClientBuilder()
        .endpoint(endpoint)
        .credential(credential)
        .buildClient();
```

## Create an identity

To create access tokens, you need an identity. Azure Communication Services maintains a lightweight identity directory for this purpose. Use the `createUser` method to create a new entry in the directory with a unique `Id`.

```java
CommunicationUserIdentifier user = communicationIdentityClient.createUser();
System.out.println("\nCreated an identity with ID: " + user.getId());
```

The created identity is required later for issuing access tokens. Store the received identity with mapping to your application's users (for example, by storing it in your application server database). 

## Issue access tokens

Use the `getToken` method to issue an access token for your Communication Services identity. The `scopes` parameter defines a set of access token permissions and roles. For more information, see the list of supported actions in [Authenticate to Azure Communication Services](../../../concepts/authentication.md). 

In the following code, use the user variable that you created in the preceding step to get a token.

```java
// Issue an access token with the "voip" scope for a user identity
List<CommunicationTokenScope> scopes = new ArrayList<>(Arrays.asList(CommunicationTokenScope.VOIP));
AccessToken accessToken = communicationIdentityClient.getToken(user, scopes);
OffsetDateTime expiresAt = accessToken.getExpiresAt();
String token = accessToken.getToken();
System.out.println("\nIssued an access token with 'voip' scope that expires at: " + expiresAt + ": " + token);
```

## Create an identity and issue a token in one request

Alternatively, you can use the 'createUserAndToken' method to create a new entry in the directory with a unique `Id` and
issue an access token at the same time.

```java
List<CommunicationTokenScope> scopes = Arrays.asList(CommunicationTokenScope.CHAT);
CommunicationUserIdentifierAndToken result = communicationIdentityClient.createUserAndToken(scopes);
CommunicationUserIdentifier user = result.getUser();
System.out.println("\nCreated a user identity with ID: " + user.getId());
AccessToken accessToken = result.getUserToken();
OffsetDateTime expiresAt = accessToken.getExpiresAt();
String token = accessToken.getToken();
System.out.println("\nIssued an access token with 'chat' scope that expires at: " + expiresAt + ": " + token);
```

Access tokens are short-lived credentials that need to be reissued. Not doing so might cause a disruption of your application users' experience. The `expiresAt` property indicates the lifetime of the access token.

## Refresh access tokens

To refresh an access token, use the `CommunicationUserIdentifier` object to reissue it:

```java
// existingIdentity represents the Communication Services identity that's stored during identity creation
CommunicationUserIdentifier identity = new CommunicationUserIdentifier(existingIdentity.getId());
AccessToken response = communicationIdentityClient.getToken(identity, scopes);
```

## Revoke an access token

You might occasionally need to explicitly revoke an access token. For example, you would do so when application users change the password they use to authenticate to your service. The `revokeTokens` method invalidates all active access tokens for a particular user. In the following code, you can use the previously created user.

```java
communicationIdentityClient.revokeTokens(user);
System.out.println("\nSuccessfully revoked all access tokens for user identity with ID: " + user.getId());
```

## Delete an identity

When you delete an identity, you revoke all active access tokens and prevent the further issuance of access tokens for the identity. Doing so also removes all persisted content that's associated with the identity.

```java
communicationIdentityClient.deleteUser(user);
System.out.println("\nDeleted the user identity with ID: " + user.getId());
```

## Run the code

Go to the directory that contains the *pom.xml* file, and then compile the project by using the following `mvn` command:

```console
mvn compile
```

Then, build the package:

```console
mvn package
```

Run the following `mvn` command to execute the app:

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```
