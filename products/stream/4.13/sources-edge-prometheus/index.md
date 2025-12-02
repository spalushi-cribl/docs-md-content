# Prometheus Edge Scraper Source


Cribl Edge supports receiving batched data from Prometheus targets.

> Type: **Internal**  |  TLS Support: **No** | Event Breaker Support: **No**
> 
> This Source currently does not support Prometheus metadata.
>
{.box .info}

This is an Internal (Pull) Source. To ingest Prometheus streaming data, see [Prometheus Remote Write](sources-prometheus-remote-write).

In addition to the functionality already supported in the existing [Prometheus Scraper](sources-prometheus), this Source is  designed to work seamlessly in Kubernetes environments; and no longer uses internal jobs framework allowing it to handle large-scale Cribl Edge deployments. 

## Configuring Cribl Edge to Scrape Prometheus Data 

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Collect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **More** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. Configure the following under **General Settings**: 
   - **Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
   - **Description**: Optionally, enter a description.
   - **Discovery type**: Use this drop-down to select a discovery mechanism for targets. See [Discovery Type](#discovery-type) below for the options and the resulting controls displayed. Some **Discovery type** options replace this section's **Targets** field with additional controls – while also adding an **AWS IAM** left tab to the modal.
   - **Poll interval**: Specify how often (in seconds) to scrape targets for metrics. Defaults to `15` seconds. This value must be an integer that divides evenly into `60`.
3. Next, you can configure the following **Optional Settings**:
   - **Extra dimensions**: Specify the dimensions to include in events. Defaults to `host` and `source`.
   - **Tags**: Optionally, add tags that you can use for filtering and grouping in the Cribl Edge UI. Use a tab or hard return between (arbitrary) tag names. These tags aren't added to processed events.
4. Optionally, configure any [Authentication](#auth), [Processing](#processing), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

#### Discovery Type {#discovery-type}

Use this drop-down to select a discovery mechanism for targets. To manually enter a targets list, use [Static](#static) (the default). To enable dynamic discovery of endpoints to scrape, select [DNS](#dns) or [AWS EC2](#ec2). Each selection exposes different controls and/or tabs, listed below.

##### Static Discovery {#static}

The `Static` option adds a **General Settings** > **Targets** field, in which you enter a list of specific Prometheus targets from which to pull metrics. Click **Add Target** to expose a table with the following options:

- **Protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 
- **Host**: Specify the host name to pull metrics from.
- **Port**: Specify the port number to append to the metrics URL for discovered targets. Defaults to `9090`.
- **Path**: Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.

##### DNS Discovery {#dns}

The `DNS` option adds the following extra fields to its **General Settings** tab:
- **Record type**: Select the DNS record type to resolve. Defaults to `SRV` (Service). Other options are `A` or `AAAA`.
- **DNS names**: Enter a list of DNS names to resolve.
- **Protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 
- **Path**:  Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.

##### AWS EC2 Discovery {#ec2}

The `AWS EC2` option adds [AWS IAM](#role) to the modal, and adds extra fields to the **General Settings** tab:
- **Protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 
- **Port**: Specify the port number to append to the metrics URL corresponding to discovered targets. Defaults to `9090`.
- **Path**: Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.
- **Region**: Select the AWS Region in which to discover EC2 instances with metrics endpoints to scrape.
- **Use public IP**:  Toggle on (default) to use the public IP address for discovered targets. Toggle off to use a private IP address.
- **Search filter**: Click **Add filter** to apply filters when searching for EC2 instances. Each filter row provides two columns: 
  - **Filter name**: Select standard [attributes](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html ) from the drop-down, or type in custom attributes.
  - **Filter values**: Enter values to match within this row's attribute, Press `Enter` between values. (If you specify no values, the search will return only `running` EC2 instances.)

Selecting`AWS EC2` also adds controls to the **Advanced Settings** tab, as described [below](#advanced-aws). 

##### Kubernetes Node {#k8node}

The `Kubernetes Node` option supports collecting metrics from an endpoint on the local node where Cribl Edge is running. Selecting this option adds the following extra fields to the **General Settings** tab:

- **Protocol**: Select `http` (the default) or `https` as the protocol to use when collecting metrics. 
- **Port**: Specify the port number to append to the metrics URL corresponding to discovered targets. Defaults to `9090`.
- **Path**: Specify a path to use when collecting metrics from discovered targets. Defaults to `/metrics`.

Selecting `Kubernetes Node` also adds a control to the **Authentication** tab, as described [below](#k8auth). 

You must first authorize the Source to discover a Kubernetes Node. For details, see [RBAC for Prometheus Edge Scraper](/edge/deploy-running-kubernetes#scraper).

##### Kuberenetes Pods {#k8pods}

The `Kubernetes Pods` option supports building a list of endpoints to collect from Pods on the same node where Cribl Edge is running. This option adds the following extra fields to the **General Settings** tab – use a custom expression to set the configuration properties on each field.

- **Protocol**: Set the protocol to use when collecting metrics. Defaults to: `metadata.annotations['prometheus.io/scheme'] || 'http'` 
- **Port**: Specify the port number to append to the metrics URL corresponding to discovered targets. Defaults to: `metadata.annotations['prometheus.io/port'] || '9090'`
- **Path**: Specify a path to use when collecting metrics from discovered targets. Defaults to: `metadata.annotations['prometheus.io/path'] || '/metrics'`
- **Filter rules**: Optionally, add rules to determine which Pods to discover for metrics. If no rules are provided, Cribl Edge will search all Pods. Otherwise, it will search Pods where rules' expressions evaluate to `true`. Each rule consists of an expression and an optional description.
  - **Filter expression**: JavaScript expression applied to Pods' objects. Return `true` to include it. Filters are based on the [Kubernetes Pod Object definition](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/), and are evaluated from top to bottom. The first expression evaluating to `false` excludes a Pod from collection. The default filter is `metadata.annotations['prometheus.io/scrape']`, which scrapes the Pod if the annotation is true.
  - **Description**: Optional description of the rule.

Selecting `Kubernetes Pods` also adds a control to the **Authentication** tab, as described [below](#k8auth). 

You must first authorize the Source to discover Kubernetes Pods. For details, see [RBAC for Prometheus Edge Scraper](/edge/deploy-running-kubernetes#scraper).

### Authentication (Prometheus) {#auth}

Use the **Authentication method** drop-down to select one of these authentication options for Prometheus:

- **Manual**: In the resulting **Username** and **Password** fields, enter Basic authentication credentials corresponding to your Prometheus targets.

- **Secret**: This option exposes a **Secret** drop-down, in which you can select a stored secret that references your credentials described above. The secret can reside in Cribl Edge's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. Click **Create** if you need to configure a new secret.

#### Authentication Settings for Kubernetes {#k8auth}

This additional setting appears only when **General Settings** > **Discovery Type** is set to `Kubernetes Pods` or `Kubernetes Node`.

- **Kubernetes**: Select this option to use Kubernetes authentication. There are no details to configure, because Cribl Edge will authenticate using the Environment Variables or Service Account tokens mounted in the Edge Pod(s). For details, see [Deploying via Kubernetes](deploy-running-kubernetes).

### AWS IAM {#role}

With the [AWS EC2](#ec2) target discovery type, you can configure [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) behavior on AWS.

#### Assume Role 

- **Enable for EC2**: Toggle on if you want to use `AssumeRole` credentials to access EC2.

- **AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

- **External ID**: Enter the External ID to use when assuming the role.

#### AWS Authentication Options {#aws-auth}

{{< id `target-discovery-auto` >}}
[Snippet not found: content/shared/4.13/snippets/_integrations-aws-auto-auth.md]

{{< id `target-discovery-manual` >}} **Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Edge Nodes not in an AWS VPC, for example, those running a private cloud. This option displays:

 - **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

{{< id `target-discovery-secret` >}} **Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. This option displays:

- **Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](securing-kms-config) in Cribl Edge's internal secrets manager or (if enabled) an external KMS. Click **Create** if you need to configure a key pair.

### Processing Settings {#processing}

#### Fields 

[Snippet not found: content/shared/4.13/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

#### Disk Spooling

**Enable disk spooling**: Whether to save metrics to disk. When toggled on, it exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Stream will delete older data. Example values: `420 MB`, `4 GB`. Default value: `1 GB`. 

**Max data age**: How long to retain data. Once reached, Cribl Stream will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Defaults to `gzip`. 

Cribl Stream will write metrics to the default location: `CRIBL_HOME/state/spool/in/edge_prometheus/${inputId}`. Use the [environment variable](environment-variables#additional-environment-variables) `CRIBL_SPOOL_DIR`, to change the default path. 

### Advanced Settings {#advanced-tab}

**HTTP Connection Timeout**: The amount of time (in milliseconds) to wait for the HTTP connection to establish before it times out. `1-60000` is the allowable range. Enter `0` to disable the timeout.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Advanced Settings for AWS {#advanced-aws}

These additional settings appear only when **General Settings** > **Discovery Type** is set to `AWS EC2`.

**Endpoint**: Specify an EC2-compatible service endpoint. If empty, this defaults to AWS' Region-specific endpoint.

**Signature version**: The signature version to use for signing EC2 requests. Defaults to `v4`. 

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on, the restrictive option.

## Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
 
  * `__source`
  * `__isBroken`
  * `__inputId`
  * `__final`
  * `__criblMetrics`
  * `__channel`
  * `__cloneCount`
  * `__kube_pod` when Discovery Mode is set to **Kubernetes Node**.
  * `__kube_node` and `__kube_pod` when Discovery Mode is set to **Kubernetes Pods**.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting

[Snippet not found: content/shared/4.13/snippets/_common-errors-http-based-source.md]
