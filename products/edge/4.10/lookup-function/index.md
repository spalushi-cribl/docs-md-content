# Lookup


The Lookup Function enriches events with external fields, using lookup table files in CSV, compressed `.csv.gz`, or binary `.mmdb` format.

> CSV files are text-based and untyped. This means all data within the file is treated as strings, regardless of its intended type (like, numbers or dates).
>
{.box .info}

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Lookup file path (.csv, .csv.gz)**: Path to the lookup file. Select an existing file that you've uploaded via Cribl Edge's UI at [Knowledge > Lookups Library](lookups-library), or specify the path. You can reference environment variables via `$`, such as `$CRIBL_HOME/file.csv`. 

> When you configure this field via a [distributed deployment](/stream/deploy-distributed)'s Leader Node, Cribl Edge will swap `$CRIBL_HOME/groups/<groupname>/` for `$CRIBL_HOME` when validating whether the file exists. In this case, the default upload path changes from `$CRIBL_HOME/data/lookups` (single-instance deployments) to `$CRIBL_HOME/groups/<groupname>/data/lookups/` (distributed deployments).
>
{.box .info}

**Match mode**: Defines the format of the lookup file, and indicates the matching logic that will be performed. Defaults to `Exact`.

**Match type**: For CIDR and Regex **Match mode**s, this attribute refines how to resolve multiple matches. `First match` will return the first matching entry. `Most specific` will scan all entries, finding the most specific match. `All` will return all matches in the output, as arrays. (Defaults to `First match`. Not displayed for Exact **Match mode**.)

**Lookup fields (.csv)**: Field(s) that should be used to key into the lookup table.
  * **Lookup Field Name in Event**:  Exact field name as it appears in events. Nested addressing supported. 
  * **Corresponding Field Name in Lookup**: The field name as it appears in the lookup file. Defaults to the **Lookup field name in event** value. This input is optional. 

> ##### Case-Sensitive / Multiple Matches
>
> Lookups are case-sensitive by default. (See the **Ignore case** option below.)
> 
> If the lookup file contains duplicate key names with different values, all **Match mode**s of this Function will use **only** the value in the key's **final** instance, ignoring all preceding instances.
>
{.box .warning}

**Output field(s)**:  Field(s) to add to events after matching the lookup table. Defaults to **all** if not specified.
  * **Output Field Name from Lookup**:  Field name, as it appears in the lookup file.
  * **Lookup Field Name in Event**: Field name to add to event. Defaults to the lookup field name. This input is optional. Nested addressing is supported.
  * **Default Value**: Optional string value to assign to the field when the lookup entry is not found.

### Advanced Settings

**Reload period (sec)**: To periodically check the underlying file for mod-time changes, and reload the file if necessary, change the default `-1` value (disabled) to a positive integer representing the check interval in seconds.

> In distributed deployments, enabling a **Reload period** can generate conflicts with configuration updates, causing Pipelines to skip executing some Lookup Functions. Cribl recommends that you enable it only for lookup files not managed by Cribl Edge's UI, or lookup files that can be updated by an external process (such as a threat list that you update via a cron job).
> 
> For lookup files that **are** managed by Cribl Edge's UI, a distributed Cribl Edge deployment will override this setting as necessary – skipping checks to prevent conflicts that could trigger skipped lookups. These restrictions do **not** apply to single-instance deployments.
>
{.box .warning}

**Ignore case**: Ignore case when performing **Match mode: Exact** lookups. Default is toggled off.

**Add to raw event**: Whether to append the looked-up values to the `_raw` field, as key=value pairs. Default is toggled off.

## Examples {#examples}

### Example 1: Regex Lookups

Assign a `sourcetype` field to events if their `_raw` field matches a particular regex.

``` {title="paloalto.csv"}
regex,sourcetype
"^[^,]+,[^,]+,[^,]+,THREAT",pan:threat
"^[^,]+,[^,]+,[^,]+,TRAFFIC",pan:traffic
"^[^,]+,[^,]+,[^,]+,SYSTEM",pan:system
```

**Match mode**: Regex

**Match type**:  First match

> When using the Lookup Function with Regex and `First match`, ensure that your lookup file contains no empty lines - not even at the bottom. Any empty row will cause the function to always return `true`.
>
{.box .info}

**Lookup field name in event**: `_raw`

**Corresponding field name in lookup**: `regex`

