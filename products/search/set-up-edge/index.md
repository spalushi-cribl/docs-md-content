# Connect Cribl Search to Cribl Edge


Configure Cribl Search to query your Cribl Edge Nodes.

---

With [Cribl Edge](/edge/), you can collect logs, metrics, application data, etc., and send them to Cribl Stream or any other supported destination in real time.

If you already have Edge Nodes running you don't need to set up anything and can start [searching](build-a-search). For
example searches, see [Searching Cribl Edge](search-edge). To search data not specified in a built-in Dataset, you can add a
custom [Dataset](#datasets) pointing to anywhere in the filesystem that Edge can read.

In this guide, you'll add a Cribl Edge Node on an Amazon EC2 instance to search its internal logs and metrics. For an
in-depth installation guide of Cribl Edge see the [Cribl Edge Deployment Guide](/edge/deploy-planning).

## Cribl Edge Access from Cribl Search {#access}

Cribl Search users can search the Edge Nodes to which they have access via Cribl Edge
[Permissions](/edge/permissions#product/) or [Roles](/edge/roles#edge-roles/). Cribl Search
[Admins and Editors](cloud-managing#search-member-roles) can create Datasets pointing to any Node (for example, using
wildcards in the [path](#path)). However, at search execution time, Cribl Search queries – and returns results from –
only the Nodes to which the querying user has access.

For details on how to manage access to Edge Nodes, see the Cribl Edge docs:

- [Access Management](/edge/access-management/)
- [Product-Level Permissions (Stream and Edge)](/edge/permissions#product/)
- [Group- and Fleet-Level Permissions](/edge/permissions#group/)

## Configure Cribl Edge {#edge}

To add a new Edge Node, follow the instructions in [Add and Update Edge Nodes](/edge/edge-nodes-add-update#bootstrap) to add or
bootstrap a new Node.

## Configure EC2 Instance

We'll launch an Ubuntu instance using the AWS Management Console. For an in-depth tutorial, see
[how to get started with Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html).

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
1. From the EC2 console dashboard, in the **Launch instance** box, select **Launch instance**. From the options that appear, select **Launch
   instance** again.
1. Under **Name and tags**, for **Name**, enter a descriptive name for your instance.
1. Under **Application and OS Images (Amazon Machine Image)**, select **Quick Start**, and then select **Ubuntu**. This
   is the operating system (OS) for your instance.
1. Under **Key pair (login)**, for **Key pair name**, select or create a key pair.
1. Keep the default selections for the other configuration settings for your instance.
1. Select **Launch instance**.
1. A confirmation page lets you know that your instance is launching. Select **View all instances** to close the
   confirmation page and return to the console.
1. On the **Instances** screen, you can view the status of the launch. It takes a short time for an instance to launch.
   When you launch an instance, its initial state is pending. After the instance starts, its state changes to running
   and it receives a public DNS name.
1. It can take a few minutes for the instance to be ready for you to connect to it. Check that your instance has passed
   its status checks; you can view this information in the **Status check** column.
1. Now you'll connect to the instance as the root user using the Amazon EC2 console. Open the Amazon EC2 console at
   https://console.aws.amazon.com/ec2/. See how to
   [connect using EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-connect-methods.html)
   for more details.
1. In the navigation pane, select **Instances**, then the instance, then **Connect**.
1. Select the **EC2 Instance Connect** tab and verify the user name, then select **Connect** to open a terminal window.
1. In the terminal, to gain root access, run the command `sudo su`.
1. In the terminal as root, paste the bootstrap script you copied from Cribl Edge in the [above section](#edge).
1. This will install the Edge Node in your EC2 instance and you can navigate back to Cribl Edge's **Edge Nodes** tab to
   view the new Node.

## Built-In Cribl Edge Datasets {#datasets}

Cribl Search comes with several Cribl Edge Datasets, detailed in [Built-In Cribl Edge Datasets](edge-datasets). Each Dataset tells Cribl Search what data to search.

You do not need to make any changes to these Datasets, and you can start searching them immediately. However, you can edit these built-in Datasets,
or add new ones to specify other logs anywhere in the filesystem that Edge can read. 

To access Cribl Edge Datasets, select **Data** from the sidebar, then look for them on the default **Datasets** tab. The following sections explain the configurations available on a Dataset.

### Path {#path}

The **Path** specifies the data that the Dataset consists of. It defines the scope of data, to narrow down what data is
in the Dataset. This is a JavaScript expression that supports environment variables, wildcards, and tokens. For example,

- `$CRIBL_STATE_DIR/system_metrics`
- `/foo/B*/*/baz`
- `/foo/${country}/${state}/${city}/` – where `country`, `state`, and `city` now become fields for all events of that
  Dataset.




#### Basic Tokens

Basic tokens' syntax follows that of [JS template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals): `${token_name}` – where `token_name` is the field (name) of interest. 

For example, if the path was set to `/var/log/${hostname}/${dataSource}/`, you could use a filter such as `hostname=='myHost' && dataSource=='mydataSource'` to specify data only from the `/var/log/myHost/mydataSource/` subdirectory.




#### Time-Based Tokens

Paths with time notation can be referenced with tokens, having a direct effect on the earliest and latest boundaries. The supported time fields are:
  * `_time` is the raw event's timestamp.
  * `__earliest` is the search start time.
  * `__latest` is the search end time.

Time-based tokens are processed as follows:
  * For each path, times must be notated in descending order. So Year/Month/Day order is supported, but Day/Month/Year is not.
  * Paths may contain more than one time component. For example, `/my/path/2020-04/20/`.
  * In a given path, each time component can be used only once. So `/my/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` is a valid expression format, but `/my/path/${_time:%Y}/${_time:%m}/${host}/${_time:%Y}/...` (with a repeated `Y`) is not supported.
  * For each path, all extracted dates/times are considered in UTC.
 
> Cribl recommends that your path always include and tokenize the largest available time fields, proceeding down to the smallest desired time fields. Otherwise, your searches might yield unexpected results, because Cribl Search will default the omitted fields to their earliest allowed value.
{.box .info}

The following `strptime` format components are allowed: 
  * `Y`, `y` for years
  * `m`, `B`, `b`, `e` for months
  * `d`, `j` for days 
  * `H`, `I` for hours
  * `M` for minutes
  * `S` for seconds
  * `s` for Unix-style Epoch times (seconds since 1/1/1970)

Time-based token syntax follows that of a slightly modified [JS template literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals):
`${_time: some_strptime_format_component}`. Examples:

Path | Matches
--- | ---
`/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` | `/path/2020/04/20/...`
`/path/${_time:year=%Y}/${_time:month=%m}/${_time:day=%d}/...` | `/path/year=2020/month=05/day=20/...`
`/path/${_time:%Y-%m-%d}/...` | `/path/2020-05-20/...`

### Path Filter {#filter}

This is a JavaScript filter expression that is evaluated against the provided Dataset path. The **Path Filter** value
defaults to `true`, which matches all data, but this value can be customized almost arbitrarily.

For example, if a Dataset has this **Path Filter**:

`source.endsWith('.log') || source.endsWith('.txt')`

...then only files/objects with `.log` or `.txt` extensions will be searched.

At the **Path Filter** field's right edge are a Copy button and an Expand button that opens a validation modal.

With DDSS or SmartStore partitioning, you can provide the index name as **Path Filter**, for example:
`index=="index_y"`.

### Partitioning Scheme

**Advanced Settings** > **Partitioning scheme** lets you select the scheme for partitioning data incoming from Splunk.

You can choose between [`Splunk DDSS`](https://docs.splunk.com/Documentation/SplunkCloud/9.1.2308/Admin/DataSelfStorage)
and [`Splunk SmartStore`](https://docs.splunk.com/Documentation/Splunk/9.1.2/Indexer/AboutSmartStore).

In both cases, provide the path where indices are stored (parent folder of your index) as **Bucket path**.


> For Splunk DDSS, the full path takes the form:
> 
> `<parent_folder>/<indexName>/db/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`
>
> or
>
> `<parent_folder>/<indexName>/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`
>
> For SmartStore, it is:
> 
> `<parent_folder>/<indexName>/db/<2-letter-hash>/<2-letter-hash>/<bucket_id_number-origin_guid>/<"guidSplunk"-uploader_guid>/`
> 
> In both cases, if your bucket is organized using this default file path, Cribl Search will automatically discover its content.
{.box .success}

### Processing

**Processing** is done with [**Datatypes**](datatypes), to break data into discrete events and define fields so they're
ready to search. These consist of rules that are applied to the data searched in your Dataset. See
[Datatypes](datatypes) for details.

## Search Cribl Edge {#searching}

Now that you have an Edge Node on an Amazon EC2 instance, you're ready to start [searching Cribl Edge](search-edge).

