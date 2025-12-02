# Lookups and Regex Magic


Regular expressions are not just for field extractions – they can also be used inside lookup tables, and in [Functions](functions), to replace and manipulate values within fields. Let's walk through a [Pipeline](pipelines) that demonstrates four different ways to leverage regular expressions in Cribl Stream.

## Why Lookup Tables Matter

When organizations use host naming standards, it is easy to understand things like regions, availability zones (AZs), IP addresses, and more. For example, consider an Amazon host called: 

`ec2-35-162-133-145.us-west1-a.compute.amazonaws.com` 

This is an EC2 host with a (dashed) IP address `35-162-133-145`, in the `us-west1` region, in Availability Zone `a`.  You can also see the domain: `compute.amazonaws.com`.

While we can understand the enriched host names, we don't know which indexes to route the data to, nor which sourcetypes to assign to the events, without looking up this information from another source. Doing so is often a huge challenge for organizations. To solve this challenge, let's combine [Regex Extract](regex-extract-function), [Lookup](lookup-function), and [Eval](eval-function) Functions with some sample events to demonstrate the power of Cribl Stream.

## Sample Events

The events below have timestamps broken out, but no indexes, sourcetypes, or other details have been assigned yet:

![](QD9uAOO.9598d13cac.png)

## The Regex Extract Function

Before we can assign an index or sourcetype, we need to extract the `host`, `region`, `az`, and `domain` fields from the events. We can use a [Regex Extract](regex-extract-function) Function with this regular expression to extract all four fields: 

`GMT:\s+(?<host>[^.]+)\.(?<region>\w+-\w+\d+)-(?<az>[^.]+)\.(?<domain>[^:]+):`

Here's that Regex Extract in a Cribl Stream Pipeline:

![](BOjtyVg.8efc8c31fd.png)
{border="true"}

#### Results of the Regex Extract Function

On the **OUT** tab of Cribl Stream's Preview pane, the extracted fields of `az`, `domain`, `host`, and `region`  now appear below the `_raw` event. You can use these extracted fields for searching in your preferred search solution.

![](y1Jh1KC.917114cc94.png)

## Lookups

We still need to determine the index and sourcetype. Cribl Stream's [Lookup Function](lookup-function) enriches events with external fields. We'll use it with the newly extracted `region` field to assign an `index` and `sourcetype` to these events.

###  Lookup File  {#lookup_table}

First, we need to [create a lookup table](lookups-library) for the Function to reference. For this, we'll use regex again.

In the table below, five simple regular expressions map the extracted `region` field to the appropriate `index` and `sourcetype`. For example, the region `us-west1-a` starts with `us`, so it matches the first regular expression: `us.+` 

We use this lookup table's first row to assign an index of `usa_index_tier`, and a sourcetype of `cloud-init`, to each matching event. The region patterns in the table's four remaining rows work the same way.

![](xqRmBHE.9935db3fdb.png)

### Lookup Function

With the lookup table saved as `region_index_sourcetype.csv`, the [Lookup Function](lookup-function) below matches the events' extracted `region` field against the regular expressions, and returns the matching `index` and `sourcetype`.

![](opKZo11.6da2af4bfe.png)

There's actually more here than meets the eye. Note that we've specified no **Output Fields**. From the Lookup Function's [documentation](lookup-function), we know this means that the Function will default to outputting **all** fields in the lookup table. So we get the contents of both remaining columns in the table we saw [above](#lookup_table): `index` and `sourcetype`.

#### Results of the Lookup Function

With the Lookup Function added to our Pipeline, the Preview pane's **OUT** tab shows that the `index` and `sourcetype` are now added to each event.

![](00Z9Wko.26760ef28f.png)

## Getting Host IP Address from Host Name

Since the IP address is present in the `host` field, we can create the `host_ip` field using an [Eval Function](eval-function) with this replace method:

`host.replace(/\w+-(\d+)-(\d+)-(\d+)-(\d+)/,'$1.$2.$3.$4')`

This regular expression uses capture groups and pulls the four IP octets present in the hostname to build the `host_ip`.  These four capture groups are noted as `$1.$2.$3.$4`, respectively. This method is very fast, and removes the need to perform a DNS lookup from the `host` field to get the host's IP address. Magic!

![](AQ0xZeu.67eda27616.png)

#### Results of the Eval Function and Replace Method

The `host_ip` field is now added to the events, displayed below `host`:

![](XqUIi7A.2ab62470ef.png)

## Customizing the Sourcetype

Finally, let's put some sense into the `sourcetype` field, using another Eval Function. By combining the values of the `${sourcetype}_${region}_${az}`, the sourcetype becomes `cloud-init_us-west1_a` – so now you can understand much more about the sourcetype at a glance. 

Examine this Eval Function's value expression, taking careful note of the backticks (`` ` ` ``) and braces (`{ }`) that surround the field names, and the underscore (`_`) that separates them.

![](MPZZbaJ.45b7c733ff.png)

#### Results of the Eval Function to Combine Values

Take a look at the updated sourcetypes, and enjoy exploring Cribl Stream with your new knowledge!

![](s1tUvep.ff0d06621f.png)

## Try This at Home

Below you'll find the lookup table, Pipeline, and sample events demonstrated in this use case. Create the lookup file first, and then import the Pipeline. (The order matters, because the Pipeline import depends on the lookup table's presence.)

