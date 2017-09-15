---
heading: WooCommerce
seo: Create Instance | WooCommerce | Cloud Elements API Docs
title: Authenticate
description: Authenticate
layout: sidebarelementdoc
breadcrumbs: /docs/elements.html
elementId: 2881
elementKey: woocommercerest
apiKey: Consumer Key
apiSecret: Consumer Secret
callbackURL: Callback URL Name
parent: Back to Element Guides
order: 20
---

# Authenticate with {{page.heading}}

You can authenticate with {{page.heading}} to create your own instance of the {{page.heading}} element through the UI or through APIs. Once authenticated, you can use the element instance to access the different functionality offered by the {{page.heading}} platform.

{% include callout.html content="<strong>On this page</strong></br><a href=#authenticate-through-the-ui>Authenticate Through the UI</a></br><a href=#authenticate-through-api>Authenticate Through API</a></br><a href=#parameters>Parameters</a></br><a href=#example-response-for-an-authenticated-element-instance>Example Response for an Authenticated Element Instance</a>" type="info" %}

## Authenticate Through the UI

Use the UI to authenticate with {{page.heading}} and create an element instance. You will need your **{{page.apiKey}}** and **{{page.apiSecret}}** that you identified in [API Provider Setup](#setup.html).

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
6. In **The Store URL** enter the url of your WooCommerce Storefront.
7. In **OAuth API Key** and **OAuth API Secret** enter your **{{page.apiKey}}** and **{{page.apiSecret}}**.
9. In Cloud Elements 2.0, optionally type or select one or more tags to add to the authenticated element instance.
7. Click **Create Instance** (Cloud Elements 2.0) or **Next** (earlier UI).
8. If using the earlier UI, optionally add tags to the authenticated element instance.
9. Note the **Token** and **ID** and save them for all future requests using the element instance.
![Authenticated Element Instance](../img/element-instance.png)
8. Take a look at the documentation for the element resources now available to you.

## Authenticate Through API

Authenticating through API is similar to authenticating via the UI. Instead of clicking and typing through a series of buttons, text boxes, and menus, you will instead send a request to our `instance` endpoint. The end result is the same, though: an authenticated element instance with a  **token** and **id**.

To authenticate an element instance:

1. Construct a JSON body as shown below (see [Parameters](#parameters)):


    ```json
    {
      "element": {
        "key": "{{page.elementKey}}"
      },
      "configuration": {
        "store.url": "<WEB_STORE_URL>",
      	"oauth.api.key": "<CONSUMER_KEY>",
      	"oauth.api.secret": "<CONSUMER_SECRET>"
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
  "configuration": {
    "store.url": "<http://mycoolstore.com>",
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
|  Name</br>`name` |  {{site.data.glossary.element-auth-name}}  | string  |
| OAuth API Key</br>`oauth.api.key` |  {{site.data.glossary.element-auth-api-key}} This is the **{{page.apiKey}}** that you noted in [API Provider Setup](setup.html). |  string |
| OAuth API Key</br>`oauth.api.secret` | {{site.data.glossary.element-auth-api-secret}} This is the **{{page.apiSecret}}** that you noted in [API Provider Setup](setup.html). | string |
| The Store URL<br>`oauth.callback.url` | The url of your WooCommerce Storefront  | string |
| tags | {{site.data.glossary.element-auth-tags}} | string |

## Example Response for an Authenticated Element Instance

In this example, the instance ID is `12345` and the instance token starts with "ABC/D...". The actual values returned to you will be unique: make sure you save them for future requests to this new instance.

```json
{
  "id": 12345,
  "name": "API Instance",
  "createdDate": "2017-08-07T18:46:38Z",
  "token": "ABC/Dxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "element": {
    "id": 2881,
    "name": "WooCommerce",
    "hookName": "WooCommercewp",
    "key": "woocommercerest",
    "description": "Add a Woocommerce (using WordPress REST) Instance to connect your existing Woocommerce account to the eCommerce Hub, allowing you to manage orders and products across multiple eCommerce Elements. You will need your Woocojmmerce API information to add an instance.",
    "image": "https://pbs.twimg.com/profile_images/378800000726053868/44a02004f2aeab0534c26fb5afc37784_400x400.png",
    "active": true,
    "deleted": false,
    "typeOauth": false,
    "trialAccount": false,
    "resources": [ ],
    "transformationsEnabled": true,
    "bulkDownloadEnabled": true,
    "bulkUploadEnabled": true,
    "cloneable": true,
    "extendable": false,
    "beta": false,
    "authentication": {
        "type": "custom"
    },
    "extended": false,
    "hub": "ecommerce",
    "protocolType": "http",
    "parameters": [  ],
    "private": false
    },
    "elementId": 2881,
    "tags": [
        "Docs"
    ],
    "provisionInteractions": [],
    "valid": true,
    "disabled": false,
    "maxCacheSize": 0,
    "cacheTimeToLive": 0,
    "configuration": {    },
    "eventsEnabled": false,
    "cachingEnabled": false,
    "externalAuthentication": "none",
    "traceLoggingEnabled": false,
    "user": {
        "id": 12345
      }
}
```