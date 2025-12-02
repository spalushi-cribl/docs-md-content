# Moogsoft/Webhook Integration


> Written for Cribl by Douglas Bothwell, formerly of Moogsoft.
>
{.box .info}

You can configure Cribl Stream to send Webhook notifications to Moogsoft, using custom integrations. A custom integration is a user-defined Moogsoft endpoint that ingests JSON payloads, and converts them to Moogsoft events or metrics.

Note these options:

* A Moogsoft custom integration can ingest either events or metrics. Each custom integration has its own separate API key, and its own Cribl-to-Moogsoft mappings.    
* There is no limit to the number of custom integrations you can create.
* The examples below illustrate simple event and metric mappings. You can define your own mappings for each endpoint, based on the data types and payloads you want to send. For details, see Moogsoft's [Create Your Own Integration](https://docs.moogsoft.com/en/create-your-own-integration--cyoi.html) documentation.

## Create the Moogsoft Custom Integration

Log into your Moogsoft SaaS instance as an Owner or Administrator, and then:
1.  Choose **Data Config** > **Ingestion Services** > **Create Your Own Integration**.    
2.  Select **Add New Integration**.
3.  Specify an integration endpoint and description.
4.  Specify the data type to send to the endpoint: **Events** or **Metrics**.
    
The integration setup screen appears, with the URL and the API key for the new endpoint.

![The Moogsoft integration setup screen](moogsoft-01.fbbdbbf11e.png)
{border="true"}

## Configure the Webhook Destination

Navigate to **Products** > **Stream** > **Worker Groups**. Select a Worker Group, then go to  **Data** > **Destinations**. Select **Webhook**. Next, select **Add Destination** to open the **Webhook** > **New Destination** modal, and configure the following.

### General Settings 

**URL**: The URL for the new custom integration (copy this from the Moogsoft UI).

**Method**: `POST`.

**Format**: `Custom`.

**Content type**: `application/json`.

### Advanced Settings

**Extra HTTP Headers**: Select **Add Header** to define this extra header: 

| **Name** | **Value** |
| --- | --- |
| `apiKey` | The API key for the new custom integration (copy this from the Moogsoft UI). |

Select **Save**, then **Commit** and **Deploy**. Now you are ready to map your events and/or metrics.
    
## Mapping Cribl Data to Moogsoft

Your mappings will differ, depending on the data you want to send. However, the simple examples below, in which we map a Cribl Stream payload to a Moogsoft custom integration, illustrate principles that apply to all custom mappings:
* Moogsoft has a defined [event schema](https://api.docs.moogsoft.com/reference/events-api-object) and [metric schema](https://api.docs.moogsoft.com/reference/metric-datum-api-object). Each schema includes a set of required fields. Your custom integration must include mappings for all required fields.
* You can define custom tags for Cribl Stream fields that do not have Moogsoft equivalents.  
* You can also specify default values in case Cribl Stream sends an object with a missing field.  
* The Moogsoft event schema includes a `severity` field. You can map Cribl Stream fields and values to Moogsoft severities, or define a default severity if a payload does not include this information.
    
Define and validate your mappings based on the type of data you plan to send to Moogsoft:
* [Event Mapping](#event-mapping)
* [Metric Mapping](#event-mapping)
    
## Event Mapping {#event-mapping}

To illustrate how to map Cribl Stream payloads to Moogsoft events, we'll walk through an example with a payload of Cribl Stream syslog messages. You'll follow the same overall workflow for any events payload.

### Send a Sample Payload to Moogsoft

In Cribl Stream, navigate to your Webhook Destination's configuration modal, select the **Test** tab, then:
1. In the **Test input** field, define one or more JSON payloads for the Cribl data you want to send. To map syslog events, set the **Select sample** drop-down to **syslog.log**.
2. Select **Test**.

### Map the Event Fields

In Moogsoft, view the custom integration's config screen. Under **Map Your Data**, you should now see the payload you just sent.

![The event payload appears in Moogsoft](moogsoft-02.b8d79c415e.png)
{border="true"}

Select the payload, and then define your Cribl-to-Moogsoft mappings. Your Cribl Stream data will largely determine the mappings you want. See [Events Object](https://api.docs.moogsoft.com/reference/events-api-object) in the Moogsoft API docs.

Here are some reasonable mappings for the syslog payload in this example:

| Cribl Source Field(s) | Moogsoft Target Field |
| --- | --- |
| `message` | `description` |
| `appname` | `service` |
| `facilityName` | `check` |
| `severity`, `severityName` | `severity` |
| `procid` | `tag.process-id` |

### Map the Severities

The Moogsoft event schema has a severity field. You can specify integers or strings, from `0`, meaning Clear, to `5`, meaning Critical. Here are some reasonable mappings.

| Syslog Severities | Moogsoft Severities |
| --- | --- |
| Emergency (`0`), Alert (`1`), Critical (`2`) | Critical (`5`) |
| Error (`3`) | Major (`4`) |
| Warning (`4`) | Warning (`2`) |
| Informational (`6`), Debug (`7`) | Unknown (`1`) |
| Notice (`5`) | Clear (`0`) |

### Verify your Mappings

Once you save and apply your mappings in Moogsoft, do the following:

1. In Cribl Stream, return to the **Test** tab for the Webhook Destination. Select **Test** again to send another payload.
2. In Moogsoft, go to the **Alerts** screen. You should now see a new alert based on the payload you just sent.
    
## Metric Mapping

To illustrate how to map Cribl Stream payloads to Moogsoft events, we'll walk through an example with a payload of Cribl Stream syslog messages. You'll follow the same overall workflow for any metrics payload.

### Send a Sample Payload to Moogsoft

In Cribl Stream, navigate to your Webhook Destination's configuration modal, select the **Test** tab, then:
1. In the **Test input** field, define one or more JSON payloads for the Cribl data you want to send. To map syslog events, set the **Select sample** drop-down to **appscope-metrics.log**.
2. Select **Test**.

### Map the Metrics Fields

In Moogsoft, view the custom integration's config screen. Under **Map Your Data**, you should now see the payload you just sent.

![The metrics payload appears in Moogsoft](moogsoft-03.d53bef086f.png)
{border="true"}

Select the payload and then define the Cribl-to-Moogsoft mappings you want. Your Cribl data will largely determine these mappings. See [Metric Datum Object](https://api.docs.moogsoft.com/reference/metric-datum-api-object) in the Moogsoft API docs.

Here are some reasonable mappings for the AppScope payload you just sent:

| Cribl Source Fields | Moogsoft Target Field |
| --- | --- |
| `_metric` | `metric` |
| `_value` | `data` |
| `host` | `source` |
| `unit` | `tag.unit` |
| `pid` | `tag.pid` |

### Verify Your Mappings

After you save and apply your mappings in Moogsoft, test them like this:

1. In Cribl Stream, return to the Webhook Destination's **Test** tab. Select **Test** again to send another payload.
2. In Moogsoft, go to the **Metrics** screen. You should now see a new alert, based on the payload you just sent.
