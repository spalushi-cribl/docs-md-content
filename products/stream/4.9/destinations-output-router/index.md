# Output Router Destination


Output Routers are meta-destinations that allow for output selection based on rules. Rules are evaluated in order, top‑down, with the first match being the winner. 

> Type: Internal | TLS Support: N/A | PQ Support: N/A 
>
{.box .info} 

## Configure Cribl Stream to Send to an Output Router

In Cribl Stream, configure Output Router: 

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Router name**: Enter a unique name to identify this Router definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Rules**: A list of event routing rules. For details, see [Rules](#rules) below.
3. Next, you can configure the following **Optional Settings**: 
   - **System fields**: A list of fields to automatically add to events that use this output. For details, see [System Fields](#fields) below.
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Rules {#rules}

Configure the following event routing rules settings.

- **Filter expression**: JavaScript expression to select events to send to output.
-  **Output**: Output to send matching events to.
-  **Description**: Optionally, enter a description of this rule's purpose.
-  **Final**: Toggle to `No` if you want the event to be checked against other rules lower in the stack.

### System Fields {#fields}

By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

##  Limitations/Options  {#notes}

* An Output Router cannot reference another. This is by design, to avoid circular references.
* Also to avoid circular references, an Output Router cannot reference a Default Destination that points back to Output Router.
* **Events that do not match any of the rules are dropped**. Use a catch-all rule to change this behavior.
* No post-processing (conditioning) can be attached to the Output Router Destination itself. But you are free to use [pre‑processing Pipelines](pipelines#input-conditioning-pipelines) on the Source tier, and [post-processing Pipelines](pipelines#output-conditioning-pipelines) on any or all Destinations that the Output Router references.
* Data can be cloned by toggling the `Final` flag to `No`. (The default is `Yes`, i.e., no cloning.)

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


