# Using S3 Storage and Replay


Cribl Stream's Replay options offer organizations fundamentally new ways to manage data, by providing an easy way to selectively ingest, **and re‑ingest**, data into systems of analysis. Let's walk through how to use this feature, step by step.



For simplicity, we’ll treat the storage destination here as Amazon S3, although it could just as easily be MinIO, or any of several other options.

## Choosing JSON vs. Raw Format

You can write data out of Cribl Stream in either of two formats – JSON or raw.

#### JSON

With this option, the parsed event, with all metadata and modifications it contains at the time it reaches the Destination step, will be wrapped in a JSON object. Each event is one line. This is newline-delimited JSON (abbreviated NDJSON). For example, here is syslog data using the JSON format option:

![JSON-formatted event](iwe6OuR.c109fe8b15.png)

#### Raw

With this option, the contents of the event’s `_raw` field – unparsed, at the time it reaches the Destination step – are written out in plaintext. Each event is one line. For example, here's the same syslog data, except unmodified and unparsed, as written out with the raw option:

![Raw-formatted](M0462Eu.9d5826d218.png)

#### Which Format?

Cribl recommends using the default JSON format. Here, we expect that timestamp extractions and other vital enhancements have already been performed. Reusing that information makes sense, and will make your replay simpler. 

Notice that in the screen capture above, the **raw** format simply contains the original data. There might be cases where you want this; but usually, exporting the preprocessed event is more desirable.

> You cannot replay data that has been exported as Parquet. The S3 Collector does not support ingesting data in the Parquet format.
>
{.box .warning}

## Writing the Data Out

The Worker Nodes stage files until certain limits are reached: Time open, idle time, size, or number of files. These settings are available on the Destination configuration modal's **Advanced Settings** tab (see S3 details [here](destinations-s3#advanced-settings)). 

Once any of the configured limits is reached, the Worker gzips the file and drops it into the object store. If you reach the open-file limit, the oldest file will be targeted.

The S3 Destination's settings also allow you to define how the uploaded files are partitioned. Host, time, sourcetype, source – all the metadata is available to you for this purpose. When you’re replaying data from the store, these partitions will be handy, to make your replay searches faster. 

We can map segments of the path back to variables (including `time`) that you can use to zero in on the exact logs you need to replay, without requiring checking `_raw`. 

