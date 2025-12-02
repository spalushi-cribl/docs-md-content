# Manage Lake Datasets


Create, edit, partition, datatype, and delete Lake Datasets.

---

This topic focuses on Lake Datasets that you populate from Cribl Stream using the [Cribl Lake Destination](destinations-cribl-lake). However, for selected types of data, you have the option to create and populate a Lake Dataset using Cribl Lake [Direct Access](direct-access).

You can also [send Cribl Search results to Cribl Lake](/search/set-up-cribl-lake#send/) by using the Search [`export`](/search/export/) operator. Data ingested from Search to Lake will typically have field names and values pre-parsed, and this parsing is a prerequisite to achieving a [Lakehouse](lakehouse) performance boost.

You can maintain up to 200 Datasets per Cribl Lake instance (meaning, per [Workspace](/stream/workspaces/)).

## Create a New Lake Dataset {#create}

To create a new Lake Dataset:

1. In the Cribl Lake sidebar, select **Datasets**. 
1. Select **Add Dataset** above the Dataset table.
1. Enter an **ID** for the new Dataset. 

   > The identifier can contain letters (lowercase or uppercase), numbers, and underscores, up to 512 characters.

   > You can't change the identifier after a Dataset is created,
   so make sure it is meaningful and relevant to the data you plan to store in it.

   > The identifier must be unique and must not match any existing [Cribl Search Datasets](/search/basic-concepts#datasets). Duplicate identifiers can cause problems when searching Cribl Lake data.
   {.box .warning}

1. Optionally, enter a **Description** of the Dataset's purpose.

1. Select a **Storage Location**. The internal `Cribl Lake` storage option is always available on this drop-down, and is the default location if you've configured no [external Storage Locations](byos). (Do not select an external Storage Location for a Dataset that you will populate via [Direct Access](direct-access).)

1. Define the **Retention period**. Cribl Lake will store data in the Dataset for the time duration you enter here. The allowed range is anywhere from 1 day to 10 years.

1. Select the **Data format**. `JSON` is the default. `Parquet` is a columnar storage format ideal for big data analytics (for tips on searching Parquet data, see [Parquet](/search/data-formats#parquet) in the Cribl Search docs). `DDSS` is available for a Dataset that you will populate via [Splunk Cloud Self Storage](splunk-cloud). The **HTTP Ingestion Enabled** check box will be automatically selected when you configure [Direct Access (HTTP)](direct-access-http#creating) to populate a Dataset.

1. Optionally, select the [**Lakehouse**](lakehouse) you want to link to the Dataset.
   When you select a Lakehouse at this point, the Lake Dataset will become a [mirrored Dataset](lakehouse#mirrored). 

   > You cannot assign a Lakehouse to a Dataset populated via [Splunk Cloud Direct Access](splunk-cloud), nor to a Dataset that uses an external [**Storage Location**](byos).
   {.box .warning}

1. Optionally, in **Advanced Settings**, select up to three [**Lake Partitions**](#lake-partitions) for the Dataset.

1. If you are linking the Dataset to a Lakehouse, then in **Advanced Settings**, you have the option to select up to five [**Lakehouse Indexed fields**](#lake-partitions).

1. Confirm with **Save**.

![Adding Lake Datasets with the form filled in](adding-lake-dataset-4.11.0.png)
{scale="85%" caption="Fill in a few fields and set the retention period, and you have a new Lake Dataset"}

You will now be able to select this new Dataset by using its identifier in a [Cribl Lake Destination](destinations-cribl-lake) or [Cribl Lake Collector](collectors-cribl-lake).

> Only [Organization Owners or Admins](/search/cloud-managing#member-roles) can add (above) or modify (below) Lake Datasets.
{.box .info}

## Edit a Lake Dataset {#edit}

To edit an existing Lake Dataset:

1. In the Cribl Lake sidebar, select **Datasets**. 
1. Select the Dataset you want to edit and change the desired information,
   including description, [Storage Location](byos), [retention period](datasets#retention), [partitions](#lake-partitions),
   and assigned [Lakehouse](lakehouse).
   You can't modify the identifier of an existing Lake Dataset.
1. Confirm with **Save**.

### Change a Lake Dataset's Retention Period {#retention}

If you change the retention period of an existing Lake Dataset, this affects all data in the Dataset.

If you increase the retention period, data **currently** in the Dataset will adopt the new retention period.

> If you **decrease** the retention period, data older than the new time window will be lost. Make sure that this is your intention before you save the change.
{.box .warning}

## Delete a Lake Dataset {#delete}

You can delete a Lake Dataset either from the Dataset table, or from an individual Dataset's page.

To delete a Lake Dataset from the Dataset table:

1. In the Cribl Lake sidebar, select **Datasets**. 
1. Select the check box next to the Dataset(s) you want to delete.
1. Select **Delete Selected Datasets**.

Alternatively, select the Dataset's row in the Dataset table, and then select **Delete**.

### Scheduled Deletion {#scheduled}

A Lake Dataset is not deleted instantly. Instead, after you select it for deletion, it will be marked with `Deletion in progress`. In this state, you can no longer edit it, or connect it to Collectors or Destinations.

Data that is older than 24 hours will be removed at midnight UTC the following day. More-recent data might be deleted after that time. Once a Lake Dataset is marked for deletion, you are no longer charged for any data that it contains.

> You cannot delete built-in Lake Datasets, or Lake Datasets that have any connected Collectors or Destinations.
{.box .info}

## Lake Partitions and Lakehouse-Indexed Fields {#lake-partitions}

Each Lake Dataset can have up to three partitions configured, and, if you're
using a Lakehouse, up to five Lakehouse-indexed fields, to speed up searching and replaying data from Cribl Lake.

A typical use case for applying partitions and indexed fields is to accelerate `hostname` or `sourcetype`
if you are receiving data from multiple hosts or sources.

You can use partitions and indexed fields both in [Search queries](search-cribl-lake#lake-partition)
and in the filter for runs of the [Cribl Lake Collector](collectors-cribl-lake).

### Which Fields to Use as Partitions and Indexed Fields? {#which-fields}

To ensure that search and replay work best, make sure you provide broader partitions or Lakehouse-indexed fields first.
The order in which you configure partitions or indexed fields for a Lake Dataset
influences the speed of search and replay of the data.

We also do not recommend partitions or indexed fields that:

- Contain PII (personally identifiable information).
- Contain objects.
- Have a name starting with an underscore (such as `_raw`).

For partitions, additionally avoid using fields that:

- Have high cardinality values.
- Are nullable (that is, can have the value of `null`).

> Don't use `source` or `dataset` as partitions or Lakehouse-indexed fields.
> These are internal fields reserved for Cribl, and are not supported as partitions/indexed fields.
{.box .warning}

## Datatypes for Lake Datasets {#datatypes}

Lake Datasets, like regular Cribl Search Datasets, can be associated with [Datatypes](/search/datatypes/).
Datatypes help separate Dataset data into discrete events, timestamp them, and parse them as needed.

To configure Datatypes for a Lake Dataset:

1. On the top bar, select **Products**, and then select **Search**.
1. In the sidebar, select **Data**.
1. In the list of Datasets select your Lake Dataset.
1. Select the **Processing** tab.
1. In the **Datatypes** list, add desired Rulesets via the **Add Datatype Ruleset** button.
   (The order of Rulesets matters.)
1. When the list of Rulesets is complete, confirm with **Save**.

> For more information about using Datatypes and Rulesets in searches, see [Cribl Search Datatypes](/search/datatypes/).
{.box .success}
