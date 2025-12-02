# Regex Extract


The Regex Extract Function extracts fields using regex named groups. Fields that start with `__` (double underscore) are special in Cribl Stream. They are ephemeral: they can be used by any Function downstream, but will be serialized only to Cribl [internal Destinations](destinations#internal).

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of the Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Regex**: Regex literal. Must contain named capturing groups, e.g.: `(?<foo>bar)`. Can contain special `_NAME_N` and `_VALUE_N` capturing groups, which extract **both the name and value** of a field, e.g.: `(?<_NAME_0>[^\s=]+)=(?<_VALUE_0>[^\s]+)`. Defaults to empty. See [Examples](#examples) below.

**Additional regex**:  Click **Add Regex** to chain extra regex conditions.

**Source field**: Field on which to perform regex field extraction. Nested addressing is supported. Defaults to `_raw`. 

### Advanced Settings

**Max exec**: The maximum number of times to apply the **Regex** to the source field when the global flag is set, or when using `_NAME_N` and `_VALUE_N` capturing groups. Named capturing groups will always use a value of `1`. Defaults to `100`.

**Field name format expression**: JavaScript expression to format field names when `_NAME_n` and `_VALUE_n` capturing groups are used. E.g., to append `XX` to all field names, use: ``` `${name}_XX` ``` (backticks are literal). If not specified, names will be sanitized using regex: `/^[_0-9]+|[^a-zA-Z0-9_]+/g`. The **original** field name is in the global `name`.  You can access other fields' values via `__e.<fieldName>`.

**Overwrite existing fields**: Whether to overwrite existing event fields with extracted values. If set to `No` (the default), existing fields will be converted to an array. If toggled to `Yes`, Regex Extract will create array fields if applied multiple times, or if fields exist. (E.g., if `src_ip` is extracted in an input Pipeline where it is assigned a value of `10.1.2.2`, and is also in a processing Pipeline with a value of `10.2.3.3`, then the resulting field is `["10.1.2.2", "10.2.3.3"]`.)

## Examples

### Example 1: Single Field from Simple Event

Assume a simple event that looks like this: `metric1=23 metric2=42 dc=23 abc=xyz`

Extract **only** the `metric1` field: 

**Regex**: `metric1=(?<metric1>\d+)`
**Result**: `metric1:"23"` 

### Example 2: Key‑Value Pairs from Multiple Fields {#example-2}

Use this sample:

```
rec_type=71 rec_type_simple=RNA dest_port=443 snmp_out=0 netflow_src="00000000-0000-0000-0000-000000000000" ssl_server_cert_status="Not Checked" dest_ip=172.20.115.42 sec_intel_event=No mac_address=00:00:00:00:00:00 dest_bytes=3746 dest_autonomous_system=0 security_context=00000000000000000000000000000000 src_port=41925 web_app=Unknown url=https://outlook.ssg.petsmart.com url_reputation="Risk unknown" first_pkt_sec=1543598207 vlan_id=0 ssl_flow_error=0 ssl_actual_action=Unknown has_ipv6=1 monitor_rule_6=N/A monitor_rule_7=N/A monitor_rule_4=N/A monitor_rule_5=N/A monitor_rule_2=N/A monitor_rule_3=N/A ips_count=0 monitor_rule_1=N/A dest_tos=0 src_ip=192.168.228.5 referenced_host="" iface_ingress=DMZ3.30 monitor_rule_8=0 event_subtype=1 fw_rule_reason=N/A event_type=1003 ssl_version=Unknown dns_resp_id=0 sensor=ssg-inet-fpr-ftd-fw01 sec_zone_egress=Inside src_tos=0 client_app="SSL client" snmp_in=0 user=Unknown ssl_flow_messages=0 iface_egress=inside http_referrer="" src_pkts=0 event_desc="Flow Statistics" event_usec=0 client_version="" fw_rule_action=Allow ssl_cert_fingerprint=0000000000000000000000000000000000000000 ssl_url_category=0 file_count=0 sec_zone_ingress=DMZ3 instance_id=6 src_bytes=1013 src_ip_country=unknown ssl_cipher_suite=TLS_NULL_WITH_NULL_NULL user_agent="" http_response=0 src_mask=0 dest_mask=0 sec_intel_ip=N/A netbios_domain="" tcp_flags=0 dns_rec_id=0 fw_policy="SSG INET Access Control Policy" last_pkt_sec=1543598207 legacy_ip_address=0.0.0.0 ip_proto=TCP connection_id=21378 dest_pkts=0 app_proto=HTTPS ssl_flow_status=Unknown ssl_rule_id=0 ssl_session_id=0000000000000000000000000000000000000000000000000000000000000000 dns_query="" rec_type_desc="Connection Statistics" url_category=Unknown fw_rule="Outbound Web" src_autonomous_system=0 ssl_flow_flags=0 ip_layer=0 event_sec=1543598205 ssl_ticket_id=0000000000000000000000000000000000000000 sinkhole_uuid=00000000-0000-0000-0000-000000000000 dest_ip_country=unknown ssl_expected_action=Unknown num_ioc=0 dns_ttl=0 ssl_policy_id=00000000000000000000000000000000 ssl_server_name=""
```

Use a regex to extract **all** k=v pairs, then use **Field Name Format Expression** to append an `_XX` suffix to each extracted field:

**Regex**: `(?<_NAME_0>[\w-]+)="?(?<_VALUE_0>(?<=")[^"]*|\S*)`
**Field Name Format Expression**: `${name}_XX`



**Results**:

![Example 2 results](ex-2-final-redacted.cb06add50c.png)

### Example 3: Multi‑Stage Extraction, Complex Events

This example builds on the syntax in [Example 2](#example-2), to tackle a more complex event structure. 

In the right **Sample Data** pane, click **Paste** and insert the following sample:

``` {title="Sample Data"}
<134>1 2020-12-22T17:06:08Z CORP_INT_NLB CheckPoint 18160 - [action:"Accept"; conn_direction:"Internal"; flags:"4606212"; ifdir:"inbound"; ifname:"bond2.1025"; logid:"0"; loguid:"{0x5fe25889,0x0,0x80ad57cd,0xeb91c0c3}"; origin:"192.168.20.54"; originsicname:"CN=TST32-VSX0-FW-DC-01_tst302-shd,O=CORP-SEC-SHRD-CMA..t7xpcz"; sequencenum:"3"; time:"1608656768"; version:"5"; __policy_id_tag:"product=VPN-1 & FireWall-1[db_tag={15E4B45A-663B-5B49-BD59-CD9B9F21AA16};mgmt=SHRDFW01CON;date=1608236862;policy_name=TEST-SHRD-POL\]"; dst:"192.168.79.20"; log_delay:"1608656768"; layer_name:”TEST-SHRD-POL Security"; layer_uuid:"e914c2f3-d7bd-4a77-8e7a-7a5e403447aa"; match_id:"1"; parent_rule:"0"; rule_action:"Accept"; rule_uid:"001ab86d-d201-4b61-9b64-0fede1a9f059"; product:"VPN-1 & FireWall-1"; proto:"17"; s_port:"45519"; service:"123"; service_id:"ntp-udp"; src:"192.168.79.22"; ]
```

This event is from a CheckPoint Firewall CMA system. With this type of event structure,  properly extracting each event field into a separate metadata field requires two-stage processing. So we'll use two Regex Extract Functions. 

The first Regex Function splits the event to separate the actual data from the header information. We'll split after the `CheckPoint 18160` string, by capturing everything between the `[` and `]`:

**Regex**: `\[(?<__fields>.*)\]`
**Source**: `_raw`

Next, add this second Regex Extract Function to extract all k=v pairs:

**Regex**: `(?<_NAME_0>[^ :]+):(?<_VALUE_0>[^;]+);` 
**Source**: `__fields`

**Results:**

![Example 3 results](3176197-regex_extract_result_checkpoint-fix.a97d61d314.png)



> For further examples, see [Using Cribl to Analyze DNS Logs in Real Time – Part 2](https://cribl.io/blog/using-cribl-to-analyze-dns-logs-in-real-time-part-2).
>
{.box .info}
