# BigPanda/Webhook Integration


You can configure Cribl Stream to send Webhook notifications to the [BigPanda](https://www.bigpanda.io/our-product/) IT Ops platform. These notifications arrive in BigPanda as [Alerts](https://docs.bigpanda.io/docs/alert), which BigPanda correlates into [Incidents](https://docs.bigpanda.io/docs/incident).

Before you begin, you should have an Admin account on a BigPanda Cloud instance.

## Prepare BigPanda to Receive Data from Cribl Stream

> The BigPanda **App Key** and **Access Token** are separate and independent. The **Access Token** is a 32-character string that is part of the value that BigPanda generates for the `Authorization` HTTP header. (It functions like an auth token or bearer token.)
>
{.box .info}

1. Log into your BigPanda Cloud instance as an Admin.

2. In the **Integrations** tab, click **New Integration**.

![The **Integrations** tab](st-webhook-bigpanda01.115dfd8226.png)

3. In the **Create a New Integration** modal, select the Cribl tile.

![The **Create a New Integration** modal](st-webhook-bigpanda02.53f26e6165.png)

- This opens the **Cribl Integration** page.

![The **Cribl Integration** page](st-webhook-bigpanda03.2c50d94227.png)

4. In the **Create an App Key** section, generate an App Key named `Cribl Stream`. You'll need the App Key when configuring Cribl Stream in the next section. Cribl Stream will insert the App Key into every event it sends to BigPanda. 

5. BigPanda will generate a page containing setup instructions for the Cribl Stream Webhook Destination. Store the following information:

    - **URL**: `https://integrations.bigpanda.io/oim/cribl/alerts`.
    - **Method**: `POST`.
    - **Format**: `Custom`.
    - **Content Type**: `application/json`.
    - **Authentication token**: A 32-character `<auth-token>`.

  The BigPanda page will also contain test code. You'll need the preceding information and the test code when configuring Cribl Stream in the next section.

## Configure the Webhook Destination in Cribl Stream

1. From a Cribl Stream instance's or Group's **Manage** submenu, select **Data** > **Destinations**. Then select **Webhook** from the **Manage Destinations** page's tiles or left nav. Click **New Destination** to open the **Webhook** > **New Destination** modal.

2. On the modal's **Configure** > **General Settings** tab, enter or select the following values:

    - **URL**:   Enter the Alerts API endpoint URL, for example: `https://integrations.bigpanda.io/oim/cribl/alerts`.
    - **Method**: `POST`.
    - **Format**: `Custom`.
    - **Content type**: `application/json`.

3. In the **Authentication** tab, select an **Authentication type**. 

    - You can select **Auth token**, and then enter the Access Token (the 32-character string you wrote down [earlier](#access-token)) in the **Token** field. 

    - Alternatively, select **Auth token (text secret)** to expose the **Secret** drop-down, in which you can select a [stored secret](securing-and-monitoring#secrets) that references the Access Token. A **Create** link is available to store a new, reusable secret.

4. Click **Save**, then **Commit & Deploy**. You are now ready to test your Webhook Destination's communication with Big Panda.

5. In the **Test** tab, enter the test code from the BigPanda setup instructions. It will look like this:

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

#### BigPanda Alert Deduplication

BigPanda processes an alert's Primary and Secondary properties on ingestion. These properties default to `host` and `check`. 

When an alert has the same Primary and Secondary properties as those received in previous alerts, the Incident Console shows only the timestamp from the initial event.

For more information on this functionality, see the BigPanda pages [Deduplication](https://docs.bigpanda.io/docs/deduplication) and [Incident_identifier](https://docs.bigpanda.io/docs/incident_identifier).

## Verify that BigPanda is Receiving Notifications and Events

In the BigPanda **Incidents** tab, you should see an Incident whose Source is `Cribl Stream`. The details of the test input you sent from the Webhook Destination should appear in an Alert within that Incident. If so: It works!
