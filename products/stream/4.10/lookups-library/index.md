# Lookups Library


## What Are Lookups

Lookups are data tables that you can use in Cribl Stream to enrich events as the [Lookup Function](lookup-function) processes them. To access the Lookups library, which provides a management interface for all lookups, navigate to **Products** > **Cribl Stream** > **Worker Groups**. Select a Worker Group, then go to **Processing** > **Knowledge** > **Lookups**. 

This library is searchable, and you can tag each lookup as necessary. There is full support for `.csv` files. The library supports compressed files, but they must be in gzip format (`.gz` extension). You can add files in multimedia database (`.mmdb`) binary format, but you cannot edit these binary files through the Cribl Stream UI.

![Lookups Library](cribl-lookups.png)
{border="true"}

## How Does the Library Work

In single-instance deployments, we store all files handled by the interface in `$CRIBL_HOME/data/lookups`. In [distributed deployments](/stream/deploying-configurations#configuration-and-lookup-file-locations), the storage path on the Leader Node is  `$CRIBL_HOME/groups/<groupname>/data/lookups/` for each Worker Group.

> For large and/or frequently replicated lookup files, you might want to bypass the Lookups Library UI and instead manually place the files in a different location. This can both reduce deploy traffic and prevent errors with the Cribl Stream default Git integration. For details, see [Managing Large Lookups](/stream/large-lookups).
>
{.box .info}

### Add Lookup Files

To upload or create a new lookup file, select **New Lookup File**, then select the appropriate option from the drop-down.

### Modify Lookup Files

To re-upload, expand, edit, or delete an existing `.csv` or `.gz` lookup file, select its row on the Lookups page. (No editing option is available for `.mmdb` files; you can only re-upload or delete these.)

In the resulting modal, you can edit files in **Table** or **Text** mode. However, we disable **Text** mode for files larger than 1 MB.

![Editing in **Table** mode](cribl-lookups-edit-table-mode.bad3bdbe5f.png)
{border="true"}

![Editing in **Text** mode](cribl-lookups-edit-text-mode.a2721e56f6.png)
{border="true"}

> ##### Edit Regex with Quotation Marks
>
> When a regex contains quotes, edit it in **Text** mode. Do not edit it **Table** mode, which can cause the regex to behave in unexpected ways.
>
{.box .warning}

## Memory Sizing for Large Lookups {#sizing}

You will need to provide extra memory for large lookup files, beyond what we describe in [Deployment Planning](deploy-planning). To determine how much extra memory to add to a Worker Node/Edge Node for a lookup, use this formula:

`Lookup file uncompressed size (MB) * 2.25 (to 2.75) *  numWorkerProcesses = Extra RAM required for lookup`

For example, if you have a lookup file that is 1 GB (1,000 MB) on disk, and three Worker Processes, you could use an average 2.50 as the multiplier:

`1,000 * 2.50 * 3 = 7,500`

In this case, the Node server or VM would need an extra 7,500 MB (7.5 GB) to accommodate the lookup file across all three Worker Processes.

###  What's with That Multiplier?  {#multiplier}

We cited a squishy range of 2.25–2.75 for the multiplier, because we found that it varies inversely with the number of columns in the lookup file: 

- The fewer columns, the higher the extra memory overhead (2.75 multiplier).
- The more columns, the lower the overhead (2.25 multiplier).

In Cribl testing: 

- 5 columns required a multiplier of 2.75
- 10 columns required a multiplier of only 2.25.

These are general (not exact) guidelines, and this multiplier depends only on the number of columns in the lookup table. The memory overhead imposed by each additional row appears to decline when more columns are present in the data.

### Maximum Table Size

Aside from the memory requirements above, the Node.js runtime imposes a hard limit on the size of lookup tables that Cribl Stream can handle. No table can contain more than 16,777,216 (that is, 2<sup>24</sup>) rows. If a lookup exceeds this size, attempting to load the lookup will log errors of the form: `failed to load function...Value undefined  out of range...`.

## Other Scenarios

See also:

- [Lookup Function](lookup-function).
- [Ingest-time Lookups](/stream/usecase-ingest-time-lookups) use case.
- [Managing Large Lookups](/stream/large-lookups) use case.
- [Redis Function](redis-function) for faster lookups using a Redis integration (bypasses the Lookups Library).