``` {title="Events before and after"}
### BEFORE:

{"_raw": "Sep 20 13:03:55 PA-VM 1,2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0"}
{"_raw": "Sep 20 13:03:55 PA-VM 1,2018/09/20 13:03:58,FOOBAR,THREAT,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0"}


### AFTER:

{"_raw": "Sep 20 13:03:55 PA-VM 1,2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0",
  "sourcetype": "pan:traffic"
  }
{"_raw": "Sep 20 13:03:55 PA-VM 1,2018/09/20 13:03:58,FOOBAR,THREAT,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0"
  "sourcetype": "pan:threat"
  }
```

### Example 2: CIDR Lookups

Assign a `location` field to events if their `destination_ip` field matches a particular CIDR range.

``` {title="paloaltoips.csv"}
range,location
10.0.0.0/24,San Francisco
10.0.0.0/16,California
10.0.0.0/8,US
```

**Match mode**: CIDR

**Match type**:  See options below

**Lookup field name in event**: `destination_ip`

**Corresponding field name in lookup**: `range` 

> In **Match mode: CIDR** with **Match type: Most specific**, the lookup will implicitly search for matches from most specific to least specific. There is no need to pre-sort data.
> 
> Note that **Match mode: CIDR** with **Match type: First Match** is likely the most performant with large lookups. This can be used as an alternative to **Most specific**, if the file is sorted with the most specific/relevant entries first. This mode still performs a table scan, top to bottom.
>
{.box .info}

``` {title="Events before and after"}
### BEFORE:

{"_raw": "Sep 20 13:03:55 PA-VM 1, 2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0",
  "destination_ip": "10.0.0.102"
  }
  
### AFTER with Match Type: First Match
 
{"_raw": "Sep 20 13:03:55 PA-VM 1, 2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0",
  "destination_ip": "10.0.0.102",
  "location": "San Francisco"
  }
  
### AFTER with Match Type: Most Specific
 
{"_raw": "Sep 20 13:03:55 PA-VM 1, 2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0",
  "destination_ip": "10.0.0.102",
  "location": "San Francisco"
  }

### AFTER with Match Type: All
 
{"_raw": "Sep 20 13:03:55 PA-VM 1, 2018/09/20 13:03:58,FOOBAR,TRAFFIC,end,2049,2018/09/20 13:03:58,34.217.108.226,10.0.0.102,34.217.108.226,10.0.2.65,splunk,,,incomplete,vsys1,untrusted,trusted,ethernet1/3,ethernet1/2,log-forwarding-default,2018/09/20 13:03:58,574326,1,53722,8088,53722,8088,0x400064,tcp,allow,296,296,0,4,2018/09/20 13:03:45,7,any,0,730277,0x0,United States,10.0.0.0-10.255.255.255,0,4,0,aged-out,0,0,0,0,,PA-VM,from-policy,,,0,,0,,N/A,0,0,0,0",
  "destination_ip": "10.0.0.102",
  "location": [
    "San Francisco",
    "California",
    "US",
  ]}
```

## More Examples and Scenarios {#more-examples}

For usage examples, see these Better Practices topics and other resources:

- [Ingest-time Lookups](/stream/usecase-ingest-time-lookups/): Enriching data in motion for fast access and on-demand accuracy.
- [Lookups and Regex Magic](/stream/usecase-lookups-regex/): Combining the Regex Extract, Lookup, and Eval Functions to enrich events with region, index, and sourcetype data.
- [Lookups as Filters for Masks](/stream/usecase-lookups-filters/): Using lookups to optimize multiple Mask Functions in a single Pipeline.
- [Managing Large Lookups](/stream/large-lookups) to optimize file locations for large lookup files.
- [Deploy Updated Lookup Files to Multiple Packs](/stream/usecase-update-lookups): Efficiently manage frequent updates to a large lookup file.
- [Deploy Updated Lookup Files](/api/create-update-lookups/): Use the Cribl API to refresh a lookup file, and commit and deploy it to Worker Groups.
- [Lookup Examples Pack](https://packs.cribl.io/packs/cribl_Lookup_Examples): Examples of using the Lookup Function and C.Lookup Expressions.

## See Also {#see-also}

- [C.Lookup Expressions](expressions-lookup): Lookup methods that you can use in JavaScript expressions.

- [Redis Function](redis-function) for faster lookups using a Redis integration.

- The [Cribl Knowledge Pack](https://packs.cribl.io/packs/cribl-knowledge-learning-pack) demonstrates using a Lookup Function to drop Cisco ASA events, or to return a regex for extracting fields.
