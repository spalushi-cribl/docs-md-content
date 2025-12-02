# Default Destination


The **Default** Destination simply enables you to specify a default output from among your already-configured Destinations. 

> Type: Internal | TLS Support: N/A | PQ Support: N/A 
>
{.box .info} 

## Configure Cribl Stream's Default Destination

In Cribl Stream configure Default.

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). A **Default** Destination is preconfigured and ready to use in the right **Destinations** column. Hover over the tile and click its **Configure** button to proceed.
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. From the resulting **Destinations** page, click anywhere on the `default` row to proceed.
2. In the **New Destination** modal, configure the following under **General Settings**:
    - **Output ID**: This is prefilled with the default value `default`, which cannot be changed via the UI.
    - **Default Output ID** drop-down to select one of your configured Destinations.
3. Select **Save**, then **Commit & Deploy**. 

## Preventing Circular References

If you've configured an [Output Router](destinations-output-router) Destination with a branch that points to this Default Destination (`default:default` ), you cannot select that Output Router here. This restriction prevents a circular dependency.
