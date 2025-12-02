# Cribl Lake Direct Access


Ship your data straight into Cribl Lake, bypassing Cribl Stream.

---

You can use Direct Access options to archive certain data directly to Cribl Lake. Here, you bypass routing inbound data through Cribl Stream. Once your data is integrated with Cribl Lake, Cribl Stream and Cribl Search can access it as a Lake Dataset.

We provide the following Direct Access options for different data sources:

- [Direct Access (HTTP)](direct-access-http): Use this option to ingest data over HTTP from a wide variety of senders. Examples include FluentBit, Logstash, Vector, and the OpenTelemetry Collector. You will find specific configuration settings for Elasticsearch Bulk API and Splunk HEC senders.

- [Splunk Cloud Self Storage](splunk-cloud): Use this option to ingest data from Splunk Cloud DDSS (Dynamic Data Self Storage) indexes.

> You can configure one Direct Access Source of each type per Workspace.
{.box .info}