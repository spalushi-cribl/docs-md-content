# QuickConnect


The QuickConnect visual rapid-development UI enables you to visually connect Cribl Edge inputs (Sources) to outputs (Destinations) through simple drag-and-drop. 

You can insert [Pipelines](pipelines) or [Packs](packs) into the connections, to take advantage of Cribl Edge's full range of data-transformation [Functions] (functions). Or you can omit these processing stages entirely, to send incoming data directly to Destinations – with minimal configuration fuss.

QuickConnect was designed as a simple, quick way to prototype, test, and send data flowing through Cribl Edge. But it's also entirely suitable for configuring a production deployment, if your needs are restricted to sending data through parallel paths, and you don't need to use [Routes](routes). (See [QuickConnect Versus Routes](#routes) to help you clarify that choice.)

![QuickConnect UI](quickconnect-basic.c233cf9716.png)

> You also have the option to start with simple direct connections, and later add [Pipelines](pipelines)' and/or [Packs](packs)' processing power at up to three stages of each connection.
>
{.box .success}

## Initiating QuickConnect  {#initiating}

To access QuickConnect, from a single-instance or individual **Edge Node**, click **Collect**.

To display QuickConnect in a distributed deployment, you must first select a **Fleet**. Click the **Manage** link to select the `default` or a different Fleet.

> You can switch to Cribl Edge's Routes UI at any time from the top nav by selecting **Routing** > **Data Routes**. To toggle back, select **Routing** > **QuickConnect**. 
>
{.box .success}

## QuickConnect Versus Routes  {#routes}

QuickConnect provides access to most of Cribl Edge's configuration options, with a few restrictions (in exchange for the simplified visual interface):

 - QuickConnect completely bypasses Routes – which is why every QuickConnect path is independent and parallel. There is no Route-level data filtering, and you forego the Cribl Edge Routing table's options to clone and cascade data across Pipelines.
 - Most [Sources](sources) are available to fully configure, with a few current exclusions.
 - Each supported Source's config can be created in either QuickConnect or Routes, and will reside only in that context. But you can [switch a config](#switching) between the two contexts, or you can create an identical config on the opposite side.
 - All [Destinations](destinations) are available to fully configure.
 - Everything you configure in QuickConnect exists separately from everything you configure in Data Routes. (But remember that you can use the **Routing** menu to toggle between contexts, and then replicate a QuickConnect configuration on the Data Routes side, or vice versa.)

## Using QuickConnect  {#using}

QuickConnect is designed to be nearly self-documenting. To get you going quickly, it displays the following Sources and Destinations straight out of the box:

![QuickConnect UI](quickconnect-sources.52ae8fa017.png)

### Enabling Sources and Destinations  {#integrations}

When you click to add Sources and Destinations, their configuration options will open in a drawer. Fill in a unique **Input ID**, and any other required fields marked by an asterisk (`*`). Then click **Save**.

### Switching Contexts  {#switching}

If you switch an **existing** Source to QuickConnect – for example, one of the preconfigured Sources that ship with Cribl Edge – you'll see the dialog shown below. This is due to QuickConnect's independence from the [Routes](#routes) interface – you need to confirm your choice to move the existing configuration from the Data Routes world to the QuickConnect world. 

![Switching a Source/Destination between worlds](quickconnect-switch-from-routes.eb75d53cfd.png)

> If you switch an existing Source config to QuickConnect, its data will no longer flow through its configured Route.
>
{.box .warning}

You can also go the opposite way: move a Source originally configured in QuickConnect to the Routing interface. In the Source's drawer or config modal, select the **Connected Destinations** left tab, and then click the resulting tab's **Send to Routes** button. Here again, you'll need to confirm your choice before proceeding.

![Switching a Source/Destination between worlds](quickconnect-switch-to-routes-raw.a991d4d311.png)

### Selecting a Pipeline or Pack  {#bottom-drawer}

When you add or modify a connection line, the bottom **Connection Configuration** drawer will prompt you to select a **Passthru**, **Pipeline**, or **Pack** connection. Selecting either **Pipeline** or **Pack** will open an **Add...** modal like this in the bottom drawer:

![QuickConnect versus Routing UIs](qc-pipeline-select.14feb4736f.png)

Here, you're selecting among already-configured Pipelines or Packs. **Resist the temptation to click on a bold blue Pipeline or Pack name.** (Doing so will open the Pipeline's or Pack's config in a separate browser tab, which might offer more configuration than you bargained for.) 

Instead, click the subtle radio button to the left of the Pipeline or Pack you want to select. (Or click on some black text around the center of your desired Pipeline's/Pack's row, which will fill in its radio button.) Then click **Save** to confirm your choice and close the drawer.

### Multiple Connections  {#multiple}

You can drag multiple connection lines between a given Source/Destination pair. (You might do this, for example, to configure parallel Pipelines to handle different data types.)

Also, you can connect one Source to multiple Destinations, or multiple Sources to one Destination. Let's isolate examples of both, from the screen capture we started with:

![Multiple connections out, multiple connections in](quickconnect-multiple.110a681968.png)

### Source/Destination Tiles' Hover Options  {#hover-options}

Once you've configured a Source or Destination, hovering over its tile will display options like these:

![Signed, sealed, delivered: tiles' on-hover options](qc-tile-on-hover.24b00994bc.png)
{scale="50%"}

The leftmost button is an indicator of the Source's/Destination's state. Live means healthy; disabled, error, or warning states will also appear here. Note that a Source will appear as **Disabled** until you connect it to a Destination. 

![This Source is not misconfigured – it's just lonely](qc-source-disconnected.18f42b7037.png)
{scale="65%"}

Clicking the middle **Configure** button reopens the Source's Destination's configuration drawer, where you can address any problems or simply update the configuration.

The right **Capture** button captures a sample of data flowing through the Source or Destination, in a drawered version of the Source's/Destinations's config modal > [Live tab]](data-preview#capturing-from-a-single-source-or-destination). (Not to be confused with a healthy tile's left **Live** indicator.)

### Group Connections  {#stacking}

Once you've configured two or more copies of the same Source or Destination type – for example, to support different configurations of one integration or protocol – QuickConnect stacks their tiles together to keep the display clean.

![4 similar Destinations, stacked](qc-stacked-tiles.ec821511ac.png)
{scale="55%"}

> The number at the stack's upper right shows how many tiles it contains.
>
{.box .success}

Hovering over a stack doesn't change its options...until you click on it. This will expand all the tiles in the group, so each tile can now reveal the [on-hover options](#hover-options) described above.

You can drag multiple connection lines from, or to, the same stack. If you drag or modify a connection from or to a stacked group, the UI will prompt you whether to similarly expand the group to access individual tiles.


