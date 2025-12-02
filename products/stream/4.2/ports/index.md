# Ports


Cribl Stream requires certain ports to be open, and additional ports are needed if you intend to use specific integrations or options to work.

## Leader

In a Distributed deployment, the following ports must be open on the Leader Node.
Ensure that the Leader is reachable on those ports from **all** Workers. 

Default Port | Protocol | Purpose | Direction
--- | --- | --- | ---
`9000` |  HTTP/S | Cribl Stream UI. | In
`9000` | HTTP/S | Bootstrapping Worker Nodes from Leader (on-prem). | In
`443` | HTTP/S | Bootstrapping Worker Nodes from Leader (Cribl.Cloud). | In
`4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on). | In
`4200` | HTTP/S | Software upgrade (via [path](upgrading#package-source), not CDN). | In

> Cluster communication cannot go through proxies because this communication is not HTTP-based. 
>
{.box .info}

## Workers 

The following ports are used by Worker Nodes.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `9000` | TCP | Cribl Stream UI. | In |
| `4200` | HTTP/S | Config bundle downloads. | Out |
| `4200` | TCP | Heartbeat/Metrics/Leader requests/notifications to clients (for example: live captures, teleporting, status updates, config bundle notifications, and so on).  | Out |
| `443` | TCP | (In Cribl.Cloud hybrid deployments) Communication with the Leader and with `https://cdn.cribl.io`. | Out |

## Other Ports

### Common Ports

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `53` | UDP | DNS lookups. | Out |
| `389` | TCP | LDAP Auth (non-TLS). | Out |
| `443` | TCP | OIDC Auth (TLS). | Out |
| `636` | TCP | LDAP Auth (TLS). | Out |

### Integrations and Apps

Integrations with specific services via Sources and Destinations or apps may require opening dedicated ports on Worker Nodes.

The defaults are listed below, but when configuring each Source or Destination you can choose another port.

| Default Port | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `162` | UDP | [SNMP Trap](sources-snmp-traps) collection (non-TLS). The preconfigured SNMP Trap Source listens on port `9162`. | In |
| `162` | UDP | [SNMP Trap](destinations-snmp-traps) Destination (non-TLS). | Out |
| `443` | HTTP/S | Collection from and output to multiple HTTPS-based Sources and Destinations. | In / Out |
| `4317` | TCP | Collection from [OpenTelemetry](sources-otel). | In |
| `5986` | HTTP/S | [Windows Event Forwarder](sources-wef) Source. | In |
| `8081` | TCP | Kafka Schema Registry. | Out |
| `8088` | TCP | [Splunk HEC](sources-splunk-hec) output. | Out |
| `8089` | TCP | [Splunk Search](sources-splunk-search). | In |
| `8125` | TCP/UDP | Output to [StatsD](destinations-statsd), [StatsD Extended](destinations-statsd-extended), and [Graphite](destinations-graphite) (non-TLS). | Out |
| `9090` | TCP | Collection / discovery from [Prometheus Scraper](sources-prometheus). | Out |
| `9092` | TCP | Collection from [Confluent Cloud](sources-confluent) or [Kafka](sources-kafka), used when no port is provided. | Out |
| `9092` | TCP | Output to [Confluent Cloud](destinations-confluent) or [Kafka](destinations-kafka), used when no port is provided. | Out |
| `9093` | TCP | Output to [Azure Event Hubs](destinations-azure-event-hubs). | Out |
| `9997` | TCP | [Splunk TCP](sources-splunk) Source. | Out |
| `10000` | | Splunk to Cribl Stream data port (Cribl App for Splunk). | In/Out |
| `10200` | HTTP/S | [Cribl HTTP](destinations-cribl-http) Destination. | In |
| `10300` | TCP | [Cribl TCP](destinations-cribl-tcp) Destination | In |
| `10080` | TCP | Collection from [HTTP JSON](sources-https) Sources. | In |
| `10420` | | `\| criblstream` Splunk search command to Cribl Stream (Cribl App for Splunk). | In/Out |

### Cribl.Cloud

Cribl.Cloud makes the `20000` – `20010` port range available for configuring additional Sources.

| Available Ports | Protocol | Purpose | Direction |
| --- | --- | --- | --- |
| `20000` – `20010` | TCP | Additional Sources in Cribl.Cloud. | In |

### Overriding Default Ports

You can override the Cribl Stream UI port (`9000`), as well as other settings,
in the `$CRIBL_HOME/local/cribl/cribl.yml` [configuration file](configuration-files).

The defaults are stored in `$CRIBL_HOME/default/cribl/cribl.yml`.
