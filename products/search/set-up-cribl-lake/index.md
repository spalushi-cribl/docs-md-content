# Connect Cribl Search to Cribl Lake


Configure Cribl Search to query your Cribl Lake or Lakehouse data.

---

[Cribl Lake Datasets](/lake/datasets/) work as Cribl Search Datasets out-of-the-box, so you can instantly [start searching](search-cribl-lake) them.

You can assign Lake Datasets to [Lakehouses](/lake/lakehouse/), for dramatically faster search responses and more-predictable billing.

Where you need direct ownership of your data, for compliance or other purposes, you can use [Storage Locations](/lake/byos/) to create Lake Datasets directly on Amazon S3. These Datasets use the Amazon S3 Standard storage class.

For more information on Lake and Lakehouses, see the [Cribl Lake docs](/lake/).

## Add a Cribl Lake Dataset Provider {#provider}

Cribl Search comes with a preconfigured Dataset Provider for Cribl Lake (called `cribl_lake`), so you don't need to add
it explicitly.

You can see it by going to **Products** (on the top bar) > **Search** > **Data** > **Dataset Providers**.


![See? It's already there!](search-lake-provider.png)
{scale="100%"}

## Add a Cribl Lake Dataset {#add-lake}

You add Lake Datasets in the Cribl Lake UI. To find out how, see the Cribl Lake docs:
[Create a New Dataset](/lake/managing-datasets#create-a-new-dataset).

## Send Cribl Search Results to Cribl Lake {#send}

After you [create a Dataset in Cribl lake](/lake/managing-datasets#create-a-new-dataset/), you can send Cribl Search
results to it, using the [`export`](export) operator. Here is a simple example:

```kusto
// export Cribl Search results to an existing Lake Dataset
dataset="cribl_search_sample"
| export to myLakeDataset
```

 (You can find more `export` examples [here](export#examples).)


> Typically, you're exporting data in which Cribl Search has already parsed field names and values. This makes the data compatible with [Lakehouses](/lake/lakehouse/). However, if you happen to send **unparsed** data, you won't be able to search it at Lakehouse speed.
{.box .info}

## Search a Cribl Lake Dataset {#search-lake}

You can [query](/search/about#action) your Lake Datasets just like any other Search Datasets. See
[Searching Cribl Lake](search-cribl-lake) for some query examples.

## Search a Lakehouse {#search-lakehouse}

Searching a Lake Dataset that's assigned to a [Lakehouse](/lake/lakehouse) is
significantly faster.


> To create a new Lakehouse, see the Cribl Lake docs: [Add a New Lakehouse](/lake/lakehouse#add-a-new-lakehouse).
{.box .success}

See how to:

- [Find Out if a Dataset is Assigned to a Lakehouse](#find-lakehouse)
- [Verify Lakehouse Use](#verify-lakehouse)
- [Search Multiple Lakehouse Datasets](#multi-lakehouse)
- [Disable Lakehouse Use](#disable-lakehouse-use)

See also: [Cribl Search Differences with Lakehouse](/search/lakehouse-differences).

### Find Out if a Dataset is Assigned to a Lakehouse {#find-lakehouse}

To see if a Dataset you're about to search is assigned to a Lakehouse or not, look for the
![Lakehouse](search-lakehouse.png) icon. For example, look at the list of available datasets in your **Search
Home**:


![A Dataset assigned to a Lakehouse](search-lakehouse-dataset.png)
{scale="100%"}

You can also go to **Products** (on the top bar) > **Search** > **Data** > **Datasets** and look at the **Lakehouse**
column.

### Verify Lakehouse Use {#verify-lakehouse}

To find out whether a search successfully used a Lakehouse, take a look at the
[tracking bar](search-overview#track-the-progress-of-your-search).

If the search failed to use a Lakehouse, the bar presents information about potential reasons.

### Search Multiple Lakehouse Datasets {#multi-lakehouse}

You can run a single query against multiple Lakehouse-assigned Datasets. For the query to execute at Lakehouse speed,
all Datasets in the query must be Lakehouse-assigned, and your query must also meet one of these conditions:

- Include only operators from the following group: [`cribl`](cribl), [`centralize`](centralize), [`extend`](extend),
  [`extract`](extract), [`foldkeys`](foldkeys), [`limit`](limit), [`mv-expand`](mv-expand), [`pivot`](pivot),
  [`project`](project), [`project-away`](project-away), [`project-rename`](project-rename), [`search`](search), and
  [`where`](where).

- Or, if you use any other operators, insert the [`centralize`](centralize) or [`limit`](limit) operator before them.
  (Result counts will be further constrained by [usage group](usage-groups#usage-limits) limits.)

If neither of the above conditions is met, or if your query includes non-Lakehouse Datasets, the query will run at
standard speed.

### Disable Lakehouse Use

You can use a `set` statement with the [`lakehouse`](set#lakehouse) option
to control whether a query can use a Lakehouse or not.

For example, to test how a Dataset assigned to a Lakehouse performs without it:

```kusto
set lakehouse="off" dataset="lakehouse_dataset"
```

### Cribl Search Differences with Lakehouse {#search-diffs}

Executing Cribl Search queries against a Lakehouse-assigned Dataset changes some behavior and results, compared to
executing the same queries without Lakehouse caching. For details, see
[Lakehouse Search Differences](/search/lakehouse-differences).
