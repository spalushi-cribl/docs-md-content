# System Proxy Configuration


You can direct all outbound HTTP/S requests to go through proxy servers. You do so by setting a few environment variables before starting Cribl Stream, as follows: 

Configure the `HTTP_PROXY` and `HTTPS_PROXY` environment variables, either with your proxy's IP address, or with a DNS name that resolves to that IP address. Optionally, follow either convention with a colon and the port number to which you want to send queries. Some `HTTP_PROXY` examples:

```shell
$ export HTTP_PROXY=http://10.15.20.25:1234
$ export HTTP_PROXY=http://proxy.example.com:1234
```

`HTTPS_PROXY` examples:

```shell
$ export HTTPS_PROXY=http://10.15.20.25:5678
$ export HTTPS_PROXY=http://proxy.example.com:5678
```

In the above examples, note that an `HTTPS_PROXY` environment variable's referenced URL should generally be in `http://` format.

## Preventing Proxy, Config, and Case Conflicts

Before you deploy Cribl Stream and configure proxies, Cribl recommends running the `printenv` shell command to check for any existing proxies in your environment. If you find existing proxies, be sure to include their environment variables in the [systemd override](#systemd-proxy) file.

If Cribl Stream is already running on any Nodes you are proxying, restart the Cribl server after you initially configure – or make any changes to – the variables outlined on this page. 

The environment variables' names can be either uppercase or lowercase. However, if you set duplicate versions of the same name, the lowercase version takes precedence. For example, if you've set both `HTTPS_PROXY` and `https_proxy`, the IP address specified in `https_proxy` will take effect.

##  HTTP and/or HTTPS? {#proxy-protocol}

Several Cribl Stream endpoints rely on the HTTPS protocol. These include the Cribl [telemetry endpoint](licensing#telemetry-data) (which must be accessed under some license types and all Cribl.Cloud plans), and the CDN used to propagate application updates and certain documentation features (API Reference and docs PDFs).

You might configure certain other Cribl Stream features (such as REST API Collectors) that require access to HTTP endpoints. For maximum flexibility, consider setting environment variables to handle both the HTTPS and HTTP protocols.

##  Proxy Configuration with systemd {#systemd-proxy}

If you are proxying outbound traffic and starting Cribl Stream using systemd, add your proxy environment variables to the systemd override file (see [Persisting Overrides](deploy-single-instance#persisting-overrides)). Add statements of this form:

``` {title="Installed systemd File"}
[Service]
Environment=http_proxy=<yourproxy>
Environment=https_proxy=<yourproxy>
Environment=no_proxy=<no_proxy_list>
```

This will prevent Cribl Stream from throwing "failed to send anonymized telemetry metadata" errors.

## Authenticating on Proxies

You can use HTTP Basic authentication on HTTP or HTTPS proxies. Specify the username and password in the proxy URL. For example:

```shell
$ export HTTP_PROXY=http://username:password@proxy.example.com:1234
$ export HTTPS_PROXY=http://username:password@proxy.example.com:5678
```

> If your `username` or `password` contains special characters, Cribl Stream will try to use these fields as the proxy address. As a workaround, [URL‑encode](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding) these fields. 
>
{.box .warning}

## Bypassing Proxies with `NO_PROXY` {#bypass}

If you've set the above environment variables, you can negate them for specified (or all) hosts. Set the `NO_PROXY` environment variable to identify URLs that should bypass the proxy server, to instead be sent as direct requests. Use the following format:

`$ export NO_PROXY="<list of hosts/domains>"`

Cribl recommends including the Leader Node's host name in the `NO_PROXY` list.

#### `NO_PROXY` Usage {#bypass-usage}

Within the `NO_PROXY` list, separate the host/domain names with commas or spaces.

Optionally, you can follow each host/domain entry with a port.
If not specified, the protocol's default port is assumed.

To match subdomains, you must either list them all in full (for example, `NO_PROXY=foo.example.com,bar.example.com`),
or apply a wildcard by prefixing the domain name with a period or `*.`: `No_Proxy=".example.com` or `No_Proxy="*.example.com`.

To match the whole domain including its subdomains, add it both with and without wildcard to the list:
`No_Proxy="example.com,.example.com`.

To disable all proxies, use the `*` wildcard: `NO_PROXY="*"`.
`NO_PROXY` with an empty list disables no proxies.

#### Cloud `NO_PROXY` Usage {#bypass-cloud}

You must include any cloud metadata endpoints (such as the [AWS Instance Metadata Service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html)) in the `NO_PROXY` list:

- AWS EC2 and Azure VM instances must include `169.254.169.254` in the list. If using IPv6 on AWS EC2, add `fd00:ec2::254` to the list.

- AWS ECS Fargate tasks must include `169.254.170.2`.

- GCP (Google Cloud Platform) VM instances must include `metadata.google.internal` and `169.254.169.254`.

## Where Proxies Apply

Proxy configuration is relevant to the following Cribl Stream components that make outbound HTTP/S requests:

### Destinations

  * [Amazon CloudWatch Logs](destinations-cloudwatch-logs)
  * [Amazon Kinesis Streams](destinations-kinesis-streams) 
  * [Amazon S3 Compatible Stores](destinations-s3) 
  * [Amazon SQS](sources-sqs) 
  * [Azure Blob Storage](destinations-azure-blob) 
  * [Azure Monitor Logs](destinations-azure-monitor-logs) 
  * [Cribl HTTP](destinations-cribl-http)
  * [CrowdStrike Falcon LogScale](destinations-humio-hec)
  * [Datadog](destinations-datadog)
  * [Data Lake > S3](destinations-data-lake-s3)
  * [DataSet](destinations-dataset)
  * [Elasticsearch](destinations-elastic) 
  * [Google Chronicle](destinations-google_chronicle)
  * [Google Pub/Sub](destinations-google_pubsub)
  * [Grafana Cloud](destinations-grafana_cloud) 
  * [Honeycomb](destinations-honeycomb) 
  * [Loki](destinations-loki) 
  * [New Relic Ingest: Logs & Metrics](destinations-newrelic)
  * [New Relic Ingest: Events](destinations-newrelic-events)
  * [OpenTelemetry](destinations-otel)
  * [Prometheus](destinations-prometheus) 
  * [SignalFX](destinations-signalfx)
  * [Splunk HEC](destinations-splunk-hec) 
  * [Sumo Logic](destinations-sumo-logic)
  * [Wavefront](destinations-wavefront)
  * [Webhook](destinations-webhook)

### Sources

* [Amazon SQS](sources-sqs) 
* [Amazon S3](sources-s3) 
* [Azure Blob Storage](sources-azure-blob)
* [CrowdStrike FDR](sources-crowdstrike)
* [Office 365 Services](sources-office-365-services)
* [Office 365 Activity](sources-office-365-activity) 
* [Office 365 Message Trace](sources-office365-msg-trace) 
* [Prometheus Scraper](sources-prometheus)
* [Prometheus Edge Scraper](/edge/sources-edge-prometheus)

### Collectors 

* [Amazon S3 Collector](collectors-s3)
* [Azure Blob Storage Collector](collectors-azure-blob)
* [Google Cloud Storage Collector](collectors-google-cloud-storage)
* [Health Check Collector](collectors-health-check)
* [REST/API Collector](collectors-rest)

### Notification Targets

* [PagerDuty](notifications-targets#pagerduty)
* [Webhook](notifications-targets#webhook)

## Testing Proxies  {#proxy-test}

To initially test your proxy configuration, consider setting up a simple, free proxy server like mitmproxy (https://mitmproxy.org), and then monitoring traffic through that server. Verify that you can trace proxied requests from your Cribl Stream instance, and can validate that outgoing requests (to [Destinations](destinations)) are working properly.

## Proxying Multiple Cribl Stream Instances in One Browser {#multiple}

Cribl Stream stores authentication tokens based on each http header's URI scheme, host, and port information. Within a given browser, Cribl Stream enforces a [same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) to isolate instances.

This means that if you want to run multiple proxied Cribl Stream instances in one browser session, you must assign them different URI schemes, hosts, and/or ports. Otherwise, logging into an extra Cribl Stream instance will expire the prior instance's session and log it out.

For example, assume that you've set up this pair of Apache proxy forward rules:

  * https://web/cribla forwards to `cribl_hosta:8001/cribla`.
  * https://web/criblb forwards to `cribl_hostb:8001/criblb`.

These two proxied addresses cannot be run simultaneously in the same browser session. However, this pair – which lead with separate URI schemes – could:
 
  * https://web/cribla forwards to `cribl_hosta:8001/cribla`.
  * https://web2/criblb forwards to `cribl_hostb:8001/criblb`.

Where separate instances **must** share URI formats, a workaround is to open the second instance in an incognito/private browsing window, or in a completely different browser.
