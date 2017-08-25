---
heading: Name of Element
seo: Authenticate | Name of Element | Cloud Elements API Docs
title: Authenticate
description: Authenticate an element instance with the service provider
layout: sidebarelementdoc
breadcrumbs: /docs/elements.html
elementId: nn
elementKey: fake
parent: Back to Element Guides
order: 20
---

# Authenticate with {{page.heading}}

You can authenticate with {{page.heading}} to create your own instance of the {{page.heading}} element through the UI or through APIs. Once authenticated, you can use the element instance to access the different functionality offered by the {{page.heading}} platform.

{% include callout.html content="<strong>On this page</strong></br><a href=#authenticate-through-the-ui>Authenticate Through the UI</a></br><a href=#authenticate-through-api>Authenticate Through API</a></br><a href=#parameters>Parameters</a></br><a href=#example-response-for-an-authenticated-element-instance>Example Response for an Authenticated Element Instance</a>" type="info" %}

## Authenticate Through the UI

Use the UI to authenticate with {{page.heading}} and create an element instance. {{page.heading}} authentication follows the typical OAuth 2.0 framework and you will need to sign in to {{page.heading}} as part of the process.

<Use this paragraph to identify the type of authentication. The sample is for OAuth2, but there are obviously others.>

If you are configuring events, see the [Events section](events.html).

To authenticate an element instance:

1. Sign in to Cloud Elements, and then search for {{page.heading}} in our Elements Catalog.

    | Cloud Elements 2.0 | Earlier UI  |
    | :------------- | :------------- |
    |  ![Search](../img/Element-Search2.png)  |  ![Search](../img/Element-Search.png)  |

3. Authenticate an element instance.

    | Cloud Elements 2.0 | Earlier UI  |
    | :------------- | :------------- |
    | Hover over the element card, and then click **Authenticate**.</br> ![Create Instance](../img/Create-Instance.gif)  | Click **Add Instance**.</br> ![Search](../img/Add-Instance.png)  |

5. Enter a name for the element instance.
9. In Cloud Elements 2.0, optionally type or select one or more tags to add to the authenticated element instance.
7. Click **Create Instance** (Cloud Elements 2.0) or **Next** (earlier UI).
8. Provide your {{page.heading}} credentials, and then allow the connection.

    After you authenticate with the API provider, the authentication flow returns you to Cloud Elements.

8. If using the earlier UI, optionally add tags to the authenticated element instance.
9. Note the **Token** and **ID** and save them for all future requests using the element instance.
![Authenticated Element Instance](../img/element-instance.png)
8. Take a look at the documentation for the element resources now available to you.

## Authenticate Through API

Authenticating through API is similar to authenticating via the UI. Instead of clicking and typing through a series of buttons, text boxes, and menus, you will instead send a request to our `instance` endpoint. The end result is the same, though: an authenticated element instance with a  **token** and **id**.

<The text below is for an OAuth2 element. If this is a basic authentication element, delete everything up to Authenticating the Element Instance (including that heading) and start this section with the sentence "Use the /instances endpoint...">

Authenticating through API follows a multi-step OAuth 2.0 process that involves:

{% include workflow.html displayNames="Redirect URL,Authenticate Users,Authenticate Instance" links="#getting-a-redirect-url,#authenticating-users-and-receiving-the-authorization-grant-code,#authenticating-the-element-instance" active=" "%}

