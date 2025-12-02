# Built-In Cribl Edge Datasets


Learn about built-in Cribl Edge Datasets available in Cribl Search.

---

Cribl Search ships with built-in Cribl Edge [Datasets](basic-concepts#datasets) so you can quickly search  internal logs and metrics for your Edge Nodes. Using these Datasets, you can find comprehensive information about an instance's
status/health, inputs, outputs, Pipelines, Routes, Functions, and traffic. You can edit these built-in Datasets, or create new ones to specify other logs anywhere in the filesystem that Edge can read.

Cribl Edge comes out-of-the-box with these Datasets:

- `cribl_edge_logs`
- `cribl_edge_state`
- `cribl_edge_metrics`
- `cribl_edge_system_logs`
- `cribl_edge_spool`
- `cribl_edge_appscope_events`
- `cribl_edge_appscope metrics`

## Details of Built-In Cribl Edge Datasets {#datasets}

Below are the type of data each Dataset returns, along with some event samples: 

<table style={{width: "100%"}}>
<thead>
<tr>
<th>Dataset Name</th>
<th>Type of Data</th>
<th>Event Sample</th>
</tr>
</thead>
<tr>
<td>
<code>cribl_edge_logs</code>
</td>
<td>Cribl Edge provides API server logs, Edge Node process(es), and Fleet logs. For details, see <a href="/edge/monitoring#types">Types of Logs</a> and <a href="/edge/internal-logs">Internal Logs</a>.
</td>
<td>

```json
{
"dataset": "cribl_edge_logs",
"source": "file:///opt/cribl/log/cribl.log",
"host": "1e7a817de3be",
"_raw": "{\"time\":\"2023-04-19T10:19:15.559Z\",\"cid\":\"api\",\"channel\":\"MetricsStore\",\"level\":\"info\",\"message\":\"active metrics\",\"numMetrics\":37}",
"time": "2023-04-19T10:19:15.559Z",
"cid": "api",
"channel": "MetricsStore",
"level": "info",
"message": "active metrics",
"numMetrics": 37,
"_time": 1681899555.559,
"datatype": "cribl_json",
"cribl_taskId": "1681903132338.sOY7mn",
"cribl_guid": "a315e568-cecb-45de-a53e-2075dbbcbdf8",
"cribl_fleet": "default_fleet"
}
```

</td>
</tr>
<tr>
<td>
<code>cribl_edge_state</code>
</td>
<td>System state provides a snapshot of the host system&#39;s current state, including details on host info, groups, and users. For details, see <a href="/edge/explore-edge-linux#host-metadata">System State</a>.
</td>
<td>

```json
{
"dataset": "cribl_edge_state",
"source": "file:///opt/cribl/state/system_state/1681902600/userGroup/OP172Y.0.json.tmp",
"host": "1e7a817de3be",
"state_source": "userGroup",
"_raw": "{\"_time\":1681902601.362,\"host\":\"1e7a817de3be\",\"name\":\"root\",\"groupId\":0,\"users\":[\"root\"]}",
"_time": 1681902601.362,
"name": "root",
"groupId": 0,
"users": ["root"],
"datatype": "cribl_json",
"cribl_taskId": "1681902737896.8EOlpP",
"cribl_guid": "a315e568-cecb-45de-a53e-2075dbbcbdf8",
"cribl_fleet": "default_fleet"
}
```

</td>
</tr>
<tr>
<td>
<code>cribl_edge_metrics</code>
</td>
<td>Cribl Edge collects metrics from the host on which it is running, and can populate some standard metrics Dashboards right out of the box. For details, see <a href="/edge/system-metrics-linux-output">Linux System Metrics Details</a>.
</td>
<td>

```json
{
"dataset": "cribl_edge_metrics",
"source": "file:///opt/cribl/state/system_metrics/1681902000/system/m2qa9B.0.json",
"host": "1e7a817de3be",
"metrics_source": "system",
"_raw": "{\"_time\":1681902001.56,\"host\":\"1e7a817de3be\",\"nodename\":\"1e7a817de3be\",\"release\":\"5.10.104-linuxkit\"\"sysname\":\"Linux\",\"version\":\"#1 SMP PREEMPT Thu Mar 17 17:05:54 UTC 2022\",\"node_uname_info\":1,\"node_cpu_count\":5}",
"_time": 1681902001,
"nodename": "1e7a817de3be",
"release": "5.10.104-linuxkit",
"sysname": "Linux",
"version": "#1 SMP PREEMPT Thu Mar 17 17:05:54 UTC 2022",
"node_uname_info": 1,
"node_cpu_count": 5,
"datatype": "cribl_json",
"cribl_taskId": "1681902797516.BskPaQ",
"cribl_guid": "a315e568-cecb-45de-a53e-2075dbbcbdf8",
"cribl_fleet": "default_fleet"
}
```

</td>
</tr>
<tr>
<td>
<code>cribl_edge_system_logs</code>
</td>
<td>Search through Linux host's OS logs in <code>/var/log</code>. For details, see <a href="/edge/internal-logs">Internal Logs</a>.
</td>
<td>

```json
{
"dataset": "cribl_edge_system_logs",
"source": "file:///var/log/bootstrap.log",
"host": "1e7a817de3be",
"_raw": "gpgv: Signature made Thu Apr 23 17:34:17 2020 UTC",
"_time": 1587663257,
"datatype": "cribl_json",
"cribl_taskId": "1681903034051.Q4gkk4",
"cribl_guid": "a315e568-cecb-45de-a53e-2075dbbcbdf8",
"cribl_fleet": "default_fleet"
}
```

</td>
</tr>
<tbody>
<tr>
<td><code>cribl_edge_spool</code></td>
<td>Search through events stored in Cribl Edge's data spool. For details, see <a href="/edge/destinations-disk-spool">Disk Spool Destination</a> ).
</td>
<td>

```json
{
  "_raw": "{\"_raw\":\"2024-01-31 11:40:12.611689 (gui/502 [100012]) <Notice>: approved lookup: name = com.apple.audio.audiohald, flags = 0x9, requestor = Google Chrome H[2151], error = 0: Success\",...Show more",
  "_time": 1706730012.611,
  "cribl_breaker": "fallback",
  "cribl_fleet": "default_fleet",
  "dataset": "cribl_edge_spool",
  "host": "C4GQHK-zpF3",
  "source": "file:///cribl/dist/ributed/managed-edge0/state/spool/out/disk_spool/disk_spool/1706730000_1706730600/jwq62k.0.json.gz",
  "data__raw": "2024-01-31 11:40:12.611689 (gui/502 [100012]) <Notice>: approved lookup: name = com.apple.audio.audiohald, flags = 0x9, requestor = Google Chrome H[2151], error = 0: Success",
  "data_source": "/private/var/log/com.apple.xpc.launchd/launchd.log",
  "datatype": "cribl_json",
  "output_id": "disk_spool"
}
```
</td>
</tr>
<tr>
<td><code>cribl_edge_appscope_events</code></td>
<td>AppScope emits events for the applications being scoped. For details, see <a href="https://appscope.dev/docs/events-and-metrics#events">What Events Do</a>.
</td>
<td>

```json
{
"_raw": "{\"type\":\"evt\",\"id\":\"ip-10-8-102-86-curl-curl wttr.in/SanFrancisco\",\"_channel\":\"119018778058933\",\"body\":{\"sourcetype\":\"http\",\"_time\":1681767085.239414,\"source\":\"http.req\",\"host\":\"ip-10-8-102-86\",\"proc\":\"curl\",\"cmd\":\"curl wttr.in/SanFrancisco\",\"pid\":653261,\"data\":{\"http_method\":\"GET\",\"http_target\":\"/SanFrancisco\",\"http_flavor\":\"1.1\",\"http_scheme\":\"http\",\"http_host\":\"wttr.in\",\"http_user_agent\":\"curl/7.68.0\",\"net_transport\":\"IP.TCP\",\"net_peer_ip\":\"5.9.243.187\",\"net_peer_port\":80,\"net_host_ip\":\"10.8.102.86\",\"net_host_port\":56622}}}",
"source": "file:///home/ubuntu/.scope/history/curl_56_653261_1681767084718677282/events.json",
"host": "ip-10-8-102-86",
"type": "evt",
"id": "ip-10-8-102-86-curl-curl wttr.in/SanFrancisco",
"_channel": "119018778058933",
"body": {
  "sourcetype": "http",
  "_time": 1681767085.239414,
  "source": "http.req",
  "host": "ip-10-8-102-86",
  "proc": "curl",
  "cmd": "curl wttr.in/SanFrancisco",
  "pid": 653261,
  "data": {
    "http_method": "GET",
    "http_target": "/SanFrancisco",
    "http_flavor": "1.1",
    "http_scheme": "http",
    "http_host": "wttr.in",
    "http_user_agent": "curl/7.68.0",
    "net_transport": "IP.TCP",
    "net_peer_ip": "5.9.243.187",
    "net_peer_port": 80,
    "net_host_ip": "10.8.102.86",
    "net_host_port": 56622
  }
},
"_time": 1681767085.5878348,
"datatype": "cribl_json",
"cribl_fleet": "default_fleet"
}
```

</td>
</tr>
<tr>
<td>
<code>cribl_edge_appscope_metrics</code>
</td>
<td>
AppScope emits metrics for the applications being scoped. For details, see <a href="https://appscope.dev/docs/events-and-metrics#metrics">What Metrics Do</a>.
</td>
<td>

```json
{
"_raw": "{\"type\":\"metric\",\"body\":{\"_metric\":\"http.req\",\"_metric_type\":\"counter\",\"_value\":1,\"http_target\":\"/SanFrancisco\",\"http_status_code\":200,\"proc\":\"curl\",\"pid\":653261,\"host\":\"ip-10-8-102-86\",\"unit\":\"request\",\"summary\":\"true\",\"_time\":1681767085.5897679}}",
"source": "file:///home/ubuntu/.scope/history/curl_56_653261_1681767084718677282/metrics.json",
"host": "ip-10-8-102-86",
"type": "metric",
"body": {
  "_metric": "http.req",
  "_metric_type": "counter",
  "_value": 1,
  "http_target": "/SanFrancisco",
  "http_status_code": 200,
  "proc": "curl",
  "pid": 653261,
  "host": "ip-10-8-102-86",
  "unit": "request",
  "summary": "true",
  "_time": 1681767085.589768
},
"_time": 1681767085.5878348,
"datatype": "cribl_json",
"cribl_fleet": "default_fleet"
}
```
</td>
</tr>
</tbody>
</table>
