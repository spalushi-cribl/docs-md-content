# Pipelines


Data matched by a given [Route](routes) is delivered to a Pipeline. Pipelines are the heart of Cribl Edge processing. Each Pipeline is a list of [Functions](functions) that work on the data.

> As with Routes, the order in which the Functions are listed matters. A Pipeline's Functions are evaluated in order, top‑>down.
>
{.box .info}

## Access Pipelines {#configure}

To access Routes:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Pipelines**.

   > On a Single-instance deployment, select **More** straight from the **Home** submenu.
   {.box .success}

After you've opened a Pipeline, the right Preview pane adds a [Pipeline Status](data-preview#status) tab, which you can select to see the Pipeline's events throughput.

### Add Pipelines

To create a new Pipeline, or to import an existing Pipeline to a different Cribl Edge instance, select **Add Pipeline** at the upper right. The resulting menu offers three options:

- **Add Pipeline**: Configure a new Pipeline from scratch, by adding Functions in Cribl Edge's graphical UI.
- **Add Pipeline with Cribl Copilot**: Use the Cribl Copilot Pipeline assistant to create a Pipeline using AI. See [Create Pipelines Using Cribl Copilot](copilot-pipelines) to learn more.
- **Import from File**: Import an existing Pipeline from a `.json` file on your local filesystem.
- **Import from URL**: Import an existing Pipeline from `.json` file at a remote URL. (This must be a public URL ending in `.json` – the import option doesn't pass credentials to private URLs – and the target file must be formatted as a valid Pipeline configuration.)

![Creating or importing a Pipeline](pipeline-add-options-4.9.png)
{border="true"}

> To export a Pipeline, see [Advanced Mode (JSON Editor)](#json). 
> 
> To import or export a Pipeline along with broader infrastructure (like Knowledge Objects and/or sample data files), see [Packs](packs).
>
{.box .success}

## How Pipelines Work

Events are always delivered to the beginning of a Pipeline via a Route. The data in the **Stats** column shown below are for the last 15 minutes.

![Pipelines and Route inputs](pipelines-screen-4.9.png)
{border="true"}

> You can press the `]` (right-bracket) shortcut key to toggle between the [Preview](data-preview) pane and an expanded Pipelines display. (This shortcut works when no field has focus.)
> 
> In the condensed Pipelines display above, you can also hover over any Pipeline's **Functions** column to see a vertical preview of the stack of Functions contained in the Pipeline:
>
{.box .info}

![Preview on hovering over the top Pipeline (highlighted in gray)](pipeline-functions-preview.f836686377.png)
{border="true"}

Within the Pipeline, events are processed by each Function, in order. A Pipeline will always move events in the direction that points outside of the system. This is on purpose, to keep the design simple and avoid potential loops.

![Pipeline Functions](pipeline-functions-4.9.png)
{border="true"}

> Use the **Attach to Route** link at the top to associate a new Pipeline with a Route.
> 
> You can streamline a complex Pipeline's display by organizing related Functions into [Function groups](functions#groups).
>
{.box .info}

###  Pipeline Settings  {#pipeline-settings}

Select the gear button at the top right to open the Pipeline's Settings. Here, you can:

- Use the **Async function timeout (ms)** to set the maximum amount of processing time, in milliseconds, that a Function is allowed to take before it is terminated. This prevents a Function from causing undesirable delays in your Pipeline (for example, a Lookup Function taking too long to process a large lookup file).

- Use the **Tags** field to attach arbitrary labels to the Pipeline. Once attached, you can use these tags to filter/search and group Pipelines.

![Pipeline Settings](ed-pipeline-settings-3.5.ee4ca9104a.png)
{border="true"}

### The `Final` Toggle

The `Final` toggle in Function settings controls what happens to the results of a Function.

When `Final` is toggled to `No` (default), events will be processed by this Function,
**and** then passed on to the next Function below.

When `Final` is toggled to `Yes`, the Function "consumes" the results,
meaning they will not pass down to any other Function below.

An icon in Pipeline view indicates that a Function is `Final`.

![Functions in a Pipeline, some of them have Final icons.](pipeline-final-flag-4.9.png)
{border="true" caption="The Eval and first Drop Functions bearing the Final icons"}

### Advanced Mode (JSON Editor) {#json}

Once you've selected the gear button to enter [Pipeline Settings](#pipeline-settings), you can select **Manage as JSON** at the upper right to edit the Pipeline's definition in a JSON text editor. In this mode's editor, you can directly edit multiple values. You can also use the **Import** and **Export** buttons here to copy and modify existing Pipeline configurations, as `.json` files.

![Advanced Pipeline Editing](pipeline-advanced-editing-mode-4.9.png)
{border="true"}

Select **Edit in GUI** at upper right to return to the graphical Pipeline Settings page; then select **Back to &lt;pipeline-name&gt;** to restore the graphical Pipeline editor.

###  Pipeline Actions  {#actions}

Select a Pipeline's Actions (**...**) menu to display options for copying or deleting the Pipeline.

Copying a Pipeline displays a confirmation message and the (highlighted) Paste button shown below.

![Paste button for copied Pipeline](pipelines-paste-button-4.9.png)
{border="true"}

Pasting prompts you to confirm, or change, a modified name for the new Pipeline. The result will be an exact duplicate of the original Pipeline in all but name.

### Chain Pipelines  {#chaining}

You can use the [Chain Function](chain-function) to send the output of a Pipeline to another Pipeline or Pack. There are scope restrictions within Packs, and general guardrails against circular references.

## Types of Pipelines

You can apply various Pipeline types at different stages of data flow. All Pipelines have the same basic internal structure (a series of Functions) – the types below differ only in their position in the system.

![Pre-processing, processing, and post-processing Pipelines](edge-diagram-complex-new.7145e71e7c.jpg)
{border="true"}

### Pre-Processing Pipelines {#input-conditioning-pipelines}

These are Pipelines that are attached to a Source to condition (normalize) the events **before** they're delivered to a processing Pipeline. They're optional. 

Typical use cases are event formatting, or applying Functions to **all** events of an input. (For example, to extract a `message` field before pushing events to various processing Pipelines.) 

You configure these Pipelines just like any other Pipeline, by selecting **More** > **Pipelines** from the **Fleets** submenu. You then attach your configured Pipeline to individual [Sources](sources), using the Source's **Pre‑Processing** > **Pipeline** drop-down.

Fields extracted using pre-processing Pipelines are made available to Routes.

### Processing Pipelines

These are "normal" event processing Pipelines, attached directly to Routes.

### Post-Processing Pipelines {#output-conditioning-pipelines}

These Pipelines are attached to a Destination to normalize the events before they're sent out. A post-processing Pipeline's Functions apply to **all** events exiting to the attached Destination. 

Typical use cases are applying Functions that transform or shape events per receiver requirements. (For example, to ensure that a `_time` field exists for all events bound to a Splunk receiver.)

You configure these Pipelines as normal, by selecting **More** > **Pipelines** from the **Fleets** submenu. You then attach your configured Pipeline to individual [Destinations](destinations), using the Destination's **Post‑Processing > Pipeline** drop-down. 

You can also use a Destination's **Post‑Processing** options to add **System Fields** like `cribl_input`, identifying the Cribl Edge Source that processed the events.

## Best Practices for Pipelines

Functions in a Pipeline are equipped with their own [filters](introduction-reference#filters). Even though filters are not required, we recommend using them as often as possible. 

As with Routes, the general goal is to minimize extra work that a Function will do. The fewer events a Function has to operate on, the better the overall performance. 

For example, if a Pipeline has two Functions, **f1** and **f2**, and if **f1** operates on `source` `'foo'` and **f2** operates on `source` `'bar'`, it might make sense to apply `source=='foo'` versus `source=='bar'` filters on these two Functions, respectively.