### Lookup Table

To create the lookup table in Cribl Stream's UI, select **Knowledge > Lookups**, then click **New Lookup File** and select **Create with Text Editor**.

Copy and paste in the header and rows listed below, then save the result as: `region_index_sourcetype.csv`.

``` {title="region_index_sourcetype.csv"}
region,index,sourcetype
us.+,usa_index_tier,cloud-init
asia.+,apac_index_tier,cloud-init
europe.+,emea_index_tier,cloud-init
northamerica.+,na_index_tier,cloud-init
southamerica.+,ltam_index_tier,cloud-init
```

### Pipeline

Below is an export of the whole Cribl Stream Pipeline presented here. [Import](pipelines#json) this JSON to get a Pipeline named: `setting_index_by_region_availability_zone.json`.

```json {title="Magic Pipeline"}
{
  "id": "setting_index_by_region_availability_zone",
  "conf": {
    "output": "default",
    "groups": {},
    "asyncFuncTimeout": 1000,
    "functions": [
      {
        "filter": "true",
        "conf": {
          "comment": "This pipeline demonstrates four different ways to leverage regular expressions in a Cribl Stream Pipeline including field extraction, lookup, replace, and field value manipulation."
        },
        "id": "comment"
      },
      {
        "filter": "true",
        "conf": {
          "source": "_raw",
          "iterations": 100,
          "overwrite": false,
          "regex": "/GMT:\\s+(?<host>[^.]+)\\.(?<region>\\w+-\\w+\\d+)-(?<az>[^.]+)\\.(?<domain>[^:]+):/"
        },
        "id": "regex_extract",
        "disabled": false,
        "description": "Extract host, region, availability_zone, and domain"
      },
      {
        "filter": "true",
        "conf": {
          "matchMode": "regex",
          "matchType": "specific",
          "reloadPeriodSec": 60,
          "addToEvent": false,
          "inFields": [
            {
              "eventField": "region"
            }
          ],
          "ignoreCase": false,
          "file": "region_index_sourcetype.csv"
        },
        "id": "lookup",
        "disabled": false,
        "description": "Lookup index and sourcetype using regex matching"
      },
      {
        "filter": "true",
        "conf": {
          "add": [
            {
              "name": "host_ip",
              "value": "host.replace(/\\w+-(\\d+)-(\\d+)-(\\d+)-(\\d+)/,'$1.$2.$3.$4')"
            }
          ]
        },
        "id": "eval",
        "description": "Create IP from hostname, removing DNS requirement",
        "disabled": false
      },
      {
        "filter": "true",
        "conf": {
          "add": [
            {
              "name": "sourcetype",
              "value": "`${sourcetype}_${region}_${az}`"
            }
          ]
        },
        "id": "eval",
        "description": "Append region and az to sourcetype",
        "disabled": false
      }
    ]
  }
}
```

### Sample Events 

And here's a sample of raw events that you can upload or copy/paste into Cribl Stream's [Preview](data-preview) pane to test the Pipeline's Functions:

``` {title="Sample events"}
Feb 06 2021 02:18:31.286 GMT: ec2-35-162-133-145.us-west1-a.compute.amazonaws.com: cloud-init[2929]: url_helper.py[DEBUG]: [0/1] open 'http://145.133.162.42/latest/api/token' with {'url': 'http://145.133.162.36/latest/api/token', 'headers': {'X-aws-ec2-metadata-token-ttl-seconds': '21600', 'User-Agent': 'Cloud-Init/19.3-5.amzn2'}, 'allow_redirects': True, 'method': 'PUT', 'timeout': 1.0} configuration
Feb 06 2021 03:33:30.302 GMT: ec2-48-169-111-182.us-east2-b.compute.amazonaws.com: cloud-init[2929]: __init__.py[DEBUG]: Looking for data source in: ['Ec2', 'None'], via packages ['', u'cloudinit.sources'] that matches dependencies ['FILESYSTEM']
Feb 06 2021 06:29:11.841 GMT: ec2-21-187-232-201.asia-northeast3-a.compute.amazonaws.com: cloud-init[2929]: atomic_helper.py[DEBUG]: Atomically writing to file /var/lib/cloud/data/status.json (via temporary file /var/lib/cloud/data/tmpS7ibzJ) - w:[644] 489 bytes/chars
Feb 06 2021 12:59:44.232 GMT: ec2-76-187-246-132.europe-west3-b.compute.amazonaws.com: cloud-init[2929]: stages.py[DEBUG]: Running module power-state-change (<module 'cloudinit.config.cc_power_state_change' from '/usr/lib/python2.7/site-packages/cloudinit/config/cc_power_state_change.pyc'>) with frequency once-per-instance
Feb 06 2021 17:04:16.921 GMT: ec2-67-205-202-104.northamerica-northeast1-c.compute.amazonaws.com: cloud-init[2929]: util.py[DEBUG]: Running command ['lxc-is-container'] with allowed return codes [0] (shell=False, capture=True)
Feb 06 2021 19:45:47.687 GMT: ec2-87-209-176-201.southamerica-east1-a.compute.amazonaws.com: DataSourceEc2.py[DEBUG]: Removed the following from metadata urls: ['http://instance-data.:8773']
```

From here, modify the sample data, lookup table, and Functions to adapt this approach to your own needs!
