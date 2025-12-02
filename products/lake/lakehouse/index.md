# Lakehouses in Cribl Lake


Search Cribl Lake faster with a Lakehouse.

---

To speed up searching data from Cribl Lake, you can assign individual Lake
Datasets to a **Lakehouse**.

Lakehouses are a parallel option to regular Lake Datasets. When searching a Lakehouse, Cribl Search returns results with much shorter latency than when searching regular Lake Datasets.

When you assign a Dataset to a Lakehouse, data ingested into the Dataset is
simultaneously sent to a cache storage system, which allows for quicker
retrieval. Data sent to the Lakehouse ages out at the same time as data in the
Lake Dataset.

> For examples of how Lakehouses can enable efficient storage and fast search of high-volume recent telemetry data, see: [Streamline Logs Storage and Analysis with a Lakehouse](lakehouse-logs).
{.box .success}

## Lakehouse Availability {#availability}

Lakehouses are available only in Cribl.Cloud Organizations on certain [plans](https://cribl.io/pricing/lake).

## Lakehouse Sizes {#sizes}

Each Lakehouse has a size. The size defines the amount of data that can be
ingested into and stored in the Lakehouse. When sizing your Lakehouse, consider
your expected data ingest for Lake Datasets associated with that Lakehouse. Then,
select a size based on this estimate. (An undersized Lakehouse will be able to
cache fewer days of data.)

The following sizes are available:

| Size     | Capacity (per day)      |
| -------- | -------------------------- |
| Small    |  600 GB |
| Medium   | 1200 GB                    |
| Large    | 2400 GB                    |
| XLarge   | 4800 GB                    |
| XXLarge  | 9600 GB                    |
| 3XLarge ([Contact Support](/support/))| 14 TB|
| 6XLarge ([Contact Support](/support/))| 28 TB|

### Exceeding Lakehouse Capacity {#exceeding}

When data flowing into a Lakehouse exceeds the Lakehouse capacity, this will degrade both ingest speed and search speed. To prevent these impacts, try to avoid sustained excessive loads. If they occur, consider expanding the Lakehouse capacity, as covered in the next section.



### Resize a Lakehouse {#resize}

After creating a Lakehouse, if you decide you need less or more storage capacity, you can resize an existing Lakehouse.

To do so, on the **Lakehouses** page, select the existing Lakehouse and choose a different size.

When you resize a Lakehouse, reserve some time for its resouces to be
reprovisioned. During this period, Lakehouse-cached searching will be
unavailable. So any search against Lake Datasets connected to the Lakehouse will
proceed at the speed of a regular search. (Cribl Search will also consume
credits for CPU time, because these searches are not covered by Lakehouse flat
billing.) After the resize operation is complete, your Lakehouse-cached data
will be searchable again.

### Lakehouse Retention {#retention}

Lakehouse retention is determined by the [retention period set for the linked Lake Dataset](datasets#retention). Data
remains in the Lakehouse cache for as long as it is retained in the associated Lake Dataset. When data ages out of the
Lake Dataset, it is also removed from the Lakehouse.

## Add a New Lakehouse {#add}

A Lake Dataset can be assigned to only one Lakehouse.
However, a Lakehouse can have more than one Lake Dataset assigned to it.

To add a new Lakehouse, as an Organization Owner:

1. In the Cribl Lake sidebar, select **Lakehouses**.
1. Select **Add Lakehouse**.
1. Enter an **ID** for the new Lakehouse and, optionally, a **Description**.
1. Define the [**Size**](#lakehouse-sizes) of the Lakehouse, based on expected ingest volume. 
    - You can change the size later.
    - There are two larger Lakehouse sizes: 3XLarge and 6XLarge. To use one
      of these sizes, [Contact Support](/support/).

The new Lakehouse will be fully active after it has been provisioned, which might take up to an hour.


![New Lakehouse page with ID, Description, and Lakehouse Size to select.](lakehouse-add-4.10.png)
{scale="90%" caption="Select an initial size for the new Lakehouse"}

## Link a Lake Dataset to a Lakehouse {#link}

You can link one or more Lake Datasets to each Lakehouse.
To do so, you need to be an [Organization Admin](/stream/permissions#organization) or [Lake Admin](lake-permissions).

Linking Datasets is possible only after the Lakehouse has been fully provisioned.

You can link Datasets in two ways, influencing how the Dataset will behave in relation to the Lakehouse:

- [Link an already existing Lake Dataset](#existing), to only cache data ingested into the Dataset in the future.
- [Link a new Lake Dataset](#new), to mirror the Dataset's contents.

### Link an Existing Lake Dataset to a Lakehouse {#existing}

To link an existing Lake Dataset to a Lakehouse:

1. In the sidebar, select **Datasets**.
1. On the resulting **Datasets** page, select the Lake Dataset you want to assign to a Lakehouse.
1. In the **Lakehouse** drop-down, select the Lakehouse you want to assign the Dataset to.

![Datasets page with Lakehouse drop-down.](lakehouse-link-dataset-4.10.png)
{scale="90%" caption="Link Lakehouses to Lake Datasets"}

When you assign an existing Lake Dataset to a Lakehouse, only data newly ingested to that Dataset will be sent to the Lakehouse.

> If you link an **empty** existing Lake Dataset to a Lakehouse,
> it will behave like a [mirrored Dataset](#mirrored).
{.box .info}

### Link a New Lake Dataset to a Lakehouse {#new}

To link a Lake Dataset to a Lakehouse during its creation:

1. In the sidebar, select **Datasets**.
1. Select **Add Dataset** above the Dataset table.
1. Configure the Dataset as described in [Create a New Lake Dataset](managing-datasets#create-a-new-dataset).
1. In the **Lakehouse** dropdown, select the Lakehouse you want to link to.

Assigning a Lake Dataset to a Lakehouse during creation results in a mirrored Dataset.

#### Mirrored Lake Datasets {#mirrored}

When you assign a Lake Dataset to a Lakehouse **while creating** the Dataset,
the Lakehouse will mirror the Dataset's contents.

Data sent to a mirrored Dataset will be ingested into the Lakehouse regardless of the `event_time` of the event.
Cribl Search will always use the Lakehouse when searching a mirrored Dataset,
unless you explicitly turn it off by setting the [`lakehouse`](/search/set#lakehouse) option to `off`.

## Insert Data into a Lakehouse {#insert}

To populate a Lakehouse, see [Prepare Data for Use with Lakehouse](destinations-cribl-lake#prepare-data-for-use-with-lakehouse).

## Delete a Lakehouse {#delete}

To delete a Lakehouse:

1. In the sidebar, select **Lakehouses**.
1. Select the check box next to the Lakehouse(s) you want to delete.
1. Select **Delete Selected Lakehouses**.

A Lakehouse is not deleted instantly. Instead, once you select the Lakehouse for deletion, it will be marked with "Deletion in progress".
At this point, you can no longer edit the Lakehouse, nor connect it to Lake Datasets.

Data that is older than 24 hours will be removed at midnight UTC the following day, while more-recent data might be deleted after that time.
Once a Lakehouse is marked for deletion, you are no longer charged to maintain any data that it contains.

> When you remove a Lakehouse, its data will still be available to be searched and replayed from its standard Lake Dataset.
{.box .success}

## Lakehouse Status {#status}

A Lakehouse can have one of the following statuses:

| Status | Description |
| --- | --- |
| Ready | Lakehouse can be linked to Lake Datasets and used. You can resize it. |
| Provisioning | Lakehouse is being prepared, you cannot resize it or link it to Lake Datasets. |
| Delayed | Provisioning the Lakehouse has encountered an issue, but it will move to Ready status when the issue is resolved. <br><br>While the Lakehouse is Delayed, you can resize it, but you can't link it to Datasets. |
| Terminated | Lakehouse has been marked for [deletion](#delete-a-lakehouse). |

## Monitor Lakehouse Usage {#monitor}

To control your usage of Lakehouses, and to make decisions about their potential resizing, you can display charts that present data per Lakehouse.

Select the sidebar's **Monitoring** link. On the resulting page, use the drop-down at the upper right to select among Lakehouses. You'll see the following charts.

- **Events**: Number of events ingested into the Lakehouse.
- **Throughput**: Number of bytes ingested into the Lakehouse.
- **Queries per Minute**: Number of queries performed on the Lakehouse.
- **Replication Latency**: Latency of ingesting data into the Lakehouse.

![Lakehouse Monitoring page, with Events, Bytes, Queries Per Minute, and Replication Latency charts.](lakehouse-monitoring-4.10.png)
{scale="100%" caption="Charts to monitor performance per Lakehouse"}

On the **Lakehouses** page, you can see summary data for all configured Lakehouses. **Ingestion Rate** measures the past 24 hours, compared with each Lakehouse's configured capacity. 

**Max Throughput** graphically compares actual throughput to a guideline representing the recommended maximum, again based on the configured capacity.

![Lakehouses page, showing summary Size, Max Throughput, and Ingestion Rate.](lakehouses-page.png)
{scale="100%" caption="Summary data on Lakehouses page"}
