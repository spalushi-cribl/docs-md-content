# Prometheus Scraper


Cribl Stream supports receiving batched data from Prometheus targets. This is a pull Source; to ingest Prometheus streaming data, see [Prometheus Remote Write](sources-prometheus-remote-write).

> Type: **Pull**  |  TLS Support: **No** | Event Breaker Support: **No**
> 
> This Source assumes that incoming data is snappy-compressed. It does not currently support Prometheus metadata.
>
{.box .info}

## Configuring Cribl Stream to Scrape Prometheus Data 

> As of v.4.1.2, this Source is deprecated on Cribl Edge. Cribl plans to remove it from Cribl Edge by v.4.2. Please use the Edge-specific [Prometheus Edge Scraper](/edge/sources-edge-prometheus) Source instead.
>
{.box .warning}

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Prometheus** > **Scraper**. Next, click either **Add Source** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] **Prometheus** > **Scraper**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings {#general}

Additional fields appear in this section depending on what [discovery type](#discovery-type) you select.

**Input ID**: Enter a unique name to identify this Source definition.

**Discovery type**: Use this drop-down to select a discovery mechanism for targets. See [Discovery Type](#discovery-type) below for the options. 

> Some **Discovery type** options replace the **Targets** field with additional controls below the **Poll interval** and **Log level** fields – while also adding an **Assume Role** and/or **Target Discovery** left tab to the modal.
>
{.box .success}

**Poll  interval**: Specify how often (in minutes) to scrape targets for metrics. Defaults to `15`. This value must be an integer that divides evenly into `60`.

**Log level**: Set the verbosity level to one of `debug`, `info` (the default), `warn`, or `error`.

#### Discovery Type {#discovery-type}

Use this drop-down to select a discovery mechanism for targets. To manually enter a targets list, use [Static](#static) (the default). To enable dynamic discovery of endpoints to scrape, select [DNS](#dns) or [AWS EC2](#ec2). Each selection exposes different controls and/or tabs, listed below.

##### Static Discovery {#static}

The `Static` option adds a **General Settings** > **Targets** field, in which you enter a list of specific Prometheus targets from which to pull metrics. 

Values can be in URL or host[:port] format, e.g.: `http://localhost:9090/metrics`, `localhost:9090`, or `localhost`. If you specify only `host[:port]`, the endpoint will resolve to: `http://host[:port]/metrics`. For further options, see [Target Discovery for DNS](#target-dns).

##### DNS Discovery {#dns}

The `DNS` option adds a [Target Discovery](#target-dns) tab to the modal, and adds two extra fields to its **General Settings** tab:
- **DNS names**: Enter a list of DNS names to resolve.
- **Record type**: Select the DNS record type to resolve. Defaults to `SRV` (Service). Other options are `A` or `AAAA` .

##### AWS EC2 Discovery {#ec2}

The `AWS EC2` option adds [Assume Role](#role) and [Target Discovery](#target-aws) tabs to the modal, and adds one extra field to the **Optional Settings** tab:
- **Region**: Select the AWS region in which to discover EC2 instances with metrics endpoints to scrape.

This option also adds controls in the **Advanced Settings** tab, as described [below](#advanced-aws). 

### Optional Settings

**Extra dimensions**: Specify the dimensions to include in events. Defaults to `host` and `source`.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Authentication (Prometheus) {#authentication}

Use the **Authentication Method** buttons to select one of these authentication options for Prometheus:

- **Manual**: In the resulting **Username** and **Password** fields, enter Basic authentication credentials corresponding to your Prometheus targets.

- **Secret**: This option exposes a **Secret** drop-down, in which you can select a stored secret that references your credentials described above. The secret can reside in Cribl Stream's [internal secrets manager](kms-config) or (if enabled) in an external KMS. Click **Create** if you need to configure a new secret.

### Assume Role {#role}

With the [AWS EC2](#ec2) target discovery type, you can configure [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) behavior on AWS.

- **Enable for EC2**: Toggle to `Yes` if you want to use `AssumeRole` credentials to access EC2.

- **AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

- **External ID**: Enter the External ID to use when assuming the role.

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

- **Use public IP**:  The `Yes` default uses the public IP address for discovered targets. Toggle to `No` to use a private IP address.

- **Search filter**: Click **Add filter** to apply filters when searching for EC2 instances. Each filter row provides two columns: 
  - **Filter name**: Select standard [attributes](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html ) from the drop-down, or type in custom attributes.
  - **Filter values**: Enter values to match within this row's attribute, Press `Enter` between values. (If you specify no values, the search will return only `running` EC2 instances.)

##### AWS Authentication Options {#aws-auth}

{{< id `target-discovery-auto` >}} **Auto**: This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance, local credentials, sidecar, or other source. The attached IAM role grants Cribl Stream Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

{{< id `target-discovery-manual` >}} **Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. This option displays the same fields as [Auto](#target-discovery-auto), plus:

 - **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

{{< id `target-discovery-secret` >}} **Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. This option displays the same fields as [Auto](#target-discovery-auto), plus:

- **Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Click **Create** if you need to configure a key pair.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Keep alive time (seconds)**: How often workers should check in with the scheduler to keep job subscription alive. Defaults to `60` seconds.

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Worker timeout (periods)**: How many **Keep alive time** periods before an inactive worker's job subscription will be revoked. Defaults to `3` periods.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Advanced Settings for AWS {#advanced-aws}

These two additional settings appear only when **Optional Settings** > **Discovery Type** is set to `AWS EC2`.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`, the restrictive option.

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

## How Cribl Stream Pulls Data

The Prometheus Source retrieves data using Cribl Stream scheduled Collection jobs. You determine the schedule using your **Poll interval** entry.

With the `DNS` or `AWS EC2` **Discovery Type**, these jobs include both Discover and Collection phases. The Discover phase runs on a single Worker, and returns 1 collection task per discovered target.

The job scheduler spreads the Collection tasks across all available Workers.

## Viewing Scheduled Jobs

This Source executes Cribl Stream's [scheduled collection jobs](/stream/collectors#scheduled). Once you've configured and saved the Source, you can view those jobs' results by reopening the Source's config modal and clicking its **Job Inspector** tab.

You can also view these jobs (among scheduled jobs for other Collectors and Sources) in the **Monitoring** > **System** > **Job Inspector** > **Currently Scheduled** tab.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).