* [Getting a redirect URL](#getting-a-redirect-url). This URL sends users to the vendor to log in to their account.
* [Authenticating users and receiving the authorization grant code](#authenticating-users-and-receiving-the-authorization-grant-code). After the user logs in, the vendor makes a callback to the specified url with an authorization grant code.
* [Authenticating the element instance](#authenticating-the-element-instance). Using the authorization code from the vendor, authenticate with the vendor to create an element instance at Cloud Elements.

### Getting a Redirect URL

{% include workflow.html displayNames="Redirect URL,Authenticate Users,Authenticate Instance" links="#getting-a-redirect-url,#authenticating-users-and-receiving-the-authorization-grant-code,#authenticating-the-element-instance" active="Redirect URL"%}

Use the following API call to request a redirect URL where the user can authenticate with the service provider. Replace `{keyOrId}` with the element key, `{{page.elementKey}}`.

```bash
curl -X GET /elements/{keyOrId}/oauth/url?apiKey=<api_key>&apiSecret=<api_secret>&callbackUrl=<url>&siteAddress=<url>
```

#### Query Parameters

| Query Parameter | Description   |
| :------------- | :------------- |
| apiKey | The key obtained from registering your app with the provider. This is the **Client ID** that you noted at the end of the [API Provider Setup section](setup.html).  |
| apiSecret |  The secret obtained from registering your app with the provider.  This is the **Client Secret** that you noted at the end of the [API Provider Setup section](setup.html).   |
| callbackUrl | The URL that will receive the code from the vendor to be used to create an element instance.   |

#### Example cURL

```bash
curl -X GET \
  'https://api.cloud-elements.com/elements/api-v2/elements/{{page.elementKey}}/oauth/url?apiKey=fake_api_key&apiSecret=fake_api_secret&callbackUrl=https://www.mycoolapp.com/auth&state={{page.elementKey}}' \
```

#### Example Response

Use the `oauthUrl` in the response to allow users to authenticate with the vendor.

<Replace the below oauthUrl value with an actual one from Postman.>

```json
{
"element": "{{page.elementKey}}",
"oauthUrl": "https://apis.hootsuite.com/auth/oauth/v2/authorize?scope=oob&response_type=code&redirect_uri=https%3A%2F%2Fhttpbin.org%2Fget&state=hootsuite&client_id=l7xx1cf795a3144b42ac96cbb3f301af6b7b"
}
```

### Authenticating Users and Receiving the Authorization Grant Code

{% include workflow.html displayNames="Redirect URL,Authenticate Users,Authenticate Instance" links="#getting-a-redirect-url,#authenticating-users-and-receiving-the-authorization-grant-code,#authenticating-the-element-instance" active="Authenticate Users"%}

Provide the response from the previous step to the users. After they authenticate, {{page.heading}} provides the following information in the response:

* code
* state

| Response Parameter | Description   |
| :------------- | :------------- |
| code | The Authorization Grant Code required by Cloud Elements to retrieve the OAuth access and refresh tokens from the endpoint.|
| state | A customizable identifier, typically the element key (`{{page.elementKey}}`) . |

{% include note.html content="If the user denies authentication and/or authorization, there will be a query string parameter called <code>error</code> instead of the <code>code</code> parameter. In this case, your application can handle the error gracefully." %}

### Authenticating the Element Instance

{% include workflow.html displayNames="Redirect URL,Authenticate Users,Authenticate Instance" links="#getting-a-redirect-url,#authenticating-users-and-receiving-the-authorization-grant-code,#authenticating-the-element-instance" active="Authenticate Instance"%}

Use the `/instances` endpoint to authenticate with {{page.heading}} and create an element instance. If you are configuring events, see the [Events section](events.html).

{% include note.html content="The endpoint returns an element instance token and id upon successful completion. Retain the token and id for all subsequent requests involving this element instance.  " %}

To authenticate an element instance:

1. Construct a JSON body as shown below (see [Parameters](#parameters)):


    ```json
    {
      "element": {
        "key": "{{page.elementKey}}"
      },
      "providerData": {
        "code": "<AUTHORIZATION_GRANT_CODE>"
      },
      "configuration": {
        "oauth.callback.url": "<CALLBACK_URL>",
        "oauth.api.key": "<CONSUMER_KEY>",
      	"oauth.api.secret": "<CONSUMER_SECRET>",
        "filter.response.nulls": true
      },
      "tags": [
        "<Add_Your_Tag>"
      ],
      "name": "<INSTANCE_NAME>"
    }
    ```

1. Call the following, including the JSON body you constructed in the previous step:

        POST /instances

    {% include note.html content="Make sure that you include the User and Organization keys in the header. See <a href=index.html#authenticating-with-cloud-elements>the Overview</a> for details. " %}

1. Locate the `token` and `id` in the response and save them for all future requests using the element instance.

#### Example cURL

```bash
curl -X POST \
  https://api.cloud-elements.com/elements/api-v2/instances \
  -H 'authorization: User <USER_SECRET>, Organization <ORGANIZATION_SECRET>' \
  -H 'content-type: application/json' \
  -d '{
  "element": {
    "key": "{{page.elementKey}}"
  },
  "providerData": {
    "code": "xxxxxxxxxxxxxxxxxxxxxxx"
  },
  "configuration": {
    "oauth.callback.url": "https;//mycoolapp.com",
    "oauth.api.key": "xxxxxxxxxxxxxxxxxx",
    "oauth.api.secret": "xxxxxxxxxxxxxxxxxxxxxxxx"
  },
  "tags": [
    "Docs"
  ],
  "name": "API Instance"
}'
```
## Parameters

API parameters not shown in {{site.console}} are in `code formatting`.

{% include note.html content="Event related parameters are described in <a href=events.html>Events</a>." %}

| Parameter | Description   | Data Type |
| :------------- | :------------- | :------------- |
| `key` | The element key.<br>{{page.elementKey}}  | string  |
| `code` | The authorization grant code returned from the API provider in an OAuth2 authentication workflow. | string |
|  Name</br>`name` |  The name for the element instance created during authentication.   | string  |
| `oauth.callback.url` | The URL where you want to redirect users after they grant access. This is the **Callback URL** that you noted in the [API Provider Setup section](setup.html).  | string  |
| `oauth.api.key` | The Client ID from {{page.heading}}. This is the **Client ID** that you noted in the [API Provider Setup section](setup.html). |  string |
| `oauth.api.secret` | The Client Secret from {{page.heading}}. This is the **Client Secret** that you noted in the [API Provider Setup section](setup.html). | string |
| tags | *Optional*. User-defined tags to further identify the instance. | string |

## Example Response for an Authenticated Element Instance

In this example, the instance ID is `12345` and the instance token starts with "ABC/D...". The actual values returned to you will be unique: make sure you save them for future requests to this new instance.

```json
{
  "id": 12345,
  "name": "API Instance",
  "createdDate": "2017-08-07T18:46:38Z",
  "token": "ABC/Dxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "element": {
      "id": 1323,
      "name": "ServiceNow OAuth",
      "hookName": "ServiceNow",
      "key": "servicenowoauth",
      "description": "ServiceNow is changing the way people work, offering service management for every department in the enterprise including IT, human resources, facilities & more.",
      "image": "https://pbs.twimg.com/profile_images/378800000041139697/cf1e6299ecb533ed82725abe96bb96a9_400x400.png",
      "active": true,
      "deleted": false,
      "typeOauth": false,
      "trialAccount": false,
      "resources": [ ],
      "transformationsEnabled": true,
      "bulkDownloadEnabled": true,
      "bulkUploadEnabled": true,
      "cloneable": true,
      "extendable": true,
      "beta": true,
      "authentication": {
          "type": "oauth2"
      },
      "extended": false,
      "hub": "helpdesk",
      "protocolType": "http",
      "parameters": [  ]
    },
    "elementId": {{page.elementId}},
    "tags": [
      "Docs"
      ],
    "provisionInteractions": [  ],
    "valid": true,
    "disabled": false,
    "maxCacheSize": 0,
    "cacheTimeToLive": 0,
    "configuration": {    },
    "eventsEnabled": false,
    "traceLoggingEnabled": false,
    "cachingEnabled": false,
    "externalAuthentication": "none",
    "user": {
        "id": 12345
      }
}
```