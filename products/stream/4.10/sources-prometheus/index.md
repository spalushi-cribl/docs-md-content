# Prometheus Scraper Source


Cribl Stream supports receiving batched data from Prometheus targets. This is a pull Source; to ingest Prometheus streaming data, see [Prometheus Remote Write](sources-prometheus-remote-write).

> Type: **Pull**  |  TLS Support: Configurable | Event Breaker Support: **No**
> 
> This Source assumes that incoming data is snappy-compressed. It does not currently support Prometheus metadata.
>
{.box .info}

## Configure Cribl Stream to Scrape Prometheus Data 

>As of version 4.2, we've removed this Source from Cribl Edge. Please use the Edge-specific [Prometheus Edge Scraper](/edge/sources-edge-prometheus) Source instead.
>
{.box .warning}

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
     - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
     - **Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
     - **Description**: Optionally, enter a description.
     - **Discovery type**: Use this drop-down to select a discovery mechanism for targets. See [Discovery Type](#discovery-type) below for the options. 

> Some **Discovery type** options replace the **Targets** field with additional controls below the **Poll interval** and **Log level** fields – while also adding an **Assume Role** and/or **Target Discovery** left tab to the modal.
>
{.box .success}

3. Next, you can configure the following **Optional Settings**: 
   - **Extra dimensions**: Specify the dimensions to include in events. Defaults to `host` and `source`.
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#auth), [Processing](#processing), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 


#### Discovery Type {#discovery-type}

Use this drop-down to select a discovery mechanism for targets. To manually enter a targets list, use [Static](#static) (the default). To enable dynamic discovery of endpoints to scrape, select [DNS](#dns) or [AWS EC2](#ec2). Each selection exposes different controls and/or tabs, listed below.

##### Static Discovery {#static}

The `Static` option adds a **General Settings** > **Targets** field, in which you enter a list of specific Prometheus targets from which to pull metrics. 

Values can be in URL or `host[:port]` format, for example: `http://localhost:9090/metrics`, `localhost:9090`, or `localhost`. 

If you specify only `host[:port]`, the endpoint will resolve to: `http://host[:port]/metrics` – that is, the protocol will be prepended and the `/metrics` path will be appended in all cases, even if you add a path. If you need a nonstandard path, you must enter the full URL (including protocol) with path. For example: `http://localhost:9090/my/custom/path`.

##### DNS Discovery {#dns}

The `DNS` option adds a [Target Discovery](#target-dns) tab to the modal, and adds two extra fields to its **General Settings** tab:
- **DNS names**: Enter a list of DNS names to resolve.
- **Record type**: Select the DNS record type to resolve. Defaults to `SRV` (Service). Other options are `A` or `AAAA` .

##### AWS EC2 Discovery {#ec2}

The `AWS EC2` option adds [Assume Role](#role) and [Target Discovery](#target-aws) tabs to the modal, and adds one extra field to the **Optional Settings** tab:
- **Region**: The AWS region in which to discover EC2 instances with metrics endpoints to scrape.

This option also adds controls in the **Advanced Settings** tab, as described [below](#advanced-aws). 

### Authentication (Prometheus) {#auth}

Use the **Authentication method** drop-down to select one of these authentication options for Prometheus:

- **Manual**: In the resulting **Username** and **Password** fields, enter Basic authentication credentials corresponding to your Prometheus targets.

- **Secret**: This option exposes a **Secret** drop-down, in which you can select a stored secret that references your credentials described above. The secret can reside in Cribl Stream's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. Click **Create** if you need to configure a new secret.

### Assume Role {#role}

With the [AWS EC2](#ec2) target discovery type, you can configure [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) behavior on AWS.

When using Assume Role to access resources in a different region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node. Setting an invalid region results in a fallback to the global STS endpoint.

- **Enable for EC2**: Toggle on if you want to use `AssumeRole` credentials to access EC2.

- **AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

- **External ID**: Enter the External ID to use when assuming the role.

- **Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Target Discovery {#discovery}

Setting the [General Settings](#general) > [Discovery type](#discovery-type) drop-down to [DNS](#dns) or [AWS EC2](#ec2) exposes this tab. These two discovery types expose different controls here.

#### Target Discovery for DNS {#target-dns}

Setting the [Discovery type](#discovery-type) drop-down to [DNS](#dns) exposes the following **Target Discovery** fields. 

**Metrics protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 



**Metrics path**:  Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.

#### Target Discovery for AWS {#target-aws}

Setting the [Discovery type](#discovery-type) drop-down to [AWS EC2](#ec2) exposes the following **Target Discovery** controls. The first controls is a special case:

- **Authentication method**: Select the **Auto**,  **Manual**, or **Secret** button to determine how Cribl Stream will authenticate against AWS. Each selection changes the fields displayed on this tab – see [AWS Authentication Options](#aws-auth) for details.

These remaining controls are displayed for all **Authentication method** selections:

- **Metrics protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 

- **Metrics port**: Specify the port number to append to the metrics URL for discovered targets. Defaults to `9090`.

- **Metrics path**: Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.

- **Use public IP**:  Toggle on (default) to use the public IP address for discovered targets. Toggle off to use a private IP address.

- **Search filter**: Click **Add filter** to apply filters when searching for EC2 instances. Each filter row provides two columns: 
  - **Filter name**: Select standard [attributes](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html ) from the drop-down, or type in custom attributes.
  - **Filter values**: Enter values to match within this row's attribute, Press `Enter` between values. (If you specify no values, the search will return only `running` EC2 instances.)

##### AWS Authentication Options {#aws-auth}

{{< id `target-discovery-auto` >}}
[Snippet not found: content/shared/4.10/snippets/_integrations-aws-auto-auth.md]

{{< id `target-discovery-manual` >}} **Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, for example, those running a private cloud. This option displays the same fields as [Auto](#target-discovery-auto), plus:

 - **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

{{< id `target-discovery-secret` >}} **Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. This option displays the same fields as [Auto](#target-discovery-auto), plus:

- **Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](securing-kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Click **Create** if you need to configure a key pair.

### Processing Settings {#processing}

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-tab}

**Reject unauthorized certificates**: Whether to accept certificates (such as self-signed certificates, for example) that cannot be verified against a valid Certificate Authority. Defaults to toggled on.

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Advanced Settings for AWS {#advanced-aws}

These two additional settings appear only when **Optional Settings** > **Discovery Type** is set to `AWS EC2`.

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on, the restrictive option.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
 
  * `__source`
  * `__isBroken`
  * `__inputId`
  * `__final`
  * `__criblMetrics`
  * `__channel`
  * `__cloneCount`

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## How Cribl Stream Pulls Data

The Prometheus Source retrieves data using Cribl Stream scheduled Collection jobs. You determine the schedule using your **Poll interval** entry.

With the `DNS` or `AWS EC2` **Discovery Type**, these jobs include both Discover and Collection phases. The Discover phase runs on a single Worker, and returns 1 collection task per discovered target.

The job scheduler spreads the Collection tasks across all available Workers.

## Viewing Scheduled Jobs

This Source executes Cribl Stream's [scheduled collection jobs](/stream/collectors#scheduled). Once you've configured and saved the Source, you can view those jobs' results by reopening the Source's config modal and clicking its **Job Inspector** tab.

You can also view these jobs (among scheduled jobs for other Collectors and Sources) in the **Monitoring** > **System** > **Job Inspector** > **Currently Scheduled** tab.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting

[Snippet not found: content/shared/4.10/snippets/_sources-troubleshooting.md]

### Common Issue

[Snippet not found: content/shared/4.10/snippets/_common-errors-http-based-source.md]
