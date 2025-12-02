# Ports


Cribl Edge requires certain ports to be open, and additional ports are needed if you intend to use specific integrations or options to work.
 
## Leader

In a Distributed deployment, the following ports must be open on the Leader Node.
Ensure that the Leader is reachable on those ports from **all** Edge Nodes. 

Default Port | Protocol | Purpose | Direction
--- | --- | --- | ---
`9000` | HTTP/S | Cribl Edge UI. | In
`9000` | HTTP/S | Bootstrapping Fleets from Leader (on-prem). | In
`443` | HTTP/S | Bootstrapping Fleets from Leader (Cribl.Cloud). | In
`4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on). | In
`4200` | HTTP/S | Software upgrade (via [path](upgrading#package-source), not CDN). | In

## Edge Nodes

The following ports are used by Edge Nodes.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `9420` | TCP | Cribl Edge UI. | In |
| `9000` | HTTP/S | Communication with the Leader for bootstrapping (on-prem). | Out
| `443` | TCP | Communication with the Leader for bootstrapping (hybrid deployment), and with `https://cdn.cribl.io` to download configurations from CDN. | Out |
| `4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on).  | Out |
| `4200` | HTTP/S | Config bundle downloads from the Leader. | Out |

## Other Ports

### Common Ports

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `53` | UDP | DNS lookups. | Out |
| `389` | TCP | LDAP Auth (non-TLS). | Out |
| `443` | TCP | OIDC Auth (TLS). | Out |
| `636` | TCP | LDAP Auth (TLS). | Out |

### Integrations and Apps

Integrations with specific services via Sources and Destinations or apps may require opening dedicated ports on Edge Nodes.

The defaults are listed below, but when configuring each Source or Destination you can choose another port.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `162` | UDP | [SNMP Trap](sources-snmp-traps) collection (non-TLS). The preconfigured SNMP Trap Source listens on port `9162`. | In |
| `162` | UDP | [SNMP Trap](destinations-snmp-traps) Destination (non-TLS). | Out |
| `443` | HTTP/S | Collection from and output to multiple HTTPS-based Sources and Destinations. | In / Out |
| `4317` | TCP | Collection from [OpenTelemetry](sources-otel). | In |
| `5986` | HTTP/S | [Windows Event Forwarder](sources-wef) Source. | In |
| `8081` | TCP | Kafka Schema Registry. | Out |
| `8088` | TCP | [Splunk HEC](sources-splunk-hec) input and output. | In/Out |
| `8089` | TCP | [Splunk Search](sources-splunk-search). | In |
| `8125` | TCP/UDP | Output to [StatsD](destinations-statsd), [StatsD Extended](destinations-statsd-extended), and [Graphite](destinations-graphite) (non-TLS). | Out |
| `9090` | TCP | Collection/discovery from [Prometheus Scraper](/stream/sources-prometheus). | Out |
| `9092` | TCP | Output to [Confluent Cloud](destinations-confluent) or [Kafka](destinations-kafka), used when no port is provided. | Out |
| `9093` | TCP | Output to [Azure Event Hubs](destinations-azure-event-hubs). | Out |
| `9200` | HTTP/S | [Elasticsearch API](sources-elastic) Source. | In |
| `9997` | TCP | [Splunk TCP](sources-splunk) Source. | Out |
| `10070` | TCP | [TCP JSON](sources-tcp-json) data. | In |
| `10080` | TCP | Collection from [HTTP JSON](sources-https) Sources. | In |
| `10200` | HTTP/S | [Cribl HTTP](destinations-cribl-http) Destination. | In |
| `10300` | TCP | [Cribl TCP](destinations-cribl-tcp) Destination. | In |

### Cribl.Cloud

Cribl.Cloud provides a set of ports linked to Sources enabled by default for your Workspace. To view them:

1. From your Cribl.Cloud Organization's top bar, select **Products**.
1. Then from the sidebar, select **Cribl** > **Workspace**, and then **Data Sources**.

Additionally, Cribl.Cloud makes the `20000` – `20010` port range available for configuring other Sources.

| Available Ports | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `20000` – `20010` | TCP | Additional Sources in Cribl.Cloud. | In |

> No other ports can be opened for Cribl-managed Worker Groups in Cribl.Cloud.
{.box .info}

### Cribl Copilot Port

To use Cribl Copilot, your Cribl Edge deployment must be able to establish a connection to `ai.cribl.cloud` on port `443`.
