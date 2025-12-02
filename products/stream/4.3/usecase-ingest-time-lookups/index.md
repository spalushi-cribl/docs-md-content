# Ingest-Time Lookups


## Enriching Data in Motion

To enrich events with new fields from external sources (such as `.csv` files), we use Cribl Stream's out-of-the-box [Lookup Function](lookup-function). Ingestion-time lookups are not only great for normalizing field names and values, but also ideal for use cases where: 

* Fast access via the looked-up value is required. For example, when you don't have a `datacenter` field in your events, but you do have a `host-to-datacenter` map, and you need to search by `datacenter`.

* Looked-up information must be temporally correct. For example, assume that you have a highly dynamic infrastructure, and you need to resolve a resource name (e.g., a container name) to its address. You can't afford to defer this to search time/runtime, as the resource and its records might no longer exist.

> To use large binary databases (like GeoIP `.mmdb` files) for Cribl Stream lookups, see [Managing Large Lookups](large-lookups). To achieve faster lookups, use Cribl Stream's [Redis](redis-function) Function.
>
{.box .info}

## Working with Lookups – Example 1

Let's assume we have the following lookup file. Given the field `conn_state` in an event, we would like to add a corresponding ingestion-time field called `action`.

``` {title="bro_conn_state.csv"}
action,"conn_state","conn_state_meaning"
dropped,S0,"Connection attempt seen, no reply."
allowed,S1,"Connection established, not terminated."
allowed,SF,"Normal establishment and termination."
blocked,REJ,"Connection attempt rejected."
allowed,S2,"Connection established and close attempt by originator seen (but no reply from responder)."
allowed,S3,"Connection established and close attempt by responder seen (but no reply from originator)."
allowed,RSTO,"Connection established, originator aborted (sent a RST)."
allowed,RSTR,"Established, responder aborted."
dropped,RSTOS0,"Originator sent a SYN followed by a RST, we never saw a SYN-ACK from the responder."
dropped,RSTRH,"Responder sent a SYN ACK followed by a RST, we never saw a SYN from the (purported) originator."
dropped,SH,"Originator sent a SYN followed by a FIN, we never saw a SYN ACK from the responder (hence the connection was 'half' open)."
dropped,SHR,"Responder sent a SYN ACK followed by a FIN, we never saw a SYN from the originator."
allowed,OTH,"No SYN seen, just midstream traffic (a 'partial connection' that was not later closed)."
```

First, make sure you have a Route and Pipeline configured to match desired events.

Next, let's add a **Lookup** function to the Pipeline, with these settings: 

  - **Lookup file path**: `$CRIBL_HOME/data/lookups/bro_conn_state.csv`
(note that Environment variables are allowed in the path).
  - **Lookup Field Name in Event** set to `conn_state`.
  - **Corresponding Field Name in Lookup** set to `conn_state`.
  - **Output Field Name from Lookup** set to `action`.
  - **Lookup Field Name in Event** set to `action`.

![Lookup Function to add `action` field](bro-lookup.png)
{border="true"}

To confirm success, verify that this search returns expected results: `sourcetype="bro" action::allowed`. Change the `action` value as necessary.


## Working with Lookups – Example 2

Let's assume we have the following lookup file, and given **both** the fields `impact` and `priority` in an event, we would like to add a corresponding ingestion-time field called `severity`.

``` {title="cisco_sourcefire_severity.csv"}
impact,priority,severity
1,high,critical
2,high,critical
3,high,high
4,high,high
0,high,high
"*",high,high
.....
"*",medium,medium
1,low,medium
2,low,medium
3,low,low
4,low,low
0,low,low
"*",low,low
1,none,low
2,none,low
3,none,informational
4,none,informational
0,none,informational
"*",none,informational
```

First, make sure you have a Route and Pipeline configured to match desired events.

Next, let's add a **Lookup** function to the Pipeline, with these settings: 

- **Lookup file path**: `$SPLUNK_HOME/etc/apps/Splunk_TA_sourcefire/lookups/cisco_sourcefire_severity.csv `
(note that Environment variables are allowed in the path).
- **Lookup Field Name(s) in Event** set to `impact` and `priority`.
- **Corresponding Field Name(s) in Lookup** set to `impact` and `priority`.
- **Output Field Name from Lookup** set to `severity`.
- **Lookup Field Name in Event** set to `severity`.

![Lookup Function to add `severity` field](sourcefire-lookup.0fc32c9740.png)
{border="true"}

To confirm success, verify that this search returns expected results: `sourcetype="cisco:sourcefire" severity::medium`. Change the `severity` value as necessary.
