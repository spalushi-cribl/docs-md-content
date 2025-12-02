# Configure Upstream Logging Agents


This page explains how to quickly connect a wide selection of common logging agents and other log sources to Cribl Stream. 
The examples below are equally valid for Cribl.Cloud and on-prem Cribl Stream instances.

Whichever source you choose, start by doing a [live capture](data-preview#live) on the corresponding Source in Cribl Stream. 
Verify that data is coming in. Then save your capture to a sample file, which will help you build your Pipelines.

## Fluent Bit and Fluentd {#fluent-bit-and-fluentd}

Both [Fluent Bit](https://github.com/fluent/fluent-bit) (written in C) and [fluentd](https://github.com/fluent/fluentd) (written in Ruby) are open-source log collectors, processors, and aggregators. 

### Fluent Bit to HEC {#fluent-bit-to-hec}

Of Fluent Bit's many output options, several work with Cribl Stream. 
Here's how to connect a Cribl Stream [Splunk HEC Source](sources-splunk-hec) to Fluent Bit with the Splunk HEC formatting option.
 
You'll need to copy and paste the following:
- From the Cribl.Cloud sidebar, select **Workspace** > **Data Sources** and copy the `in_splunk_hec` ingest address. 
- From a Cribl Stream instance or Worker Group, select **Data** > **Sources** > **Splunk HEC** > `<source_name>` > **Auth Tokens**, and copy the HEC token. 

Decide how to adjust the `match` parameter for your use case. 
For example, an asterisk (wildcard) value will send **all** events to Cribl Stream. 

Edit the Fluent Bit settings accordingly. They're usually in `/etc/td-agent-bit/td-agent-bit.conf`, in a form similar to this:

```shell
[OUTPUT]
    name         splunk
    match        *
    host         in.main-default-<organization>.cribl.cloud
    port         8088
    splunk_token <HEC_token>
    tls          on
    # optional
```
Save the changes and restart the `td-agent-bit` service.

### Fluent Bit to Elastic Sources {#fluent-bit-to-elastic}

You can also connect Cribl Stream to Fluent Bit, using an Elastic Source.

```shell
[OUTPUT]
    name         es
    match        *
    host         10.0.21.134
    port         9200
    index        my_index
    type         my_type
```

### Fluentd to HEC {#fluentd-to-hec}

If you don’t already have it, install the `splunk_hec` fluentd mod: 

`sudo gem install fluent-plugin-splunk-hec`

You'll need to copy and paste the following:
- From the Cribl.Cloud sidebar, select **Workspace** > **Data Sources** and copy the `in_splunk_hec` ingest address. 
- From a Cribl Stream instance or Worker Group, select **Data** > **Sources** > **Splunk HEC** > `<source_name>` > **Auth Tokens**, and copy the HEC token. 

Decide how to adjust the `match` parameter for your use case. 
Use an asterisk (wildcard) if you want to send **all** events to Cribl Stream. 

Edit the `<match` section of the fluentd settings accordingly. They could be in `/etc/fluent/fluent.conf` or another location, as described in the fluentd [docs](https://docs.fluentd.org/configuration).

```shell
<match **>
  @type splunk_hec
  @log_level info
  hec_host in.main-default-<organization>.cribl.cloud
  hec_port 8088
  hec_token <HEC_token>
  index <index_name>
  source_key <file_path>
  <format>
    @type json
  </format>
</match>
```

Save the changes and restart fluentd.

## Splunk

Cribl Stream can directly ingest the native Splunk2Splunk (S2S) protocol. As a result, both Splunk [universal](https://docs.splunk.com/Documentation/Forwarder/8.2.4/Forwarder/Abouttheuniversalforwarder) and 
[heavy](https://docs.splunk.com/Splexicon:Heavyforwarder) forwarders can send log data to Cribl Stream. 

### Splunk Forwarder (Universal or Heavy) to Splunk TCP

See how to configure a Splunk Forwarder in the [Splunk TCP Source](sources-splunk#config-splunk-fwd) document.

## Elastic

In the Elastic ecosystem, [Elastic Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html) is an agent that functions as a lightweight shipper for forwarding and centralizing log data.

### Elastic Filebeat {#elastic-filebeat}

Cribl Stream can ingest the native Elasticsearch streaming protocol directly.  

Cribl recommends setting an authorization token so that your receiver will accept only your traffic. This [setting](sources-elastic#general-settings) is in the **Auth Tokens** field of Cribl Stream's
**Sources** > **Elasticsearch API** > **General Settings** tab.

If you want to enable Basic authentication: 
* Concatenate your username and password with a colon in between, like this: `username:password`.
* Encode the `username:password` string using base64. 
* Prepend the string `Basic ` (including the trailing space) to the encoded string. 
* Enter the resulting string in the **Auth tokens** field as described above.

You'll see something like this:

![Basic auth in the Cribl Stream Elasticsearch API Source](log-agt-00.5bbd176bfe.png)
{border="true"}

```yaml
output.elasticsearch:
  hosts: ["in.main-default-<organization>.cribl.cloud:9200/search"]
  protocol: "https"
  ssl.verification_mode: "full"
  username: <some_username>
  password: <some_password>
```

## syslog {#syslog}

To receive syslog data in Cribl Stream, create a native [Syslog Source](sources-syslog). See Cribl's [best practices](usecase-syslog) for working with syslog data. 

> Cribl recommends not using UDP over the public internet. 
> UDP examples are included here for test purposes only.
> 
> UDP, by definition, does not guarantee delivery. While TCP mitigates this problem, using either UDP or TCP without TLS over the public internet exposes unencrypted data.
>
{.box .warning}

### syslog-ng (TLS over TCP)

Cribl recommends using TLS over TCP to send data from syslog-ng to Cribl Stream.

Here's an example of `syslog-ng.conf` edited such that all event data will be encrypted on the way to Cribl.Cloud. 
Adapt the snippet to your use case and edit `syslog-ng.conf` accordingly.

```
destination d_syslog {
    syslog("in.main-default-<organization>.cribl.cloud"
        transport("tls")
        port(6514)
        tls(
            peer-verify(required-trusted)
            ca-dir("/etc/syslog-ng/ca.d")
        )
    );
};

log {
    source(s_network);
    destination(d_syslog);
};
```

Save the changes and restart the syslog-ng service.

### syslog-ng (UDP or TCP)

Here's an example of `syslog-ng.conf` – for test purposes only, because it does not use TLS.

Adapt the snippet to your use case, and edit `syslog-ng.conf` accordingly. You can change `tcp` to `udp` to switch protocols.

```
destination d_syslog {
    syslog("in.main-default-<organization>.cribl.cloud"
        transport("tcp")
        port(9514)
    );
};

log {
    source(s_network);
    destination(d_syslog);
};
```

Save the changes and restart the syslog-ng service.

### syslog-ng with Load-Balanced Outputs

You can forward logs from syslog-ng to multiple Cribl Stream Workers through a redundant, load-balanced output by using the syslog-ng `elasticsearch-http` [output module](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.33/administration-guide/33#TOPIC-1663280). With this arrangement, syslog-ng receives syslog messages over TCP and/or UDP and outputs events over the [Elasticsearch HTTP Bulk API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html) to Cribl Stream Workers.

> Forwarding to Cribl Stream from the syslog-ng `elasticsearch-http` output module requires syslog-ng version 3.19 or higher. 
> 
> To check your syslog-ng version, run the following command:
> 
> ```shell
> syslog-ng -V
> ```
>
{.box .warning}

Adapt the snippet to your use case, and edit `syslog-ng.conf` accordingly. To learn more about syslog-ng destination flags, see the syslog-ng [Administration Guide](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.33/administration-guide/34). 

```
@version: 3.33
@include "scl.conf"

# Inputs
source s_network {
    # flags(no-parse) disables syslog-ng parsing of syslog messages (raw passthru)
    network(transport("tcp") port("9514") flags(no-parse));
    network(transport("udp") port("9514") flags(no-parse));
};

destination d_elasticsearch_tls {
    # https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.33/administration-guide/34
    elasticsearch-http(url("https://worker1:9200/_bulk" "https://worker2:9200/_bulk" "https://worker3:9200/_bulk")
        batch-lines(100)
        batch-bytes(512Kb)
        batch-timeout(2500)
        persist-name("d_elasticsearch-http-load-balance")
        type("")
        index("syslog")
        # Auth token
        #headers("Authorization: <token from Stream>")
        # or Basic Auth
        #user("username")
        #password("password")
    );
};

log {
    source(s_network);
    destination(d_elasticsearch_tls);
};
```

Save the changes and restart the syslog-ng service.

### rsyslog (TLS over TCP)

You'll need to install the `rsyslog-gnutls` package if it’s not already on your system. E.g.: 

`apt install rsyslog-gnutls`

If the `rsyslog-gnutls package` is not already on your system, run the following command (or equivalent) to install it:

`apt install rsyslog-gnutls`

Cribl recommends using TLS over TCP to send data from rsyslog to Cribl Stream.

Here's an example of `rsyslog.conf`, edited such that all event data will be encrypted on the way to Cribl.Cloud. Adapt the snippet to your use case, and edit `rsyslog.conf` accordingly:

```
# path to the crt file.
# You will need a valid ca cert file. Most linux distros come with this
$DefaultNetstreamDriverCAFile /etc/ssl/certs/ca-certificates.crt

*.*	action(
         type="omfwd"
         target="in.main-default-<organization>.cribl.cloud"
         port="6514"
         protocol="tcp"
         action.resumeRetryCount="100"
         queue.type="linkedList"
         queue.size="10000"
         StreamDriver="gtls"
         StreamDriverMode="1" # run driver in TLS-only mode
)
```

Save the changes and restart the rsyslog service.

### rsyslog (UDP or TCP)

Here's an example of `rsyslog.conf` – for test purposes only, because it does not use TLS.

Adapt the snippet to your use case, and edit `rsyslog.conf` accordingly. You can change `tcp` to `udp` to switch protocols. 

```
*.*	action(
         type="omfwd"
         target="in.main-default-<organization>.cribl.cloud"
         port="9514"
         protocol="tcp"
         action.resumeRetryCount="100"
         queue.type="linkedList"
         queue.size="10000"
)
```

Save the changes and restart the rsyslog service.

### Appliances that Send syslog

Many appliances emit syslog data. At a minimum, configure your appliance(s) to send syslog over TCP with TLS enabled, if possible. 

Even better, stand up a Cribl Stream instance close to the appliances in your network topology. Have the appliances send syslog to the nearby Cribl Stream, which can optionally transform, filter, and/or enhance the data, and then send it to a more centralized Cribl Stream cluster in Cribl.Cloud or an one-prem location.

The Cribl Stream worker that's closest to the appliance will use TCP JSON, with TLS enabled, to send syslog to the second Cribl Stream instance:

![Sending syslog from an appliance to Cribl Cloud](log-agt-01.19ddcd851a.png)
{border="false"}

The benefits of placing the first Cribl Stream instance close to the log producer include: 
* Since there are fewer network hops, UDP becomes a more viable option, if you want it. 
* If you're using TCP, that can be more performant here than it would be across the internet. 
* The first Cribl Stream instance can do data reduction and/or compression. These operations reduce the volume of the data hitting the wire on the way to the next hop. 
* This can help keep data egress costs under control.

## Vector

[Vector](https://vector.dev/docs/about/) is Datadog's tool for building observability pipelines.

### Vector to HEC

Vector supports Splunk's HTTP Event Collector (HEC). 
Here's how to connect a Cribl Stream [Splunk HEC Source](sources-splunk-hec) to Vector.
 
For Cribl.Cloud especially, Cribl recommends setting an authorization token so that your receiver will accept only your traffic. 
This [setting](sources-splunk#auth-tokens) is in Cribl Stream's
**Sources** > **Splunk HEC** > **Auth Tokens** tab.

The snippet below references a random syslog generator called `dummy_logs` for testing purposes. 
Adapt the snippet to your use case and edit `vector.toml` (or `vector.yaml` or `vector.json`) accordingly.

```toml
[sinks.my_sink_id]
type           = "splunk_hec"
inputs         = [ "dummy_logs" ]
endpoint       = "https://in.main-default-<organization>.cribl.cloud:8088"
compression    = "gzip"
token          = "<optional-but-recommended>"
encoding.codec = "json"
```

### Vector to Elasticsearch

Vector supports Elasticsearch API. 
Here's how to connect a Cribl Stream [Elasticsearch API Source](sources-elastic) to Vector. 

The snippet below references a random syslog generator called `dummy_logs` for testing purposes. 
The snippet also specifies Basic authentication. Follow the procedure for enabling Basic authentication described in the Elastic Filebeat example [above](#elastic-filebeat).

Adapt the snippet to your use case and edit `vector.toml` (or `vector.yaml` or `vector.json`) accordingly.

```
[sinks.elastic_test]
type          = "elasticsearch"
inputs        = [ "dummy_logs" ]
endpoint      = "https://in.main-default-<organization>.cribl.cloud:9200/search"
compression   = "gzip"
index         = "my_es_index"
auth.user     = "<some_username>"
auth.password = "<some_password>"
auth.strategy = "basic"
```

## Pull-based Sources

All the above data sources **push** data into Cribl Stream. Here are brief notes on configuring some popular Cribl Stream-native [Pull Sources](sources#pull-sources):



### Amazon Kinesis

You can create an [Amazon Kinesis Source](sources-kinesis-firehose) in Cribl Stream.
This is straightforward for on-prem Cribl Stream instances.

Cribl.Cloud sits behind an network load balancer, which Amazon Data Firehose does not support. 
For this reason, the best option for connecting Amazon Kinesis to Cribl.Cloud is to (1) have Amazon Data Firehose write to an Amazon S3 bucket, 
and (2) set up a Cribl Stream Amazon S3 Source to pull data from there, as described in the next section.

### Amazon S3

Configuring an [S3 Source](sources-s3) is virtually the same for Cribl.Cloud as it is for on-prem Cribl Stream instances.

See [this](https://cribl.io/blog/a-simple-guide-to-scalable-data-collection-from-amazon-s3/) in-depth article about the Cribl Stream S3 Source, whose content also applies to Cribl.Cloud.
Here's an overview of the process:

* Configure an S3 bucket to send `s3:ObjectCreated:*` events to an SQS queue. 
* Configure a Cribl Stream S3 (Pull) Source (not Collector) to subscribe to the SQS feed from step 1.
  * Be sure to properly configure permissions for the Secret/Access keys you use for authentication.
* Set up [Event Breaker](event-breakers) rules as required based on the contents of your log files.

### Office 365 Service, Activity, or Message Trace

Configuring an Office 365 [Services](sources-office-365-services), [Activity](sources-office-365-activity), or [Message Trace](sources-office365-msg-trace) Source is virtually the same for Cribl.Cloud as it is for on-prem Cribl Stream instances. 
Note that you need to [start your Office 365 Content subscription](sources-office-365-activity#start-subscriptions) from within Office 365, or there will be no data available to pull.

## AppScope

[Integrating](https://appscope.dev/docs/logstream-integration/) AppScope with Cribl Stream is simple and fast. The easiest way is to just set the `$SCOPE_CRIBL` environment variable to define a connection between Cribl Stream and AppScope.

For example, you could "scope" the `nginx` command, and send the captured data using TLS over TCP:

`SCOPE_CRIBL=tcp://in.main-default-<organization>.cribl.cloud:10091 scope nginx`
