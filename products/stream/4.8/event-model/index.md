# Event Model


Cribl Stream bases all data processing on discrete data entities commonly known 
as **events**. An event is a collection of key-value pairs (fields). Some 
[Sources](sources) deliver events directly, while others might deliver 
bytestreams that need to be broken up by [Event Breakers](event-breakers). 
Events travel from a Source through [Pipelines'](pipelines) [Functions](functions), 
and on to [Destinations](destinations).  

The internal representation of a Cribl Stream event is as follows: 

```json {title="Event Model"}
{
  "_raw": "<body of non-JSON parse-able event>",
  "_time": "<timestamp in UNIX epoch format>",
  "__inputId": "<Id/Name of Source that delivered the event>",
  "__other1": "<Internal field1>",
  "__other2": "<Internal field2>",
  "__otherN": "<Internal fieldN>",
  "key1": "<value1>",
  "key2": "<value2>",
  "keyN": "<valueN>",
  "...": "..."  
}
```

## Internal Fields

Cribl Stream adds internal fields during Pipeline data processing. They 
serialize only to Cribl [internal Destinations](destinations#available-internal-destinations).

A few notes about internal fields:

* Internal fields start with a double-underscore.

* Each [Source](sources) can add one or many internal fields to each event. For 
  example, [Syslog](sources-syslog) adds both a `__inputId` and a `__srcIpPort` 
  field.

* Treat internal fields as read-only. Modifying them can result in unintended 
  consequences to your data flows.

* Upon arrival from a [Source](sources), if an event cannot be JSON-parsed, Cribl 
  assigns all of its content to `_raw`.

* If you do not configure a timestamp for extraction, the Pipeline process 
  assigns the current time (in UNIX epoch format) to the  `_time` field. (Note 
  that when Cribl Stream emits events based on incoming Kafka messages, it 
  handles the `_time` field a bit differently. See 
  [Kafka Source](sources-kafka#how-product-handles-the-_time-field) for more 
  information.)

* Cribl reserves the right to change all internal fields that are not documented 
  under Sources or Destinations.

## System Fields

Cribl Stream automatically can add system fields to events during 
post-processing. By default, this includes `cribl_pipe`, a field that identifies 
the Cribl Stream Pipeline that processed the event. Other additional system 
fields include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

A few notes about system fields:

* System fields start with the string `cribl_`.

* Treat system fields as read-only. Like internal fields, modifying system 
  fields can result in unintended consequences to your data flows. Adding a 
  Pipeline function to trim system fields will not successfully remove system 
  fields since Cribl Stream adds these fields during post-processing.

* Cribl reserves the right to change all system fields that are not documented 
  under Sources or Destinations.


## Using Capture 

Use the Capture tool to:

- Troubleshoot events.
- Observe what an event looks like as it travels through the system.

While in [Preview](data-preview) (right pane):

1. Select **Capture Data**. 

2. In the resulting modal, enter a **Filter expression** to narrow down the 
   events of interest. 

3. Select **Capture...** and (optionally) change the default Time and/or Event 
   limits. 

4. Select the desired **Where to capture** option. There are four options: 

  |   | Option | Description |
  | - | ------ | ----------- |
  | 1 | **Before the pre-processing Pipeline** | Capture events right after they're delivered by the respective Input (and after PQ, if enabled). |
  | 2 | **Before the Routes** | Capture events right after the pre-processing Pipeline. before they go down any Routes ([QuickConnect](quickconnect) bypasses Routes, and can bypass processing Pipelines.) |
  | 3 | **Before the post-processing Pipeline** | Capture events right after any Processing Pipeline that handled them, before any post-processing Pipeline. |
  | 4 | **Before the Destination** | Capture events right after any post-processing Pipeline, before they go out to the configured Destination. |

![Illustration of the Capture options in the Pipeline](cribl-diagram-in-outs.png)
