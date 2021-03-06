---
heading: MailChimp v3.0
apiProvider: MailChimp # For cases where the API Provider is different than the element name. e;g;, ServiceNow vs. ServiceNow Oauth
seo: Endpoint Setup | MailChimp | Cloud Elements API Docs
title: Endpoint Setup
description: Endpoint setup
layout: sidebarelementdoc
breadcrumbs: /docs/elements.html
elementId: 527
elementKey: mailchimpv3
apiKey: Client Id #In OAuth2 this is what the provider calls the apiKey, like Client ID, Consumer Key, API Key, or just Key
apiSecret: Client Secret #In OAuth2 this is what the provider calls the apiSecret, like Client Secret, Consumer Secret, API Secret, or just Secret
callbackURL: Redirect URI #In OAuth2 this is what the provider calls the callbackURL, like Redirect URL, App URL, or just Callback URLparent: Back to Element Guides
parent: Back to Element Guides
order: 5
---

# API Provider Setup

To authenticate a {{page.heading}} element instance you must register an app with {{page.apiProvider}}. When you authenticate, use the **{{page.apiKey}}**, **{{page.apiSecret}}**, and **{{page.callbackURL}}** as the **API Key**, **API Secret**, and **Callback URL**.  If you plan to monitor events, also set up webhooks by following the steps in [Set Up Events](#set-up-events).

See the latest setup instructions in the [{{page.apiProvider}} documentation](https://developer.mailchimp.com/documentation/mailchimp/reference/authorized-apps/).

{% include callout.html content="<strong>On this page</strong></br><a href=#locate-credentials-for-authentication>Locate Credentials for Authentication</a></br><a href=#create-an-application>Create an Application</a></br><a href=#set-up-events>Set Up Events</a>" type="info" %}

## Locate Credentials for Authentication

If you already created an application, follow the steps below to locate the **{{page.apiKey}}**, **{{page.apiSecret}}**, and **{{page.callbackURL}}**. If you have not created an app, see [Create an Application](#create-an-application).

To find your OAuth 2.0 credentials:

1. Log in to your account at [{{page.apiProvider}}](https://mailchimp.com/).
2. In the upper right corner, click your name, and then select **Account**.
![Account](img/account.png)
3. Click **Extras**, and then select **Registered apps**.
![Registered Apps](img/registerd.png)
4. Locate your app, and then click **Edit**.
2. Scroll down and record the the **{{page.callbackURL}}**, **{{page.apiKey}}**, and **{{page.apiSecret}}**.
![Key secret and URL](img/mailchimp-creds.png)

## Create an Application

If you have not created an application, you need one to authenticate with {{page.apiProvider}}.

To create an application:

1. Log in to your account at [{{page.apiProvider}}](https://mailchimp.com/).
2. In the upper right corner, click your name, and then select **Account**.
![Account](img/account.png)
3. Click **Extras**, and then select **Registered apps**.
![Registered Apps](img/registerd.png)
2. Click **Register an App**.
3. Complete the required information.
4. In **{{page.callbackURL}}** enter the URL that will receive the authorization grant code and return users upon authentication during the OAuth Authorization process.
4. Click **Create**.
2. Scroll down and record the the **{{page.callbackURL}}**, **{{page.apiKey}}**, and **{{page.apiSecret}}**.
![Key secret and URL](img/mailchimp-creds.png)

## Set Up Events

{{page.apiProvider}} supports webhooks. If you want to enable events when you authenticate an element instance, complete the steps below to set up webhooks with {{page.apiProvider}}. You can set up webhooks after you authenticate with a `POST /lists/{id}/webhook` request.

To set up webhooks:

1. Log in to your account at [{{page.apiProvider}}](https://mailchimp.com/).
2. Navigate to your **Lists**
3. Choose the list to add a webhook to, and then navigate to the Settings.
![Settings](img/settings.png)
4. Scroll down and click **Webhooks**.
5. Click **Create New Webhook**.
3. In **Callback URL** enter the URL to receive webhook notifications, such as `https://api.cloud-elements.com/elements/api-v2/events/{{page.elementKey}}`.
6. Set up your webhook, and then click **Save**.
6. Record the **Callback URL** which you will use when you set up events as the **Event Notification Callback URL**.

Next [authenticate an element instance with {{page.apiProvider}}](authenticate.html).
