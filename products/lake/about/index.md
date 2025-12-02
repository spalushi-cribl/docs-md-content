# About Cribl Lake


Meet Cribl Lake, a data lake solution for long-term, full-fidelity storage of IT and security data.

---

Cribl Lake receives data from various sources via [Cribl Stream](/stream/) or [Direct Access](direct-access), and stores that data – enabling you to later query the data in [Cribl Search](/search/), or to replay it through Stream as needed.

Besides data you explicitly send to the Lake, Cribl Lake also receives and retains internal logs and metrics from the Leader of your Cribl.Cloud Organization.

## What Can You Do with Cribl Lake? {#features}

Among the features that Cribl Lake provides, you can:

- Create a data lake in a few clicks and a few minutes.

- Store any type of IT and security data: raw, structured, or unstructured.

- Set and manage [retention](datasets#retention-periods) and access-control policies through a graphical UI, reducing dependencies among multiple teams.

- Use Cribl Stream [Pipelines](/stream/pipelines) and Functions to control the mix, volume, and structure of data sent to your lake.

- Search your Lake data in place, using Cribl Search, to minimize ingress charges for analysis.

- Assign Lake Datasets to [Lakehouses](lakehouse) for dramatically faster search responses and more-predictable billing.

## Requirements for Cribl Lake

Cribl Lake is available only in [Cribl.Cloud](/stream/deploy-cloud/).

Cribl Lake can connect, via a [Collector](collectors-cribl-lake) or [Destination](destinations-cribl-lake),
with both Cribl-managed [Cloud](/stream/cloud-workers/) and customer-managed [hybrid](/stream/worker-groups/) Stream Worker Groups.
Hybrid Worker Groups must be running version 4.8 or higher.

## Data Storage in Cribl Lake

Cribl Lake organizes your data into [Datasets](datasets).

![Table with all Datasets available in Cribl Lake](lake-datasets-4.9.2.png)
{scale="100%" caption="Built-in and custom Datasets in Cribl Lake."}

Your data is stored in open, non-proprietary formats, which avoids vendor lock-in.
At any time, you can replay the stored data and send it to a [Destination](/stream/destinations/) of your choice.

Cribl Lake takes care of the storage format schema and organizes the data for you.
This way, you don’t have to think about your partitioning configuration.

The data is stored as gzip-compressed JSON, or Parquet, files.

## Storage Limits in Cribl Lake

With a [Cribl.Cloud](/stream/deploy-cloud/) free plan, you can store up to 50 GB of data.

Data in the `cribl_metrics` and `cribl_logs` Datasets is stored for 30 days free of charge. 

## Send Data to Cribl Lake

You can archive data to Cribl Lake in several ways:

- To send data from any Cribl Stream Source to Cribl Lake, use the Stream [Cribl Lake Destination](destinations-cribl-lake).

- To [send the results](/search/set-up-cribl-lake#send/) of a Cribl Search query to Cribl Lake, use the [`export`](/search/export/) operator.

- To archive Splunk Dynamic Data Self Storage (DDSS) data to Cribl Lake, use the Lake [Splunk Cloud Self Storage](splunk-cloud) option.

- To archive data to Cribl Lake over HTTP, use the [Direct Access (HTTP)](direct-access-http) option. 

> (This option supports FluentBit, Logstash, Vector, the OpenTelemetry Collector, and a wide variety of other senders. You will find specific configuration settings for Elasticsearch Bulk API and Splunk HEC senders.)

## Replay Data from Cribl Lake

Cribl Lake adds a dedicated [Collector Source](collectors-cribl-lake) to Cribl Stream, which lets you replay data stored in the lake and send it via Routes or QuickConnect.

To replay data stored in Cribl Lake and send it to any [Destination](/stream/destinations/) via Routes or QuickConnect, use the [Cribl Lake Collector](collectors-cribl-lake) available in Cribl Stream.

## Search Cribl Lake

[Cribl Lake Datasets](datasets) work as [Cribl Search Datasets](/search/about#datasets/) out-of-the-box.

This means you do not need to create any additional [Dataset Providers](/search/about#dataset-providers/)
or [Datasets](/search/about#datasets/) in Cribl Search.
You can instantly [start searching](search-cribl-lake) the data flowing to your Cribl Lake.

> See [Searching Cribl Lake](search-cribl-lake) for example searches you can run over your data stored in Cribl Lake.
{.box .success}

## Monitor Cribl Lake Usage

You can monitor your Cribl Lake data usage on the Cribl.Cloud portal's [**FinOps Center**](/stream/cloud-billing) page.

Details of credits consumed by Cribl Lake are available on the [**Invoices**](/stream/cloud-billing#download-invoices) page.
