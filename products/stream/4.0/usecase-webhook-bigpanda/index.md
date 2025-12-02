# BigPanda/Webhook Integration


You can configure Cribl Stream to send Webhook notifications to the [BigPanda](https://www.bigpanda.io/our-product/) IT Ops platform. These notifications arrive in BigPanda as [Alerts](https://docs.bigpanda.io/reference#alerts), which BigPanda correlates into [Incidents](https://docs.bigpanda.io/reference#incidents-how-it-works).

Before you begin, you should have an Admin account on a BigPanda Cloud instance.

## Prepare BigPanda to Receive Data from Cribl Stream

> The BigPanda **App Key** and **Access Token** are separate and independent. The **Access Token** is a 32-character string that is part of the value that BigPanda generates for the `Authorization` HTTP header. (It functions like an auth token or bearer token.)
>
{.box .info}

1. Log into your BigPanda Cloud instance as an Admin.

2. In the **Integrations** tab, click **New Integration**.

![The **Integrations** tab](st-webhook-bigpanda01.8147d2a4a8.png)

3. In the **Create a New Integration** modal, select **Alerts REST API** and click **Integrate**.

![The **Create a New Integration** modal](st-webhook-bigpanda02.0e173c9497.png)

- This opens the **Alerts API Integration** page.

![The **Alerts API Integration** page](st-webhook-bigpanda03.31566a8fca.png)

4. In the **Create an App Key** section, generate an App Key named `Cribl Stream`. You'll need the App Key when configuring Cribl Stream in the next section. Cribl Stream will insert the App Key into every event it sends to BigPanda. 

5. Store the following information from the **Make a REST Call From Your Monitoring System** section:

    - The Alerts API endpoint URL. 

    - {{< id `access-token` >}} The `Authorization` HTTP header. This should consist of the word `Bearer`, a space, and a 32-character string.   The 32-character string will be your **Access Token**. 

    - The `Content-Type` HTTP header. This should be `application/json`.

      You'll need the URL and header values when configuring Cribl Stream in the next section. 

6. View your completed `Cribl Stream` integration in the **Integrations** tab.
 

## Configure the Webhook Destination in Cribl Stream

1. From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Destinations**. Then select **Webhook** from the **Manage Destinations** page's tiles or left nav. Click **New Destination** to open the **Webhook** > **New Destination** modal.

2. On the modal's **Configure** > **General Settings** tab, enter or select the following values:

    - **URL**:   Enter the Alerts API endpoint URL, for example: `https://api.bigpanda.io/data/v2/alerts`
    - **Method**: `POST`
    - **Format**: `Custom`
    - **Content type**: `application/json`

3. In the **Authentication** tab, select an **Authentication type**. 

    - You can select **Auth token**, and then enter the Access Token (the 32-character string you wrote down [earlier](#access-token)) in the **Token** field. 

    - Alternatively, select **Auth token (text secret)** to expose the **Secret** drop-down, in which you can select a [stored secret](securing-and-monitoring#secrets) that references the Access Token. A **Create** link is available to store a new, reusable secret.

4. Click **Save**, then **Commit & Deploy**. You are now ready to test your Webhook Destination's communication with Big Panda.

5. In the **Test** tab, enter the following test input, substituting your own App Key value for the `<your_app_key>` placeholder shown:

```
  [
	{
		"app_key": "<your_app_key>",
		"status": "critical",
		"host": "production-database-1",
		"timestamp": 1402302570,
		"check": "CPU overloaded",
		"description": "CPU is above upper limit (70%)",
		"cluster": "production-databases",
		"my_unique_attribute": "myUniqueValue987654321"
	}
  ]
```
5. Click **Run Test**.

This should send an alert to BigPanda.

#### BigPanda Alerts API Requirements

HTTP payloads sent to the BigPanda Alerts API must satisfy rules that are beyond the scope of this topic. For details, see the BigPanda documentation about [Alert Properties](https://answers.bigpanda.io/en/articles/4558805-alert-properties-primary_property-and-secondary_property) and [Integration Diagnostics](https://docs.bigpanda.io/docs/troubleshooting-integrations).

However, at at minimum, three fields are required:

1. `app_key`.
2. `status`.
3. `host` OR `service` OR `application` OR `device`.

Thus, the test input shown above works even if you omit all but the first three fields.

There are other possibilities for the third field, but they require understanding how BigPanda determines the `primary_property` of an Alert, plus some additional BigPanda configuration. See the BigPanda links above for details.

## Verify that BigPanda is Receiving Notifications and Events

In the BigPanda **Incidents** tab, you should see an Incident whose Source is `Cribl Stream`. The details of the test input you sent from the Webhook Destination should appear in an Alert within that Incident. If so: It works!
