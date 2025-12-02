# Collector Sources



Unlike other Cribl Stream [Sources](sources), Collectors are designed to ingest data intermittently, rather than continuously. You can use Collectors to dispatch on‑demand (ad hoc) collection tasks, which fetch or "replay" (re-ingest) data from local or remote locations. 

Collectors also support **scheduled** periodic collection jobs – recurring tasks that can make batch collection of stored data more like continual processing of streaming data. You configure Collectors prior to, and independently from, your configuration of [ad hoc versus scheduled](collectors-schedule-run) collection runs.

Collectors are integral to Cribl Stream's larger story about optimizing your data throughput. Send full-fidelity log and metrics data ("everything") to low-cost storage, and then use Cribl Stream Collectors to selectively route ("replay") only needed data to your systems of analysis.



> ##### Collector Resources
>
> - Video introduction to [Data Collection](https://vimeo.com/457821834), in < 2 minutes.
> - Video introduction to [Data Collection Scheduling](https://vimeo.com/459560875), in < 2 minutes.
> - Free, interactive try-out of Collectors in Cribl's [Data Collection & Replay](https://sandbox.cribl.io/course/data-collection) sandbox.
> - Using Collectors guides: [S3 Storage and Replay](/stream/usecase-replay-s3) | [REST API Collectors](/stream/usecase-rest) | [Microsoft Graph API Collection](/stream/usecase-rest-ms-graph) | [ServiceNow API Collection](/stream/usecase-rest-snow) | [Creating a Custom Collector](/stream/usecase-rest-create-collector).
>
{.box .success}

## Collector Types

Cribl Stream currently provides the following Collector options:

* [Azure Blob](collectors-azure-blob) – enables data collection and replay from Azure Blob Storage objects.
* [Database](collectors-database) – enables data collection from database management systems like MySQL and SQL Server.
* [File System/NFS](collectors-filesystem) – enables data collection and replay from local or remote filesystem locations. 
* [Google Cloud Storage](collectors-google-cloud-storage) – enables data collection and replay from Google Cloud Storage buckets.
* [Health Check](collectors-health-check) – monitors the availability of system endpoints.
* [REST/API Endpoint](collectors-rest) – enables data collection and replay via REST API calls. Provides four Discover options, to support progressively more complex (and dynamic) item enumerations.
* [S3](collectors-s3) – enables data collection and replay from Amazon S3 buckets or S3-compatible stores.
* [Script](collectors-script) – enables data collection and replay via custom scripts.
* [Splunk Search](collectors-splunk-search) – enables data collection and replay from Splunk queries. Supports both simple and complex queries, as well as real-time searches.

If you are exploring Collectors for the first time, the File System/NFS Collector is the simplest to configure, while the REST/API Collector offers the most complex configuration options.

## How Do Collectors Work

You can configure a Cribl Stream Node to retrieve data from a remote system by selecting **Manage** from the top nav, then a **Worker Group** to configure. Next, click **Data** > **Sources** > **Collectors**. Data collection is a multi-step process:

First, define a Collector instance. In this step, you configure **collector-specific settings** by selecting  a Collector type and pointing it at a specific target. (E.g., the target will be a directory if the type is File System, or an S3 bucket/path if the type is Amazon S3.)

Next, schedule or manually run the Collector. In this step, you configure either [scheduled-job–specific](collectors-schedule-run#schedule-config) or [run‑specific](collectors-schedule-run#run-config) settings – such as the run Mode (Preview, Discovery, or Full Run), the Filter expression to match the data against, the time range, etc.

When a Node receives this configuration, it prepares the infrastructure to execute a collection job. A collection job is typically made up of one or more tasks that: discover the data to be fetched; fetch data that match the run filter; and finally, pass the results either through the [Routes](routes) or (optionally) into a specific [Pipeline](pipelines) and [Destination](destinations).

> Select **Monitoring** > **System** > **Job Inspector** to see the results of recent collection runs. You can filter the display by Worker Group (in [distributed deployments](/stream/deploy-distributed)), and by run type and run timing.
>
{.box .info}

## Advanced Collector Configuration

To edit any Collector’s definition in a JSON text editor, click **Manage as JSON** at the bottom of the **New Collector** modal, or on the **Configure** tab when editing an existing Collector. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing Collector configurations as `.json` files.

## Scheduled Collection Jobs {#scheduled}

You might process data from inherently non-streaming sources, such as REST endpoints, blob stores, etc. [Scheduled jobs](collectors-schedule-run#schedule-config)  enable you to emulate a data stream by scraping data from these sources in batches, on a set interval. 

You can schedule a specific job to pick up new data from the source – data that hadn’t been picked up in previous invocations of this scheduled job. This essentially transforms a non-streaming data source into a streaming data source.

## Collectors in Distributed Deployments

In a [distributed deployment](/stream/deploy-distributed), you configure Collectors at the Worker Group level, and Worker Nodes execute the tasks. However, the Leader Node oversees the task distribution, and tries to maintain a fair balance across jobs. 

When Workers ask for tasks, the Leader will normally try to assign the next task from a job that has the least tasks in progress. This is known as "Least-In-Flight Scheduling," and it provides the fairest task distribution for most cases. If desired, you can change this default behavior by opening **Group Settings** > **General Settings** > **Limits** > **Jobs**, and then setting **Job dispatching** to **Round Robin**.

> More generally: In a distributed deployment, you configure Collectors and their jobs on individual Worker Groups. But because the Leader manages Collectors' state, if the Leader instance fails, Collection jobs will fail as well. (This is unlike other [Sources](sources), where Worker Groups can continue autonomously receiving incoming data if the Leader goes down.)
>
{.box .warning}

## Monitoring and Inspecting Collection Jobs {#inspect}

Select **Monitoring** > **System** > **Job Inspector** to view and manage pending, in-flight, and completed collection jobs and their tasks.

![Job Inspector](job-inspector.8a7a4f88c7.png)
{border="true"}

Here are the options available on the Job Inspector page:

- **All** vs. **Currently Scheduled** tabs: Click **Currently Scheduled** to see jobs forward-scheduled for future execution – including their cron schedule details, last execution, and next scheduled execution. Click **All** to see all jobs initiated in the past, regardless of completion status.

- Job categories (buttons): Select among **Ad-hoc**, **Scheduled**, **System**, and **Running**. (At this level, **Scheduled** means scheduled jobs already running or finished.)

- Group selectors: Select one or more check boxes to display the **Pause**, **Resume**, etc., buttons shown along the bottom.

- Sortable headers: Click any column to reverse its sort direction.

- Search bar: Click to filter displayed jobs by arbitrary strings.

- Action buttons: For finished jobs, the icons (from left to right) indicate: **Rerun**; **Keep job artifacts**; **Copy job artifacts**; **Delete job artifacts**; and **Display job logs** in a modal. For running jobs, the options (again from left to right) are: **Pause**; **Stop**; **Copy job artifacts**; **Delete job artifacts**; and **Live** (show collection status in a modal).


