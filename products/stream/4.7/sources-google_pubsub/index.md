# Google Cloud Pub/Sub Source


Cribl Stream supports receiving data records from [Google Cloud Pub/Sub](https://cloud.google.com/pubsub/docs/admin), a managed real-time messaging service for sending and receiving messages between applications.

> Type: **Pull**  |  TLS Support: **YES** (secure API) | Event Breaker Support: **No**
>
{.box .info}

## Configuring Cribl Stream to Receive Data from Pub/Sub

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select  [**Pull** > ] **Google Cloud** > **Pub/Sub**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select  [**Pull** > ] **Google Cloud** > **Pub/Sub** Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

> When working with Google Cloud Pub/Sub, you need to know:
>
> * The project where your credentials originated.
> * The project that contains any topic and subscription you want to work with.
>
> If the desired topic and subscription reside in the same project where your credentials originated, describe them using either short names **or** fully-qualified names.
> 
> If the desired topic and subscription reside in a different project than the one where your credentials originated, you **must** describe them using fully-qualified names.
> 
> Here's an example of a fully-qualified topic name: `projects/my-project-id/topics/my-topic-id`.
>
> The short name for the same topic would be: `my-topic-id`.
> 
{.box .info}

**Input ID**: Enter a unique name to identify this Pub/Sub Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**. 

**Topic ID**: ID of the Pub/Sub topic from which to receive events.

**Subscription ID**: ID of the subscription to use when receiving events.

### Optional Settings

**Create topic**: If toggled to `Yes`, Cribl Stream will create the topic on Pub/Sub if it does not exist.

**Create subscription**: If set to `Yes` (the default), Cribl Stream will create the subscription on Pub/Sub if it does not exist.

**Ordered delivery**: If toggled to `Yes`, Cribl Stream will receive events in the order that they were added to the queue. For ordered delivery to work correctly, ensure that both the process receiving events and the subscription from which messages are read have ordered delivery enabled.

**Region**: Region to retrieve messages from. Select `default` to allow Google to auto-select the nearest region. (If you've enabled **Ordered delivery**, the selected region must be allowed by message storage policy.)

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Authentication

Use the **Authentication method** drop-down to select a Google authentication method:

**Auto**: This option uses the environment variables `PUBSUB_PROJECT` and `PUBSUB_CREDENTIALS`, and requires no configuration here.

**Manual**: With this default option, you use the **Service account credentials** field to enter the contents of your service account credentials file (a set of JSON keys), as downloaded from Google Cloud. 

To insert the file itself, click the upload button at this field's upper right. As an alternative, you can use environment variables, as outlined [here](https://googleapis.dev/ruby/google-cloud-pubsub/latest/file.AUTHENTICATION.html).

**Secret**: Use the drop-down to select a key pair that you've [configured](kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. 

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

  * **Name**: Field name.
  * **Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Max backlog**: When the Destination exerts backpressure, this setting limits the number of events that Cribl Stream will queue for processing before it stops retrieving further events. Defaults to `1000` events.

**Number of concurrent streams**: How many streams to pull messages from at one time. Doubling the value doubles the number of messages this Source pulls from the topic (if available), while consuming more CPU and memory. Defaults to `5`.

**Request timeout (ms)**: Pull request timeout, in milliseconds. Defaults to `60000` ms (i.e., 1 minute).

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

- `__messageId` – ID of the message from Google.
- `__projectId` – ID of the Google project from which the data was received.
- `__publishTime` – Time at which the event was originally published to the Pub/Sub topic.
- `__subscriptionIn` – The subscription from which the event was received.
- `__topicIn` – The topic from which the event was received.

## Google Cloud Roles and Permissions

Your Google Cloud service account should have at least the following roles on subscriptions:

- `roles/pubsub.subscriber`
- `roles/pubsub.viewer` or `roles/viewer`

As an alternative, you can rely on the following discrete permissions, which are included in the above roles:

- `pubsub.snapshots.seek`
- `pubsub.subscriptions.consume`
- `pubsub.topics.attachSubscription`

To enable Cribl Stream's **Create topic** and/or **Create subscription** options, your service account should have one of the following (or higher) roles:

- `roles/pubsub.editor` 
- `roles/editor`

Either `editor` role confers multiple permissions, including those from the lower `viewer`, `subscriber`, and `publisher` roles. For additional details, see the Google Cloud [Access Control](https://cloud.google.com/pubsub/docs/access-control) topic.

## How Cribl Stream Pulls Data

Pub/Sub treats all the Worker Nodes as members of a Consumer Group, and each Worker gets its share of the load from Pub/Sub. This is the same process as normal Kafka. By default, Workers will poll every 1 minute. In the case of Leader failure, Worker Nodes will continue to receive data as normal.

## Troubleshooting

[Snippet not found: content/shared/4.7/snippets/_sources-troubleshooting.md]
