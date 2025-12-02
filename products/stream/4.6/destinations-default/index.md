# Default Destination


The **Default** Destination simply enables you to specify a default output from among your already-configured Destinations. 

> Type: Internal | TLS Support: N/A | PQ Support: N/A 
>
{.box .info} 

## Configuring Cribl Stream's Default Destination

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). A **Default** Destination is preconfigured and ready to use in the right **Destinations** column. Hover over the tile and click its **Configure** button to proceed.

 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Default**. From the resulting **Manage Default Destination** page, click anywhere on the `default` row to proceed.

![Default Destination – click to configure](default-destination.a77fac711b.png)

In the resulting **Destinations > Default** drawer or modal, use the **Default Output ID** drop-down to select one of your configured Destinations. After you click **Save**, this will become Cribl Stream's default Destination.

The only other field here is the **Output ID**, whose value is locked to `default`.

## Preventing Circular References

If you've configured an [Output Router](destinations-output-router) Destination with a branch that points to this Default Destination (`default:default` ), you cannot select that Output Router here. This restriction prevents a circular dependency.
