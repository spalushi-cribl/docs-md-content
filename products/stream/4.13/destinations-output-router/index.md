# Output Router Destination


The Output Router Destination enables dynamic, rule-based routing of events to one or more downstream Destinations. It acts as a meta-destination, evaluating each event against an ordered set of filter rules (JavaScript expressions) to determine the appropriate Destinations (outputs). 

This is useful where routing decisions depend on event content, such as sending security logs to a SIEM and application logs to cloud storage, all from a single Pipeline.

**How It Fits Into the Data Flow**

* **Source ingestion**: Events are ingested from Sources.
* **Pre-processing Pipelines**: Pre-processing Pipelines attached to Sources run first. Fields created or modified here are available for routing and Output Router rule evaluation.
* **Route Pipelines**: If configured, regular Pipelines attached to Routes process events next. Fields set here are also available to Output Router rules.
* **Output Router**: The Output Router evaluates its rules (**Filter expressions**) against each event using fields from pre-processing and Route Pipelines. Matching rules determine which Destinations receive the event. If multiple rules match and final is `false`, the event is cloned and sent to all matching Destinations.
* **Destination(s)**: Events are delivered to the selected Destinations.
* **Post-processing Pipelines**: Post-processing Pipelines attached to Destinations run last, after routing. Fields created or modified here are not available to Output Router rules.

> Type: Internal | TLS Support: N/A | PQ Support: N/A 
>
{.box .info} 

## Configure an Output Router Destination

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
1. In the **New Destination** modal, configure the following under **General Settings**:
   - **Router name**: Enter a unique name to identify this Router definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Rules**: A list of event routing rules. Rules are evaluated in order, top‑down. For details, see [Rules](#rules).
1. You can configure the following **Optional Settings**: 
   - **System fields**: A list of fields to automatically add to events that use this Destination. For details, see [System Fields](#fields).
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. On the **Advanced Settings** tab you can configure an **Environment** if you're using GitOps. Optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.
1. Select **Save**, then **Commit & Deploy**. 

### Rules {#rules}

Rules route events by examining event content. These rules are processed sequentially from top to bottom.

> Events that do not match any of the rules are dropped. Use a catch-all rule (`true`) last to avoid dropping unmatched events, like in this [example](#example).
{.box .success}

**Filter expression**: A JavaScript expression to select events to route to Destinations. Filter expressions evaluate event fields after [pre‑processing](pipelines#input-conditioning-pipelines) (input conditioning at the Source) and after any [Route Pipeline](pipelines#processing-pipelines) processing, but before any [post-processing](pipelines#output-conditioning-pipelines) (output conditioning at the Destination). Fields created or modified in pre-processing or Route Pipelines are available to Output Router filter expressions. Fields created or modified during post-processing are not available, as post-processing occurs after routing decisions are made.

**Output**: The Destination to send matching events to. This can be any configured Destination, except for another Output Router or a [Default Destination](destinations-default) that points back to an Output Router.

**Description**: Optionally, enter a description of this rule's purpose.

**Final**: By default (toggled on), event routing stops after this rule matches. Toggle off if you want the event to be evaluated against other rules lower in the stack, allowing cloning to multiple Destinations.

### System Fields {#fields}

By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

## Example

Scenario: 

* Send all events where `host` starts with `66` to Destination `San Francisco`.
* From the rest of the events: Send all events with `method` field `POST` or `GET` to both `Seattle` and `Los Angeles` (i.e., clone them). 
* Send the remaining events to `New York City`.

Router Name: **router66**

<table>
<thead>
<tr>
<th>Filter Expression</th>
<th>Output</th>
<th>Final</th>
</tr>
</thead>

<tbody>
<tr>
<td><code>host.startsWith(&#39;66&#39;)</code></td>
<td><strong>San Francisco</strong></td>
<td>Yes</td>
</tr>
<tr>
<td><code>method==&#39;POST&#39; || method==&#39;GET</code></td>
<td><strong>Seattle</strong></td>
<td>No</td>
</tr>
<tr>
<td><code>method==&#39;POST&#39; || method==&#39;GET&#39;</code></td>
<td><strong>Los Angeles</strong></td>
<td>Yes</td>
</tr>
<tr>
<td><code>true</code></td>
<td><strong>New York</strong></td>
<td>Yes</td>
</tr>
</tbody>
</table>


