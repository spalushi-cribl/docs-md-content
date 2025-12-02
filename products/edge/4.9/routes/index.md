# Routes


Before incoming events are transformed by a processing Pipeline, Cribl Edge uses a set of filters to first select a **subset** of events to deliver to the correct Pipeline. This selection is normally made via Routes. 

> ##### Don't Need Routes? {{< id `no-routes` >}}
>
> Routes are designed to filter, clone, and cascade incoming data across a related set of Pipelines and Destinations. If all you need are independent connections that link parallel Source/Destination pairs, you can use Cribl Edge's [QuickConnect](quickconnect) rapid visual configuration tool as an alternative to Routes.
>
{.box .success}

## Accessing Routes {#configure}

To access Routes:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Data Routes**.

   > On a Single-instance deployment, select **More** straight from the **Home** submenu.
   {.box .success}

1. To configure a new Route, select **Add Route**.

## How Do Routes Work

Routes apply filter expressions on incoming events to send matching results to the appropriate Pipeline. Filters are JavaScript-syntax–compatible expressions that are configured with each Route. Examples are: 
- `true` 
- `source=='foo.log' && fieldA=='bar'`

> There can be multiple Routes in a Cribl Edge deployment, but each Route can be associated with only **one processing** Pipeline. (Sources and Destinations can also have their own attached pre-/post-processing [Pipelines](pipelines#types-of-pipelines).)
> 
> Each Route must have a unique name. The **Route Name** field is case-sensitive, so entering the same characters with different capitalization will create a Route with a separate name.
>
{.box .info}

Routes are evaluated in their display order, top‑>down. The stats shown in the **Bytes**/**Events** (toggle) column are for the most-recent 15 minutes.

![Routes and bytes](routes-4.9.png)
{border="true"}

In the example above, incoming events will be evaluated first against the Route named **speedtest**, then against **mtr**, then against **statsd**, and so on. At the end, the **default** Route serves as a catch-all for any event that does not match any of the other Routes.

Above, note the **Events In** drop-down menu to toggle between displaying Events versus Bytes, and to display In, Out, or Dropped.

Displayed percentages of Events and Bytes In and Out are by comparison to throughput or filtering across all Routes. For example, if you have four Routes that share an identical filter, each Route's Events In might show approximately `25%` of the total inbound traffic across all Routes.

## Managing the Routes Page

To apply a Route before another, simply drag it vertically. Use the toggles to turn Routes `On`/`Off` inline, as necessary, to facilitate development and debugging. 

You can press the `]` (right-bracket) shortcut key to toggle between the [Preview](data-preview) pane and the expanded Routes display shown above. (This works when no field has focus.)

Click a Route's Options (•••) menu to display multiple options:
* Insert, group, move, copy, or delete Routes.
* Insert comments above or below Routes.
* Capture sample data through a selected Route. 

![Route > Options menu](routes-context-menu-4.9.png)
{border="true"}

Copying a Route displays a confirmation message and adds a Paste button next to **Add Route**.
Pasting creates an exact duplicate of the Route, with a warning indicator to change its duplicate name.

You can use comments to describe Routes' purposes and/or interactions.

![Comments make the Routing table self-documenting](routes-insert-comment-4.9.png)
{border="true"}

## Output Destination 

You can configure each Route with an **Output** that defines a [Destination](destinations) to send events to, after they're processed by the Pipeline.

If you toggle **Enable expression** to `Yes`, the **Output** field changes to an **Output expression** field. Here, you can enter a JavaScript expression that Cribl Edge will evaluate as the name of the Destination. Sample expression format: `` `myDest_${C.logStreamEnv}` ``. (This evaluation happens at Route construction time, not per event.)

![Output expression enabled](routes-output-expression-4.9.png)
{border="true" scale="50%"}

> Expressions that match no Destination name will silently fail.
> 
> On Routes within Packs, neither **Output** control is available, because Packs cannot specify Destinations.
>
{.box .warning}

## The `Final` Toggle

The `Final` toggle in Route settings controls what happens to an event after a Route-Pipeline pair matches it.

When `Final` is toggled to `Yes` (default), the Route "consumes" an event,
meaning the event will not pass down to any other Route below.

![Diagram of events flowing through a Route with the Final toggle, where only non-matched events pass on to further Routes.](routes-final-yes.e7ab80d7c2.png)
{caption="The Route is Final and non-matched events stop here"}

When `Final` is toggled to `No`, the event will be processed by this Route,
**and** then passed on to the next Route below.

![Diagram of events flowing through a Route with the Final toggle disabled, where all events pass on to further Routes.](routes-final-no.3ef631ae0d.png)
{caption="The Route isn't Final and all events pass further on"}

When you toggle `Final` to `No`, the event sent to the current Route technically becomes a *clone* of the original event.
The **Add Clone** button lets you add a fields – names and values – to the cloned events sent to this Route's Pipeline.

![Non-final Route: add clone fields](routes-clones-4.9.png)
{border="true" scale="40%"}

### Event Cloning Strategy

Depending on your cloning needs, you might want to follow a **most-specific first** or a **most-general first** processing strategy.
The general goal is to minimize the number of filters/Routes an event gets evaluated against. For example:  

If you don't need to clone any events (that is, `Final` is toggled to `Yes` everywhere),
then it makes sense to start with the broadest expression at the top.
This will consume as many events as early as possible. 

If you need to clone a narrow set of events, then it might make sense to do that upfront.
Then, follow it with a Route that consumes those clones immediately after.

## The `endRoute` Bumper {#endroute}

The `endRoute` bumper appears at the bottom of the Routing table. This is a reminder that, if **no** Route in the table has the `Final` flag enabled, events will continue to Cribl Edge's configured [Default Destination](destinations-default).

![endRoute warning and link](endroute-bumper.f145476ba2.png)
{border="true"}

This is a backstop to ensure data flow. However, if that configured default is also configured as the **Output** of a Route higher in the table, duplicate events will reach that Destination.

You can correct this either by setting a Route to `Final`, or by changing the Default Destination. The bumper provides a link to the Default Destination's config, and identifies the currently configured default in parentheses.

## Route Groups {#groups} 

A Route Group is a collection of consecutive Routes that can be moved up and down the Route stack together. Groups help with managing long lists of Routes. They are a UI visualization only: While Routes are in a group, those Routes maintain their global position order.

> Route groups work much like [Function groups](functions#groups), offering similar UI controls and drag-and-drop options.
>
{.box .info}

## Unreachable Routes {#unreachable}

Routes display an "unreachable" warning indicator (orange triangle) when data can't reach them. 

![Unreachable Route warning, on hover](unreachable-route-tooltip.5be1da8809.png)

This condition will occur when, with your current configuration, any Route higher in the stack matches **all** three of these conditions:

- Previous Route is enabled.
- Previous Route is final (**Final** toggle is set to `Yes`).
- Previous Route's **Filter** expression evaluates to true, (e.g., `true`, `1 === 1`, etc.).

Note that the third condition above can be triggered intermittently by a randomizing method like `Math.random()`. This might be included in a previous Route's own Filter expression, or in a Pipeline Function (such as one configured for random data [sampling](sampling-function)).

## Routing with Output Router

[Output Router](destinations-output-router) Destinations offer another way to route data. These function as meta-Destinations, in that they allow you to send data to multiple peer Destinations based on rules. Rules are evaluated in order, top‑>down, with the first match being the winner.
