# Windows SID Lookup


The Windows SID Lookup Function translates Windows SID (security identifiers) into canonical account names for use in downstream Pipeline steps.

The Function uses the local Windows host where the Edge Node is running to resolve the SID, relying on OS resolution for domain objects.

> The Windows SID Lookup Function works only in Cribl Edge on Windows.
> 
> Because Cribl Leader Nodes always run on Linux, previewing the results of the Windows SID Lookup Function from the Leader will not work.
> Instead, to preview the results, [teleport](/edge/managing-edge-nodes#teleport-into-an-edge-node) into an Edge Node running on Windows.
{.box .info}

## How the Windows SID Lookup Function Works

The Windows SID Lookup Function returns:

- The corresponding domain or account value.
- An error message when a SID cannot be resolved. 
- "Unsupported Platform" when used on a platform other than Windows.

## Configure the Windows SID Lookup Function

To configure this Function:

1. Add a Windows Event Logs source to begin generating data.
   See [Windows Event Logs](sources-windows-event-logs) for more information.

1. Add a new Pipeline or open an existing Pipeline. Add or capture sample data test your Pipeline as you build it. See [Pipelines](pipelines) for more information.

1. At the top of the Pipeline, select **Add Function** and search for `Windows SID Lookup`, then select it.

1. In the **Windows SID Lookup** section, configure the following general settings:

   - **Filter:** A JavaScript filter expression that selects which data to process through the Function. Defaults to `true`, meaning it evaluates all data events. If you don't want to use the default, you can use a JavaScript expression to apply this Function to specific data events. See [Build Custom Logic to Route and Process Your Data](filter-and-transform-data) for examples.
   - **Description:** A brief description of how this Function modifies your data to help other users understand its purpose later. Defaults to empty.
   - **Final:** When enabled, stops data from continuing to downstream Functions for additional processing. Default: Off. Toggle **Final** on if Fold Keys is the last Function in your Pipeline.
   - **Lookup fields**: Table of fields to place translated SID is. Select **Add Field** to add each field in a row with the following controls:
     - **Name**: Enter the name of the field to contain the translated SIDs.
     - **Value Expression**: Enter a JavaScript expression to extract the SID value(s) from the event field (typically `_raw`). Expressions can contain nested addressing. When you insert JavaScript template literals, strings intended to be used as values must be delimited with single quotes, double quotes, or backticks.
     - **Enabled**: Toggle this off to disable evaluating individual expressions, while retaining their configuration in the table. Useful for iterative development and debugging.

1. Test that you configured the Function correctly by comparing sample incoming data with outgoing data in the Data Preview pane and Pipeline Diagnostics tool. See [Data Preview](data-preview) for more information.
