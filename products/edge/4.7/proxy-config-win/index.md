# System Proxy Configuration on Windows


You can direct all outbound HTTP/S requests to go through proxy servers.

## Enable Proxy Server

First, you need to make sure the proxy server is enabled.

There are two ways to do it, using Windows system settings and Group Policy Objects.

### Windows Proxy Settings

To enable the proxy server for all processes and services on a host, you can use the system proxy settings:

1. In Windows settings go to **Network & Internet** > **Proxy**.
1. Under **Manual proxy setup**, toggle **Use a proxy server** to **On**.
1. In the **Address** and **Port** boxes, enter the proxy server name or IP address and (optionally) port.
1. Confirm with **Save**.

### Group Policy Object

Alternatively, you can use a Windows Group Policy Object to enable the proxy server
across groups of accounts or groups of servers.
See [Microsoft Q&A thread](https://learn.microsoft.com/en-us/answers/questions/1263144/setting-up-gpo-for-proxy-settings#answers)
for a detailed instruction.


## Communication with Leader over Proxy

If you outbound TCP connection from an Edge Node to the Leader must go through proxy,
you need to set the `CRIBL_DIST_WORKER_PROXY` environment variable:

`CRIBL_DIST_WORKER_PROXY=<socks4|socks5>://<username>:<password>@<host>:<port>`

##  HTTP and/or HTTPS?  {#proxy-protocol}

Several Cribl Edge endpoints rely on the HTTPS protocol – the Cribl [telemetry endpoint](licensing#telemetry-data) – which must be accessed with some license types, as well as the CDN used to propagate application updates, certain documentation features (API Reference and docs PDFs), and config bundle downloads.

You might configure certain other Cribl Edge features that require access to HTTP endpoints. For maximum flexibility, consider setting environment variables to handle both the HTTPS and HTTP protocols.

## Authenticating on Proxies

You can use HTTP Basic authentication on HTTP or HTTPS proxies. Specify the user name and password in the proxy URL. For example:

```bash
$environment = [string[]]@("HTTP_PROXY=http://username:password@proxy:1234","HTTPS_PROXY=http://username:password@proxy:1234")
Set-ItemProperty HKLM:SYSTEM\CurrentControlSet\Services\Cribl -Name Environment -Value $environment
```

## Bypassing Proxies with `NO_PROXY` {#bypass}

If you've set the above environment variables, you can negate them for specified (or all) hosts. Add the `NO_PROXY` environment variable to identify URLs that should bypass the proxy server, to instead be sent as direct requests. Use the following format:

```shell
NO_PROXY="<list of hosts/domains>"
```

Cribl recommends including the Leader Node's host name in the `NO_PROXY` list.

#### `NO_PROXY` Usage {#bypass-usage}

Within the `NO_PROXY` list, separate the host/domain names with commas or spaces.

Optionally, you can follow each host/domain entry with a port.
If not specified, the protocol's default port is assumed.

To match subdomains, you must either list them all in full (for example, `NO_PROXY=foo.example.com,bar.example.com`),
or apply a wildcard by prefixing the domain name with a period or `*.`: `NO_PROXY=".example.com` or `NO_PROXY="*.example.com`.

To match the whole domain including its subdomains, add it both with and without wildcard to the list:
`NO_PROXY="example.com,.example.com`.

To disable all proxies, use the `*` wildcard: `NO_PROXY="*"`.
`NO_PROXY` with an empty list disables no proxies.

#### Cloud `NO_PROXY` Usage {#bypass-cloud}

You must include any cloud metadata endpoints (such as the [AWS Instance Metadata Service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html)) in the `NO_PROXY` list:

- AWS EC2 and Azure VM instances must include `169.254.169.254` in the list. If using IPv6 on AWS EC2, add `fd00:ec2::254` to the list.

- AWS ECS Fargate tasks must include `169.254.170.2`.

- GCP (Google Cloud Platform) VM instances must include `metadata.google.internal` and `169.254.169.254`.

## Where Proxies Apply

Proxy configuration is relevant to the following Cribl Edge components that make outbound HTTP/S requests:

### Destinations

  * [S3 Compatible Stores](destinations-s3) 
  * [Amazon Kinesis Data Streams](destinations-kinesis-streams) 
  * [AWS CloudWatch Logs](destinations-cloudwatch-logs)
  * [AWS SQS](sources-sqs) 
  * [Azure Blob Storage](destinations-azure-blob) 
  * [Azure Monitor Logs](destinations-azure-monitor-logs) 
  * [Cribl HTTP](destinations-cribl-http)
  * [CrowdStrike Falcon LogScale](destinations-humio-hec)
  * [Elasticsearch](destinations-elastic) 
  * [Grafana Cloud](destinations-grafana_cloud) 
  * [Honeycomb](destinations-honeycomb) 
  * [Loki](destinations-loki) 
  * [Prometheus](destinations-prometheus) 
  * [Splunk HEC](destinations-splunk-hec) 
  * [Webhook](destinations-webhook)

### Notification Targets

* [PagerDuty](notifications-targets#pagerduty)
* [Webhook](notifications-targets#webhook)

## Testing Proxies {#proxy-test}

To initially test your proxy configuration, consider setting up a simple, free proxy server like mitmproxy (https://mitmproxy.org), and then monitoring traffic through that server. Verify that you can trace proxied requests from your Cribl Edge instance, and can validate that outgoing requests (to [Destinations](destinations)) are working properly.

