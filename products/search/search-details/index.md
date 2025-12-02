# Search Details


Identify problems with your query by looking up the details of the search.

---

## Details Modal {#details}

To troubleshoot issues, you can view the search's details, pipeline plan, logs, metrics, and more. At the bottom right
of the query box, select **Details**.


![Search details](search-details-button.png)
{scale="30%"}

The resulting **Details** modal (shown below) displays the following:

- **id**: Unique identifier of the search. You can use it to, for example,
  [reuse search results](search-results#reuse). To quickly copy the ID, hover over the **Search Job** panel at the top,
  and select the **Copy to clipboard** icon.
- **status**: Search status - `new`, `running`, `completed`, `failed`, or `canceled`.
- **user**: User that ran the search.
- **query**: Search query string. If the query contains [Macros](macros), you can see what they resolved to, by
  selecting [**Expand Macros**](macros#expand-macros-used-in-a-query).
- **earliest**: Beginning of query time range, in a relative time format or milliseconds.
- **latest**: End of query time range, in a relative time format or milliseconds.
- **timeCreated**: Time when the search was created.
- **timeStarted**: Time when the search started running.
- **timeCompleted**: Time when the search was completed.
- **timeElapsed**: Total time the search ran.
- **timeInQueue**: How much time the search spent waiting in the queue.
- **sampleRate**: Ratio to reduce results, see [sampling](build-a-search#sampling).
- **set options**: Any [`set`](set)-statement options affecting the search, like
  [`maxResultsPerSearch`](set#max_results_per_search) or [`allow_previous_results`](set#allow_previous_results).

You can also select the following:

- **Search the Results**, to quickly [query the result set](search-results#search-the-results) of the search.
- **Rerun**, to [run](search-results#rerun) a new search that uses exactly the same query text and settings.
- **View Results**, to [display the cached results](search-results#view) of the search.

### Diagnostics

The **Diagnostics** drop-down provides options to download a compressed folder with many backend logs, configuration
files, and optionally, your search results.


![Diagnostics drop-down](search-diagnostics-button.png)
{scale="100%"}

Select **Exclude Results** to remove the search results from the downloaded diagnostics folder.

If you're sending the diagnostics folder to someone, like Cribl Support, we recommend selecting this option to remove
the results from the folder. This will keep the file size manageable.

For more about sending diagnostics to Cribl Support, see: [Share Diagnostics](diagnosing-search).

## Search Plan

The **Search plan** tab shows the backend processes your search ran. Your query was converted to a set of pipelines that
work on the data. Pipelines are broken into the following categories:

- **Federated**: Pipelines executed by the remote end.
- **Coordinated**: Pipelines executed by the coordinator process.
- **Combined**: Combined **Federated** and **Coordinated** view.

Typically, the first function you'll see in the **Federated** pipeline is **Drop**. This is a filtering function, which
drops data that does **not** match its **Filter** expression.

If your search consists of multiple stages, you can view the pipelines for each stage separately. To see a specific
stage, select its ID on the left (for example, **root**).

### Export a Search Plan

You can export the plan of a search as a JSON file:

1. [Run a search](build-a-search).
1. Once the search starts running, select **Details**, then **Search Plan**.
1. If your search consists of multiple stages, select the ID of the stage whose plan you want to export (for example,
   **root**).
1. At the bottom left, select **View as JSON** to see the search plan in JSON format.
1. At the top right, select **Export**. The search plan is downloaded as a JSON file.

## Logs

Cribl Search creates log events of your search. You'll see informational and debugging level entries with details on
every process run for your search.

Logs are separated into either **Coordinated** or **Executors** types:

- **Coordinated**: Organize query execution and do post-processing, for example, merging, sorting, aggregation, and
  persisting the search results.
- **Executors**: Scan data, for example, reading from S3, decompressing, filtering, and projection.

### Find Logs

Select inside of the pane with the logs and then press Control+F (Windows) or Command+F (Mac) to open the **Find** bar.

You can search by plain text and have three advanced search options:

- Match Case
- Match Whole Word
- Regular Expression

By default, searches run against all logs. To search against only specific text, highlight the desired text and click
the three horizontal lines icon.


![Find in certain logs](search-status-logs-find-lines.png)
{scale="100%"}

## Metrics

Metrics from your search are provided to give you insight into the search's performance and the amount of data it
touched.


![Search metrics](search-details-metrics.png)
{scale="100%"}

The top area provides a high-level summary of the stats for the search:

- **Time elapsed**: How much time the search took.
- **CPU\*s**: Sum total of CPU seconds spent on the search.
- **Scanned**: Total volume of data ingested by [executors](#executors) and [coordinators](#coordinators), calculated
  after decompression.
- **Events returned**: Number of events the search returned.
- **Executors**: Number of workers the search was split across.

### Coordinators

The **Coordinators** table displays a summary of the work done by the coordinator locally. If no Datasets were processed
by the coordinator, **Bytes In**, **Bytes Out**, and **Events Out** stats will be **N/A**.

### Executors

The **Executors** table displays metrics of the work done by the federated executors, reported on a per-executor basis.
