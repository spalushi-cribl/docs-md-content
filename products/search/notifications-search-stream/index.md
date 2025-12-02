# Notifications via Cribl Stream


Send Notifications about your [scheduled searches](scheduled-searches) through [Cribl Stream](/stream/) to multiple downstream services.

---

This topic shows how to send Notifications to Cribl Stream [Worker Groups](/stream/worker-groups), which can forward them to multiple downstream services, using workflows you've configured to also handle Notifications from Cribl Stream itself (and other Cribl products).

This technique is a convenient alternative to configuring multiple [Webhook Notification](/search/webhook-notification-targets) targets in Cribl Search. Here, you configure a single Webhook target for the Worker Group, and let Cribl Stream multiplex the outbound Notifications from there.

We outline this technique in the following general steps:

- [Identify Ingest Address](#address) for your Worker Group

- [Add Webhook Notification Target](#target) in Cribl Search

- [Schedule and Save Search and Notification](#notification)

- [Verify Inflow to Cribl Stream](#inflow)

- [Configure Outflow from Cribl Stream](#outflow)

## Identify Ingest Address {#address}

To find the ingest address of a Cribl-managed Worker Group, start in the Cribl.Cloud Organization that hosts your Cribl Search instance:

1. From the top bar, select **Products** > **Cribl**.

1. From the resulting sidebar, select **Data Sources**.

1. Use the **Group** drop-down to select the Cribl-managed Worker Group where you want to send the alerts.

1. Under **Sources Enabled by Default**, find the `http` entry. 

1. Here, copy the **Ingest Address** (URL) of your Worker Group. This will point to port `10080`. The general format is:  
`https://<groupName>.<workspaceName>.<organizationId>.cribl.cloud:10080`

> An example with the most typical Group and Workspace names, and with a fictitious Organization name:
`https://default.main.goat-farm.cribl.cloud:10080`

> For a [hybrid Group](/stream/cloud-enterprise#hybrid) running on a host that you manage, the ingest address will typically be configured through your load balancer.
{.box .info}

## Add Webhook Notification Target {#target}

1. Navigate back to Cribl Search: Select **Products** > **Search**.

1. Select **Settings** > **Search** > **Notification Targets**.

1. Select the **Webhook** type.

1. Select **Add Target**.

1. Paste the **Ingest Address** URL that you copied from your Cribl.Cloud Organization, appending `/cribl/_bulk` as in this example:   
`https://default.main.<organizationId>.cribl.cloud:10080/cribl/_bulk`

4. Save the new target as `stream_webhook` or your desired name.

![Configuring the Webhook Notification target](search-stream-notify-1.png)
{scale="100%"}

## Schedule and Save Search and Notification {#notification}

1. Create a search, customized according to your needs. The example shown here uses this query:  
`dataset="$vt_dummy" | extend alert="true"`

2. Select **Actions** > **Save** to open the configuration modal shown below. 

![Saving the search configuration](search-stream-notify-2.png)
{scale="95%"}

3. [Schedule](/search/scheduled-searches) the search, according to your needs. This example sets a `* * * * *` cron schedule, to repeat the search every minute.

![Scheduling the search](search-stream-notify-3.png)
{scale="60%"}

4. Add the Notification, pointing to the Webhook target you configured [above](#target).

![Specifying the Notification](search-stream-notify-4.png)
{scale="70%"}

5. **Save** the search, with its configured schedule and Notification.

## Verify Inflow to Cribl Stream {#inflow}

Next, make sure that Cribl Search is forwarding your Notifications to Cribl Stream:

1. In Cribl Stream, navigate to your targeted Worker Group.

1. Open the [HTTP Source](/stream/sources-https) config modal: Select **Sources** > **HTTP**.

1. Select the **Live Data** tab, then its **Capture** button. 

1. Set a long time window (several minutes), then select **Start**. 

1. Wait. In about a minute, a properly configured alert will appear.

![Previewing the Notification's arrival in Cribl Stream](search-stream-notify-5-preview.png)
{scale="100%"}

## Configure Outflow from Cribl Stream {#outflow}

1. In Cribl Stream, configure or reconfigure a [Destination](/stream/destinations), [Pipeline](/stream/pipelines), and [Route](/stream/routes) to relay the alerts to your chosen downstream service(s).

1. Verify connectivity all the way through to these receivers.

1. That's it!
