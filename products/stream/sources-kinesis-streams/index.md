# Amazon Kinesis Data Streams Source

 

Cribl Stream supports receiving data records from [Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/). 

> Type: **Pull**  |  TLS Support: **YES** (secure API) | Event Breaker Support: **No** | Uses Key-Value Store
>
{.box .info}

## Configure Cribl Stream to Receive Data from Amazon Kinesis Data Streams 

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this Kinesis Stream Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**. 
    - **Description**: Optionally, enter a description.
    - **Stream name**: Enter the name of the Kinesis Data Stream (not ARN) to read data from.
    - **Region**: Select the AWS Region where your Kinesis Data Stream is located. If your Region isn't listed, you can manually enter it. Additionally, you can specify the service endpoint in the **Endpoint** field under [Advanced Settings](#advanced-tab).
3. Next, you can configure the following **Optional Settings**: 
    - **Shard iterator start**: Location from which a newly created Source should start reading any shard, including shards that are added later. Defaults to `Earliest Record`. For details on Leader dependency, see [Leader Dependency](#leader) below.
    - **Record data format**: Format of data inside the Kinesis Stream records. Gzip compression is automatically detected. For details, see [Record Data Format Options](#options).
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#auth), [AssumeRole](#assume), [Processing](#processing) and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 
   
### Leader Dependency {#leader}

The Amazon Kinesis Data Streams Source relies on the Leader Node for proper operation. If the Leader Node becomes unavailable, the Source's performance becomes degraded. In this state:
- Existing shard readers will continue to process data.
- New shard readers will not start.
  
This means that while your current Kinesis data streams will continue to be processed, any changes that require new shard readers (such as increasing the number of shards in your Kinesis stream) will be delayed until the Leader Node is restored. Your Leader Node must be stable for optimal performance.

### Record Data Format Options {#options}
In **Record data format**, options include:
  - [Cribl](destinations-tcp-json#section-format) (the default): Use this option if Cribl Stream wrote data to Kinesis in this format. This is a type of NDJSON.
  - [Newline JSON](https://github.com/ndjson/ndjson-spec): Use if the records contain newline-delimited JSON (NDJSON) events – for example, Kubernetes logs ingested through Kinesis. This is a good choice if you don't know the records' format.
  - [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/ValidateLogEventFlow.html): Use if you've configured CloudWatch to send logs to Kinesis.
   - **Event per line**: NDJSON can use this format when it fails to parse lines as valid JSON.

### Authentication {#auth}

Use the **Authentication method** drop-down to select an AWS authentication method:

**Auto**: This default option uses the [AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-node.html) to automatically obtain credentials in the following order of attempts:

1. **IAM Roles for Amazon EC2**: Loaded from AWS Identity and Access Management (IAM) roles attached to an EC2 instance.
1. **Shared Credentials File**: Loaded from the shared credentials file (`~/.aws/credentials`).
1. **Environment Variables**: Loaded from environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
1. **JSON File on Disk**: Loaded from a JSON file on disk.
1. **Other Credential-Provider Classes**: Other credential-provider classes provided by the AWS SDK for JavaScript.

The `Auto` method works both when running on AWS and in other environments where the necessary credentials are available through one of the above methods.

> ##### SSO Providers
> When using the auto authentication method, you can leverage SSO providers like [SAML](usecase-sso-saml#temp-access) and [Okta](usecase-sso-okta#temp-access) to issue temporary credentials. These credentials should be set in the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. The AWS SDK will then use these environment variables to authenticate.
{.box .info}

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC – for example, those running in a private cloud. 

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key.

#### IAM Role Authentication 

When using an IAM role to authenticate with Kinesis Streams, the IAM policy statements must include the following Actions:

- `kinesis:GetRecords`
- `kinesis:GetShardIterator`
- `kinesis:ListShards`

For details, see AWS' [Actions, resources, and condition keys for Amazon Kinesis Data Streams](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonkinesisdatastreams.html) documentation.

#### Manual Authentication

The **Manual** option exposes these additional fields:

**Access key**: Enter your AWS access key. If not present, will fall back to `env.AWS_ACCESS_KEY_ID`, or to the metadata endpoint for IAM role credentials.

**Secret key**: Enter your AWS secret key. If not present, will fall back to `env.AWS_SECRET_ACCESS_KEY`, or to the metadata endpoint for IAM credentials.

#### Secret Authentication

The **Secret** option exposes this additional field:

**Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](securing-kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Follow the **Create** link if you need to configure a key pair.

### Assume Role {#assume}

When using **Assume Role** to access resources in a different Region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that Region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node. To specify a custom STS endpoint, set the `CRIBL_AWS_STS_ENDPOINT` environment variable. If both are set, `CRIBL_AWS_STS_ENDPOINT` takes precedence. Setting an invalid Region or endpoint results in a fallback to the global STS endpoint.

**Enable for Kinesis Streams**: Whether to use Assume Role credentials to access Kinesis Streams. Default is toggled off.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings {#processing}

#### Fields 

In this section, you can define new fields or modify existing ones using JavaScript expressions, similar to the [Eval](eval-function) function. 

* The **Field Name** can either be a new field (unique within the event) or an existing field name to modify its value.
* The **Value** is a JavaScript expression (enclosed in quotes or backticks) to compute the field's value (can be a constant). Select this field's advanced mode icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.

This flexibility means you can:

* Add new fields to enrich the event.
* Modify existing fields by overwriting their values.
* Compute logic or transformations using JavaScript expressions.

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-tab}

**Shard selection expression**: A JavaScript expression to be called with each `shardId` for the stream. The shard will be processed if the expression evaluates to a truthy value. Defaults to `true`. Avoid trying to use this expression to distribute data evenly across Worker Processes – the preferred way to do that is to use [round robin load balancing](#pull).

**Service period**: Time interval (in minutes) between consecutive service calls that check for configuration updates and perform maintenance. Defaults to `1` minute. This differs from [polling frequency](#polling), which determines how often the Source fetches new data. Adjust the service period to balance the need for quick configuration updates (use a shorter interval) with the frequency of maintenance tasks (use a longer interval for less frequent tasks).

**Endpoint**: Kinesis stream service endpoint. If empty, the endpoint will be automatically constructed from the Region. 

**Signature version**: Signature version to use for signing Kinesis Stream requests. Defaults to `v4`.

**Verify KPL checksums**: Enable this setting to verify Kinesis Producer Library (KPL) event checksums. This setting is not available when Cribl Stream is deployed in [FIPS mode](fips-mode).

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority – self-signed certificates, for example. Defaults to toggled on.

**Avoid duplicate records**: If toggled on, this Source will always start streaming at the next available record in the sequence. (This can cause data loss after a Worker Node's unexpected shutdown or restart.) If toggled off (default), the Source will reread the last two batches of events at startup. (This prevents data loss, but can ingest duplicate events.)

**Records limit per call**: In v.4.0.4 and newer, sets the maximum number of records ingested, per shard, in each [getRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_GetRecords.html) call. You can acclerate data collection by increasing the default/minimum `5000` limit, to as high as `10000` records.

**Total records limit**: In v.4.0.4 and newer, sets the maximum number of records, across all shards, to pull down at once per Worker Process. Default/minimum value is `20000` records. Beware of very high values that could excessively consume Workers' memory.

**Shard load balancing**: The load-balancing algorithm to use for spreading out shards across Workers and Worker Processes. Options include:
- Consistent Hashing (default)
- Round Robin

For the Round Robin option, both the Leader and the Worker Groups must be on Cribl Stream 4.1 or later.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Field for this Source:

* `__checksum`
* `__encryptionType`
* `__final`
* `__inputID`
* `__isKPL`
* `__partitionKey`
* `__sequenceNumber`

Apart from the internal fields, one important non-internal field that this Source adds is `_raw`.

## Principles for Configuration Decisions {#pull}

To get the best results, apply these principles when configuring the Source and Amazon Kinesis Data Streams itself: 

**The number of shards should be a multiple of the number of Worker Processes in the Worker Group.** A ratio of one shard to one Worker Process yields the fastest possible processing. 

**Distributing data evenly across Worker Processes yields the best performance.** To achieve this, choose **Advanced Settings > Shard Load Balancing > Round Robin**. This helps avoid the situation where some Worker Processes are overburdened while others sit idle. Avoid trying to use the **Advanced Settings > Shard selection expression** for this purpose. 

### How the Source and Kinesis Interact

The Leader Node for the Kinesis Streams Source stores shard state on disk, so that the Source can pick up where it left off across restarts. The state file is located in the `$CRIBL_HOME/state/` directory. The path format looks like this:

`.../state/kvstore/<group-id>/input_kinesis_<input-id>_<stream-name>/state.json`

For example:

`state/kvstore/default/input_kinesis_kinesisIn_just-a-test/state.json`

Worker Processes become Kinesis Consumers and get a list of available shards from Kinesis. They then contact the Leader Node to request the sequence numbers that the Leader has stored for those shards. Each sequence number references a specific record. 



Every five minutes, each Worker Process takes the latest sequence numbers for the shards it is responsible for, and forwards those sequence numbers to the Leader Node, enabling the Leader Node to update the state that it is storing. The Leader Node persists the `shardId` > `sequenceNumber` mapping to disk using a key-value store where the keys are Shard IDs and the values are sequence numbers.

In situations where Worker Processes must reload state (by retrieving it from the key-value store on the Leader Node), each Worker Process will resume reading the same shard it was reading before, provided that neither the number of shards nor the number of Worker Processes have changed.

#### Polling Frequency {#polling}

The polling frequency for the Kinesis Streams Source is adaptive. By default, if the Source has caught up to the latest records, it sets a delay of `1` second. If it has not caught up, it immediately requests the next batch of records without waiting. This ensures efficient data consumption without unnecessary delays.

### Setting **Shard iterator start**

Under certain circumstances, the Leader Node will not have any sequence numbers for a given shard and the Worker Process will be reading from that shard for the first time. (These conditions include when the shard is newly added to the Kinesis stream and/or when the Source is newly created.)

When this happens, the Worker Process then does what is specified in **Optional Settings** > **Shard iterator start**: 

* When **Shard iterator start** is set to `Earliest record`, the Worker starts reading from the beginning. 
* When it is set to `Latest record`, the Worker Process resumes reading the shard from the point where Cribl Stream previously left off.



## Troubleshooting

The Source's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](sources#capture-source-data) to see real-time events as they are ingested. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the ingestion process, including any errors or warnings that may have occurred.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify ingestion issues. Analyze the graphs showing events and bytes in/out over time.
