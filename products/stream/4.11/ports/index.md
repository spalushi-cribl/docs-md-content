# Ports


Cribl Stream requires certain ports to be open, and additional ports are needed if you intend to use specific integrations or options to work. 

## Leader

In a Distributed deployment, the following ports must be open on the Leader Node.
Ensure that the Leader is reachable on those ports from **all** Workers. 

Default Port | Protocol | Purpose | Direction
--- | --- | --- | ---
`9000` | HTTP/S  | Cribl Stream UI. | In
`9000` | HTTP/S | Bootstrapping Worker Nodes from Leader (on-prem). | In
`443` | HTTP/S | Bootstrapping Worker Nodes from Leader (Cribl.Cloud). | In
`4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on). | In
`4200` | HTTP/S | Software upgrade (via [path](upgrading#package-source), not CDN). | In

> If you want to use proxy for communication between Leader and Worker Nodes, use SOCKS proxy instead of HTTP/S.
> You need to use SOCKS proxy, because HTTP/S proxies typically don't support raw TCP sockets that Leader-Worker communication uses.
{.box .info}

## Workers

The following ports are used by Worker Nodes.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `9000` | TCP | Cribl Stream UI. | In |
| `9000` | HTTP/S | Communication with the Leader for bootstrapping (on-prem). | Out
| `443` | HTTP/S | Communication with the Leader for bootstrapping (hybrid deployment), and with `https://cdn.cribl.io` to download configurations from CDN. | Out |
| `4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on).  | Out |
| `4200` | HTTP/S | Config bundle downloads from the Leader. | Out |

## Other Ports

This section lists port allocations for specific transport protocols and other special purposes.

### Common Ports

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `53` | UDP | DNS lookups. | Out |
| `389` | TCP | LDAP Auth (non-TLS). | Out |
| `443` | TCP | OIDC Auth (TLS); and [Cribl Lake Destination](/stream/destinations-cribl-lake) on [hybrid](cloud-enterprise#hybrid) Worker Groups that you manage. | Out |
| `636` | TCP | LDAP Auth (TLS). | Out |
| `9002` | TCP | Browser access to the identity server when using [Personal Identity Verification (PIV)](authentication#piv) authentication. | In |

### Integrations and Apps

Integrations with specific services, via Sources and Destinations or apps, might require opening dedicated ports on Worker Nodes.

The defaults are listed below. When configuring most Sources or Destinations, you can choose a different port. However, on [hybrid](cloud-enterprise#hybrid) Worker Groups that you manage, the [Cribl Lake Destination](/stream/destinations-cribl-lake) is hard-coded to send outbound HTTP/S traffic through port `443`.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `162` | UDP | [SNMP Trap](sources-snmp-traps) collection (non-TLS). The preconfigured SNMP Trap Source listens on port `9162`. | In |
| `162` | UDP | [SNMP Trap](destinations-snmp-traps) Destination (non-TLS). | Out |
| `443` | HTTP/S | Collection from and output to multiple HTTPS-based Sources and Destinations. | In/Out |
| `4317` | TCP | Collection from [OpenTelemetry](sources-otel). | In |
| `5986` | HTTP/S | [Windows Event Forwarder](sources-wef) Source. | In |
| `8081` | TCP | Kafka Schema Registry. | Out |
| `8088` | TCP | [Splunk HEC](sources-splunk-hec) input and output. | In/Out |
| `8089` | TCP | [Splunk Search](sources-splunk-search). | In |
| `8125` | TCP/UDP | Output to [StatsD](destinations-statsd), [StatsD Extended](destinations-statsd-extended), and [Graphite](destinations-graphite) (non-TLS). | Out |
| `9090` | TCP | Collection/discovery from [Prometheus Scraper](sources-prometheus). | Out |
| `9092` | TCP | Collection from [Confluent Cloud](sources-confluent) or [Kafka](sources-kafka), used when no port is provided. | Out |
| `9092` | TCP | Output to [Confluent Cloud](destinations-confluent) or [Kafka](destinations-kafka), used when no port is provided. | Out |
| `9093` | TCP | Output to [Azure Event Hubs](destinations-azure-event-hubs). | Out |
| `9200` | HTTP/S | [Elasticsearch API](sources-elastic) Source. | In |
| `9997` | TCP | [Splunk TCP](sources-splunk) Source. | Out |
| `10000` | | Splunk to Cribl Stream data port (Cribl App for Splunk). | In/Out |
| `10070` | TCP | [TCP JSON](sources-tcp-json) data. | In |
| `10080` | TCP | Collection from [HTTP JSON](sources-https) Sources. | In |
| `10200` | HTTP/S | [Cribl HTTP](destinations-cribl-http) Destination. | In |
| `10300` | TCP | [Cribl TCP](destinations-cribl-tcp) Destination. | In |
| `10420` | | `\| criblstream` Splunk search command to Cribl Stream (Cribl App for Splunk). | In/Out |

### Cribl.Cloud Ports {#cloud-ports}

Cribl.Cloud provides a set of ports linked to Sources enabled by default for your Workspace. To view them:

1. From your Cribl.Cloud Organization's top bar, select **Products**.
1. Then from the sidebar, select **Cribl** > **Workspace**, and then **Data Sources**.

Additionally, Cribl.Cloud makes the `20000` – `20010` port range available for configuring other Sources.

| Available Ports | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `20000` – `20010` | TCP | Additional Sources in Cribl.Cloud. | In |

> No other ports can be opened for Cribl-managed Worker Groups in Cribl.Cloud.
{.box .info}

### Overriding Default Ports

You can override the Cribl Stream UI port (`9000`), as well as other settings,
in the `$CRIBL_HOME/local/cribl/cribl.yml` [configuration file](configuration-files).

The defaults are stored in `$CRIBL_HOME/default/cribl/cribl.yml`.