> For examples of the expressions Cribl recommends, see the example [Destination definition](#destination) below.
>
{.box .success}

## Retrieving the Events

With events stored in an object store, we can now point Cribl Stream to that store to Replay selected events back through the system. We want to use data in the file path as an initial level of filtering, to exclude as much data as we can from download. Object retrieval and unpacking imposes a big resource hit in the Replay process, so minimize your impact radius. Searching against `_raw` data is also possible, but should be secondary to `_time`, `sourcetype`, `index`, `host`, etc.

Retrieval details: The Leader picks one Node to do the discovery exploration, to find the potential objects that are in play. That list of targets is then doled out to the Worker Group to actually pull down the objects, and to examine them for content matches, before executing final delivery. All Worker Nodes share the workload of retrieving and re-injecting the data.

Finally, we need to process the data coming back to extract each event. As with any incoming data stream on a compatible Source, Cribl Stream can use default, or custom, Event Breaker definitions. In this case, we recommended above to use JSON as the format of the events when we write to disk, so we’ll use the **Cribl** Event Breaker ruleset on this Source. This Event Breaker contains a newline-delimited-JSON definition.

##  Setting Up and Running Replay  {#run}

With the above concepts established, let's put them to work. We'll round-trip data through our Destination and replay it in Cribl Stream.

### The Store

Object storage, or any shared storage, will work. As long as all the Cribl Stream Nodes can see it, we’re ready to go. For the purposes of this post, let’s stick with an S3-compatible store.

You’ll need all the credentials, keys, secret keys, endpoints, etc., required to access the bucket that you intend to use in your object store. (For details on cross-account access, see [this blog post](https://cribl.io/blog/manage-data-collection-across-aws-accounts/).) Obviously, the bucket should be able to grow to the size intended for your long-term archival needs.

#### Storage Class Compatibility

> Cribl Stream's Collectors and Replay feature do not support the S3 Glacier, S3 Deep Glacier, or Azure archive tier, due to long retrieval times for those storage classes.
> 
> Collectors and Replay **do** support S3 Glacier Instant Retrieval when you're using the S3 [Intelligent‑Tiering](https://aws.amazon.com/s3/storage-classes/intelligent-tiering/) storage class.
>
{.box .warning}

###  The Destination Definition  {#destination}

In your Cribl Stream Worker Group config, create a new [S3 Destination](destinations-s3). The screenshot below is an example built for this demo.

Use the Destination's **Advanced Settings** tab to adjust the limits, if needed. The defaults are fine for most situations, but depending on your partitioning scheme, we could be talking about 100+ files. So **make sure your staging area has enough space**. 

In particular, set the **Max open files** option appropriately. It overrides the size and time limits. We recommend that the staging area be its own volume, so that you don’t fill up a more-vital volume by mistake.

![Partitioning and filename expressions](MkPKni3.7e536811ba.png)

The screen capture above includes the partitioning and filename prefix expressions. Below is the full text of each expression. In a nutshell, we’re using time and other metadata to construct a path in the object store, which will be useful to us at replay time:

```shell
Year/Month/Day/index/host/sourcetype/HHMM-foobar.gz
```

Partitioning (one line):

```js
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d')}/${index ? index : 'no_index'}/${host ? host : 'no_host'}/${sourcetype ? sourcetype : 'no_sourcetype'}`
```

Filename:

```js
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%H%M')}`
```

We’ve also included a Key Prefix from Cribl Stream's **Processing** > **Knowledge** > **Global Variables**. You could use this to partition your logs by environment, or any other qualifier.

### The Archival Route

Create a new Cribl Stream Route called `Archival` that matches everything you want to archive. In this case, we've set it to `!__replayed`, and set this internal field in the Source (see the next step). Any event that is **not** coming from the defined Replay process will be archived. 

Select the `passthru` Pipeline, or create an empty Pipeline and use that. Select the S3 Destination you created above. And finally, make sure **Final** is **not set**. We want data to flow through this Route, not stop at it. So, position the `Archival` Route at or near the top of the Routing table. Save your work, commit, and deploy.

![Pipeline configuration](99RIXMD.841f9c8cc3.png)

Once saved and deployed, data should be flowing to your object store. Check your work: Go to the [Monitoring](monitoring) dashboard and select **Data** > **Destinations** at the top. You want to see a green checkmark on the right side for your S3 Destination. If it’s not green, find out why. 

> You can also use an S3-capable browser, like Cyberduck, to check the files more manually. (Specific troubleshooting steps are outside the scope of this doc.)
>
{.box .info}

![A green (healthy) destination](so8JoKK.43d0dd404c.png)

### The Replay Collector

From the top nav of your Cribl Stream Worker Group or single instance, select **Data** > **Sources** > **Collectors** > **S3**, and create a new S3 Collector with the ID `Replay`. Use the **Auto‑populate from** option to pull in the configs from your S3 Destination.

In the **Path** field, we need to establish the tokens to extract from the path on the S3 store. If you used the recommended partitioning scheme in the above example [Destination definition](#destination), we recommend specifying these tokens here:

```
/${MYENV}/${_time:%Y}/${_time:%m}/${_time:%d}/${index}/${host}/${sourcetype}/${_time:%H}${_time:%M}-${extended}
```

If you chose to leave out `C.Vars.MYENV`, exclude the first segment. It is **vital** that this scheme match your partitioning scheme in the Destination definition. 

With these tokens defined, you can now define filters that exclude and include relevant files before Cribl Stream is required to download and open them. This cuts down on the work required, by orders of magnitude. 

> This is a core concept. We want to avoid having to open files to check for matches, by instead relying on key data in the path. Cribl Stream can immediately exclude, without downloading, the files that have no chance of matching our target events. 
> 
> Using `_time`, `sourcetype`, `host`, and `index`, it can accurately zero in on the target files. After this high-level filtering, Cribl Stream will download and interrogate the contents of what’s left, and will send the matches along to the Routing table.
>
{.box .success}

Finally, under **Result Settings** &gt; **Event Breakers**, select the `Cribl` Ruleset. This Ruleset understands how to parse the JSON packaged events stored by the Archive Destination.

![Cribl Event Breaker Ruleset](4cwz2nM.c585f74a73.png)

To make it easier to identify events that have been replayed, create a field (**Result Settings** > **Fields**) named `__replayed` and set it to `true`. You could filter on this field in Routes and Pipelines later. Because it’s a double-underscore field, it’s internal-only, and won’t be passed on to your final destinations. In a previous step, we used it to prevent replayed events from being re-archived:

![__replayed field](LYXJYUP.cee1f34bc0.png)

Now save your Collector, and commit and deploy.

## Testing Replay

After you’ve accumulated some data in your S3 store, head over to Pipelines and start a capture. For the filter, use `__replayed`, and run it for 300 seconds, 10 max events. 

Once it’s running, in a new browser window, navigate back to Collectors, and run your Replay Collector. Filter on `true`, and set the **Earliest** time to `-1h` and the **Latest** to `now`. 

You can run it in Preview mode to make sure you get results, and then come back and do a full run. (Or you can just do a full run right off the bat if you’re confident.) 

With a running job, you can click on the job ID to follow its progress. You can also pop back over to the Capture browser window or tab, and you should see events there.

In a full run, the events will proceed through your Routes as normal, and will land wherever your original Routes and Pipelines dictate. In this case, they landed in Splunk, and we could easily see duplicate events – that is, **exact** duplicate events – in the 1‑hour timeframe that the Collector job defined.

Once defined, this Collector can be controlled via [scheduling](collectors-schedule-run#schedule-config), manual runs, or API calls. And in production use, when configuring the job's **Run configuration** or **Schedule configuration** modal, you’d want to fill in the **Filter** expression to meet your needs.
