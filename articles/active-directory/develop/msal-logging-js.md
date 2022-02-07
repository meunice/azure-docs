---
title: Logging errors and exceptions in MSAL.js
titleSuffix: Microsoft identity platform
description: Learn how to log errors and exceptions in MSAL.js
services: active-directory
author: mmacy
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/21/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
---
# Logging in MSAL.js

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## Configure logging in MSAL.js

Enable logging in MSAL.js (JavaScript) by passing a loggerOptions object during the configuration for creating a `PublicClientApplication` instance. The only required config parameter is the client ID of the application. Everything else is optional, but may be required depending on your tenant and application model.

The loggerOptions object has the following properties:

- `loggerCallback`: a Callback function that can be provided by the developer to handle the logging of MSAL statements in a custom manner. Implement the `loggerCallback` function depending on how you want to redirect logs. The loggerCallback function has the following format ` (level: LogLevel, message: string, containsPii: boolean): void`
     - The supported log levels are: `Error`, `Warning`, `Info`, and `Verbose`. The default is `Info`.
- `piiLoggingEnabled` (optional): if set to true, logs personal and organizational data. By default this is false so that your application doesn't log personal data. Personal data logs are never written to default outputs like Console, Logcat, or NSLog.

```javascript
const msalConfig = {
    auth: {
        clientId: "enter_client_id_here",
        authority: "https://login.microsoftonline.com/common",
        knownAuthorities: [],
        cloudDiscoveryMetadata: "",
        redirectUri: "enter_redirect_uri_here",
        postLogoutRedirectUri: "enter_postlogout_uri_here",
        navigateToLoginRequestUrl: true,
        clientCapabilities: ["CP1"]
    },
    cache: {
        cacheLocation: "sessionStorage",
        storeAuthStateInCookie: false,
        secureCookies: false
    },
    system: {
        loggerOptions: {
            loggerCallback: (level: LogLevel, message: string, containsPii: boolean): void => {
                if (containsPii) {
                    return;
                }
                switch (level) {
                    case LogLevel.Error:
                        console.error(message);
                        return;
                    case LogLevel.Info:
                        console.info(message);
                        return;
                    case LogLevel.Verbose:
                        console.debug(message);
                        return;
                    case LogLevel.Warning:
                        console.warn(message);
                        return;
                }
            },
            piiLoggingEnabled: false
        },
        windowHashTimeout: 60000,
        iframeHashTimeout: 6000,
        loadFrameTimeout: 0,
        asyncPopups: false
    };
}

const msalInstance = new PublicClientApplication(msalConfig);
```

## Next steps

For more code samples, refer to [Microsoft identity platform code samples](sample-v2-code.md).