# Multiple Agents Reference Architecture


This reference architecture shows one Cribl Stream deployment possibility for ingesting data from a large quantity of agents. Here, a single basic configuration is replicated across multiple Worker Groups, organized by geographic location.

![Multiple agents reference architecture](ref-arch-many-agents.614fd9ff34.png)
{border="true"}

> Download an editable version of this diagram in [SVG](/downloads/ref-arch-many-agents.svg) or [draw.io](/downloads/ref-arch-many-agents.drawio) format. Download Cribl stencils [here](https://assets.ctfassets.net/xnqwd8kotbaj/2OnWVALZxgnpdWhiTV40Ey/c68757ee18aa7ef411e9ef12a5fd0db7/Cribl-Stencil-Pack-ALL.zip).
>
{.box .success}

## Single or Multiple Worker Groups?

The multiple-Groups architecture shown here is one approach. It might or might not be desirable for your use case. 

Pros of using multiple Worker Groups:
- Configuration granularity.
- Can use [Packs](packs) to export/reuse configurations across Groups.

Cons of using multiple Worker Groups:
- Multiple configurations to maintain (even if shared via Packs).

## General Sizing Considerations

Size based on data throughput (in+out), and on number of incoming TCP connections.

## Sizing Examples

Below are two scenarios demonstrating how to determine the number of Stream Worker Processes you'll need to allocate. Both scenarios assume the same distribution of chatty versus medium-chatty agents, but with a different balance of throughput volume versus agents volume.

### How Many Events per Second?

We assume three tiers of "chatty" agents, based on events per second. You'll probably recognize your senders from these definitions:
- Chatty agents (100 EPS/agent) – Size 150 agents/vCPU. (Examples are domain controllers or intermediary agents.)
- Medium-chatty agents (30 EPS/agent) – Size 250 agents/vCPU. (Most servers will fall into this medium category.)
- Low-volume agents (3 EPS/agent) – Size 5000 agents/vCPU (Examples are workstations.)

For the most accurate sizing, obtain EPS reports from your current observability tools.

### Using the Scenarios

In each scenario, to size based on the number of agents directly feeding Cribl Stream, take the **largest** vCPU count from the pair of volume versus ports calculations.

### Scenario 1

40 TB/day throughput, 40,000 agents, 10% of them very chatty, 90% medium-chatty.
- Sizing for volume (non-hyperthreaded CPU cores): 205 Worker Processes.
- Sizing from TCP ports (non-hyperthreaded CPU cores): (0.10 x 40,000 / 150) + (0.90 x 40,000 / 250) = (27 + 144) = 171.
- Result: Allocate **205** Worker Processes, the larger of the two.

### Scenario 2

4 TB/day throughput, 20,000 agents, 10% of them very chatty, 90% medium-chatty.
- Sizing from vCPUs (non hyperthreaded CPU Cores): 11 Worker Processes.
- Sizing from TCP ports (non-hyperthreaded CPU Cores): (0.10 x 20,000 / 150) + (0.90 * 20,000 / 250) = (14 + 72) = 86.
- Result: Allocate **86** Worker Processes, the larger of the two.

## Elastic Agent Configuration

The configuration to use in `filebeat.yml` depends on your agent's version. 

#### Versions 8.x+

```
setup.ilm.enabled: false
```

#### Versions 7.15 Through 7.17

```yaml
output.elasticsearch:
hosts: ["https://<your‑host‑here>.com:9200/elastic"]
```

Or:

```yaml
output.elasticsearch:
hosts: ["<server-name-here>:9200/elastic", "<another-server-name-here>:9200/elastic"]
```

#### Versions 7.14 and Earlier

```yaml
output.elasticsearch:
hosts: ["https://<your‑host‑here>.com:9200"]
```

Or:

```yaml
output.elasticsearch:
hosts: ["<server-name-here>:9200", "<another-server-name-here>:9200"]
```

Configure the agent's `worker` setting to avoid TCP pinning. See the `worker` configuration option in 
[Configure the Elasticsearch Output](https://www.elastic.co/guide/en/beats/winlogbeat/current/elasticsearch-output.html).

## Splunk Forwarder Configuration

Here are configurations for a few different scenarios.

> Setting `blockOnCloning` to `false` ensures that if either Cribl or Splunk becomes unavailable, the other system will continue receiving data without dropping events, preventing potential data loss or interruptions in transmission.
>
{.box .info}

#### Send All Data to Cribl

`outputs.conf` to send to both Splunk and Cribl: 

```
[tcpout]
defaultGroup=splunk,stream
blockOnCloning=false

[tcpout:splunk]
server=10.1.1.197:9997,10.1.1.198:9997
blockOnCloning=false

[tcpout:stream]
server=10.1.1.200:9997,10.1.1.201:9997
sendCookedData=true
blockOnCloning=false
```

`outputs.conf` to send only to Cribl:

```
[tcpout]
defaultGroup=stream
blockOnCloning=false

[tcpout:stream]
server=10.1.1.200:9997,10.1.1.201:9997
sendCookedData=true
blockOnCloning=false
```

#### Send All Data (Minus Splunk Internal Logs) to Cribl

`inputs.conf` for internal logs:

```
# datasource1.log only gets sent to Splunk

[monitor:/path/to/log/datasource1.log]
_TCP_ROUTING = splunk
index = ds1_index
sourcetype = sourcetype1
```

`inputs.conf` for all other logs:

```
# datasource2.log will get sent to both Splunk and Stream

[monitor:/path/to/log/datasource2.log]
_TCP_ROUTING = splunk, stream
index = ds2_index
sourcetype = sourcetype2
```

`outputs.conf` for internal logs:

```
[tcpout] 
defaultGroup=splunk,stream
blockOnCloning=false

[tcpout:splunk]
server=10.1.1.197:9997,10.1.1.198:9997
blockOnCloning=false

[tcpout:stream]
server=10.1.1.200:9997,10.1.1.201:9997
sendCookedData=true
blockOnCloning=false
```

## Fluent Bit Configuration

Here are sample configurations for three different ways to ingest Fluent Bit data.

#### Splunk HEC Source on Cribl Workers

[Fluent Bit reference](https://docs.fluentbit.io/manual/pipeline/outputs/splunk) 

```
[OUTPUT]
Name cribl
Match *
host <HOST>
port 8088
Cribl_HEC_Token xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
Cribl_HEC_Send_Raw On #optional: to raw instead of event endpoint
TLS On
TLS.Verify Off #optional
```

#### Raw HTTP Source on Cribl Workers

[Fluent Bit reference](https://docs.fluentbit.io/manual/pipeline/outputs/http) 

```
[OUTPUT]
Name http
Match *
Host cribl_worker_load_balanced_vip
Port 10443
URI /optional/uri/for/filtering/in/cribl
Format json_lines
Header Authorization <Cribl-generated token> 
Header X-Key-B Value_B
```

#### TCP JSON Source on Cribl Workers

[Fluent Bit reference](https://docs.fluentbit.io/manual/pipeline/outputs/tcp-and-tls) 

```
[OUTPUT]
Name tcp
Format json_lines
Workers 2
tls on #optional for tls setting
json_date_key date
json_date_format epoch

# Note: Use mTLS, because this plugin doesn't appear to support authTokens
```

## Cribl Destination Considerations

Adjust Cribl Destinations' **Advanced Settings** > **Max connections** setting (where available) to limit the load on receivers. E.g.: For a [Splunk Load Balanced ](destinations-splunk-lb)Destination, if you have 5 Worker Groups sending data to the same Splunk indexer cluster, with 25 indexers, set **Max connections** to between `5`-`10`. 

## Disclaimers

All product names, logos, brands, trademarks, registered trademarks, and other marks listed in this document are the property of their respective owners. All such marks are provided for identification and informational purposes only. The use of these marks does not indicate affiliation, endorsement, or ownership of the marks or their respective owners.

This document is provided “as is” with NO EXPRESS OR IMPLIED WARRANTIES OR REPRESENTATIONS. It is intended to aid users in displaying system architecture, but might not be applicable or appropriate in all circumstances. You should not act on any information provided until you have formed your own opinion through investigation and research. Cribl is not responsible for any use of this document or the information provided herein.
