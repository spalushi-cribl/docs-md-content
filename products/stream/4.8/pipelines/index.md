# Pipelines


Data matched by a given [Route](routes) is delivered to a Pipeline. Pipelines are the heart of Cribl Stream processing. Each Pipeline is a list of [Functions](functions) that work on the data.

> As with Routes, the order in which the Functions are listed matters. A Pipeline's Functions are evaluated in order, top‑>down.
>
{.box .info}

## Accessing Pipelines {#configure}

Select **Processing** > **Pipelines** from Cribl Stream's global top nav (single-instance deployments) or from a Worker Group's/Fleet's top nav (distributed deployments). Next, click any displayed Pipeline to see or reconfigure its contained Functions.

> After you've clicked into a Pipeline, the right Preview pane adds a [Pipeline Status](data-preview#status) tab, which you can click to see the Pipeline's events throughput.
>
{.box .success}

### Adding Pipelines

To create a new Pipeline, or to import an existing Pipeline to a different Cribl Stream instance, click **Add Pipeline** at the upper right. The resulting menu offers three options:

- **Create Pipeline**: Configure a new Pipeline from scratch, by adding Functions in Cribl Stream's graphical UI.
- **Import from File**: Import an existing Pipeline from a `.json` file on your local filesystem.
- **Import from URL**: Import an existing Pipeline from `.json` file at a remote URL. (This must be a public URL ending in `.json` – the import option doesn't pass credentials to private URLs – and the target file must be formatted as a valid Pipeline configuration.)

![Creating or importing a Pipeline](pipeline-add-options.e40a3e044e.png)
{border="true"}

> To export a Pipeline, see [Advanced Mode (JSON Editor)](#json). 
> 
> To import or export a Pipeline along with broader infrastructure (like Knowledge Objects and/or sample data files), see [Packs](packs).
>
{.box .success}

## How Do Pipelines Work

Events are always delivered to the beginning of a Pipeline via a Route. The data in the **Stats** column shown below are for the last 15 minutes.

![Pipelines and Route inputs](pipelines-screen-cropped.3cc6cfa3f3.png)
{border="true"}

> You can press the `]` (right-bracket) shortcut key to toggle between the [Preview](data-preview) pane and an expanded Pipelines display. (This shortcut works when no field has focus.)
> 
> In the condensed Pipelines display above, you can also hover over any Pipeline's **Functions** column to see a vertical preview of the stack of Functions contained in the Pipeline:
>
{.box .info}

![Preview on hovering over the top Pipeline (highlighted in gray)](pipeline-functions-preview.f836686377.png)
{border="true"}

Within the Pipeline, events are processed by each Function, in order. A Pipeline will always move events in the direction that points outside of the system. This is on purpose, to keep the design simple and avoid potential loops.

![Pipeline Functions](pipeline-functions.9cb2061fb2.png)
{border="true"}

> Use the **Attach to Route** link at upper left to associate a new Pipeline with a Route.
> 
> You can streamline a complex Pipeline's display by organizing related Functions into [Function groups](functions#groups).
>
{.box .info}

###  Pipeline Settings  {#pipeline-settings}

Click the gear button at the top right to open the Pipeline's Settings. Here, you can:

- Use the **Async function timeout (ms)** to set the maximum amount of processing time, in milliseconds, that a Function is allowed to take before it is terminated. This prevents a Function from causing undesirable delays in your Pipeline (for example, a Lookup Function taking too long to process a large lookup file).

- Use the **Tags** field to attach arbitrary labels to the Pipeline. Once attached, you can use these tags to filter/search and group Pipelines.

![Pipeline Settings](st-pipeline-settings-3.5.ee4ca9104a.png)
{border="true"}

### The `Final` Toggle

The `Final` toggle in Function settings controls what happens to the results of a Function.

When `Final` is toggled to `No` (default), events will be processed by this Function,
**and** then passed on to the next Function below.

When `Final` is toggled to `Yes`, the Function "consumes" the results,
meaning they will not pass down to any other Function below.

A flag in Pipeline view indicates that a Function is `Final`.

![Functions in a Pipeline, some of them have Final flags.](pipeline-final-flag-4.5.png)
{border="true" caption="The Eval and first Drop Functions bearing the Final flag"}

### Advanced Mode (JSON Editor) {#json}

Once you've clicked the gear button to enter [Pipeline Settings](#pipeline-settings), you can click **Manage as JSON** at the upper right to edit the Pipeline's definition in a JSON text editor. In this mode's editor, you can directly edit multiple values. You can also use the **Import** and **Export** buttons here to copy and modify existing Pipeline configurations, as `.json` files.

![Advanced Pipeline Editing](pipeline-advanced-editing-mode.62ac04a92f.png)
{border="true"}

Click **Edit in GUI** at upper right to return to the graphical Pipeline Settings page; then click **Back to &lt;pipeline-name&gt;** to restore the graphical Pipeline editor.

###  Pipeline Actions  {#actions}

Click a Pipeline's Actions (**...**) menu to display options for copying or deleting the Pipeline.

Copying a Pipeline displays a confirmation message and the (highlighted) Paste button shown below.

![Paste button for copied Pipeline](pipelines-paste-button.48e1af9b3f.png)
{border="true"}

Pasting prompts you to confirm, or change, a modified name for the new Pipeline. The result will be an exact duplicate of the original Pipeline in all but name.

### Chaining Pipelines  {#chaining}

You can use the [Chain Function](chain-function) to send the output of a Pipeline to another Pipeline or Pack. There are scope restrictions within Packs, and general guardrails against circular references.

## Types of Pipelines

You can apply various Pipeline types at different stages of data flow. All Pipelines have the same basic internal structure (a series of Functions) – the types below differ only in their position in the system.

![Pre-processing, processing, and post-processing Pipelines](stream-diagram-complex-new-2.7f767535e7.png)
{border="true"}

### Pre-Processing Pipelines {#input-conditioning-pipelines}

These are Pipelines that are attached to a Source to condition (normalize) the events **before** they're delivered to a processing Pipeline. They're optional. 

Typical use cases are event formatting, or applying Functions to **all** events of an input. (E.g., to extract a `message` field before pushing events to various processing Pipelines.) 

You configure these Pipelines just like any other Pipeline, by selecting **Pipelines** from the top menu. You then attach your configured Pipeline to individual [Sources](sources), using the Source's **Pre‑Processing > Pipeline** drop-down.

Fields extracted using pre-processing Pipelines are made available to Routes.

### Processing Pipelines

These are "normal" event processing Pipelines, attached directly to Routes.

### Post-Processing Pipelines {#output-conditioning-pipelines}

These Pipelines are attached to a Destination to normalize the events before they're sent out. A post-processing Pipeline's Functions apply to **all** events exiting to the attached Destination. 

Typical use cases are applying Functions that transform or shape events per receiver requirements. (E.g., to ensure that a `_time` field exists for all events bound to a Splunk receiver.)

You configure these Pipelines as normal, by selecting **Pipelines** from the top menu. You then attach your configured Pipeline to individual  [Destinations](destinations), using the Destination's **Post‑Processing > Pipeline** drop-down. 

You can also use a Destination's **Post‑Processing** options to add **System Fields** like `cribl_input`, identifying the Cribl Stream Source that processed the events.

## Best Practices for Pipelines

Functions in a Pipeline are equipped with their own [filters](introduction-reference#filters). Even though filters are not required, we recommend using them as often as possible. 

As with Routes, the general goal is to minimize extra work that a Function will do. The fewer events a Function has to operate on, the better the overall performance. 

For example, if a Pipeline has two Functions, **f1** and **f2**, and if **f1** operates on `source` `'foo'` and **f2** operates on `source` `'bar'`, it might make sense to apply `source=='foo'` versus `source=='bar'` filters on these two Functions, respectively.
