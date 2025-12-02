# Streamline Logs Storage and Analysis with a Lakehouse

Use a Cribl Lakehouse to efficiently store, and quickly analyze, high-volume recent telemetry data.

---

This is an overview of how to ingest log files into a Lakehouse, and then use [Cribl Search](/search/) to periodically analyze only specific data relevant for threat-hunting or security investigations.

Especially as data volume increases, this can be a cost-effective alternative to bulk-forwarding data to a traditional SIEM system. This topic demonstrates sequential setup in your upstream logs sender, Cribl Stream, Cribl Lake, and Cribl Search. The broad steps are:

1. [Collect](#collect-parse) and stage logs in the applications and frameworks that generate them..
1. [Shape](#shape) the data, using a Cribl Stream Parser.
1. [Create](#dataset) a Cribl Lake Dataset to store your parsed data.
1. [Route](#route) the parsed data to the new Lake Dataset.
1. [Configure](#lakehouse) a Lakehouse, to support low-latency analytics in Cribl Search.
1. [Analyze](#search) relevant data, using Cribl Search ad hoc and scheduled queries.
1. [Visualize](#dashboard) your data, using Cribl Search predefined or custom Dashboards.

## Collect, Stage, and Shape Logs {#collect-parse}

Once you've configured log collection on the infrastructure where you run your applications, ingesting logs into [Cribl Stream](/stream/) provides a straightforward way to parse events for efficient storage and retrieval in Cribl Lake.

Cribl Stream ships with dedicated [Source](/stream/sources/) integrations for common data senders and protocols. Our [Integrations](https://cribl.io/integrations/) page outlines steps for customizing our Sources to ingest data from senders without dedicated counterparts.

### Shape with Parsers {#shape}

With data flowing through Cribl Stream, you can apply the built-in [Parser Function](/stream/parser-function) to parse events into fields optimized for Cribl Lake storage. This Function enables you to choose from a [library](/stream/parsers-library) of predefined Parsers for common data sources, and the library's UI facilitates customizing, cloning, and adding Parsers to shape your specific data.

Here, we provide specific steps for collecting, staging, and parsing three common Amazon log formats: VPC Flow Logs, CloudTrail logs, and CloudFront logs. Beyond these specific examples, you can use a similar workflow to efficiently store and analyze many other types of logs, by substituting a different Stream Parser or configuring your own.

{{% tabs "vpc" "vpc" "VPC Flow Logs" "cloudtrail" "CloudTrail Logs" "cloudfront" "CloudFront Logs" %}}

{{% tab-item "vpc" %}}
### Collect and Stage VPC Flow Logs {#collect-vpc}

Your first steps take place in the Amazon VPC console or EC2 console. If you're not already collecting VPC Flow Logs,
the Amazon [Logging IP Traffic Using VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
documentation provides specific instructions.

1. [Set up AWS VPC Flow logging](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-flow-logs.html) within
   your AWS account.
1. Configure [writing](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3.html) the VPC Flow Logs to an
   Amazon S3 bucket.
1. [Enable S3 permissions](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3-permissions.html) for Cribl Stream to read from the bucket.



### Shape (Parse) the Data {#shape-vpc}

Next, use Cribl Stream to ingest your VPC Flow Logs data, and to parse the named fields that Lakehouse requires. (You'll
want to configure all Stream steps within your Cribl.Cloud Organization, to enable [routing to Cribl Lake](#route)
later.)

1. Configure a Cribl Stream [Amazon S3 Source](/stream/sources-s3/) to continuously collect (stream) data from your S3
   bucket.
1. Create or adapt a simple Cribl Stream [Pipeline](/stream/pipelines#access-and-view) that will process this data.
1. Add a single [Parser Function](/stream/parser-function/) to the Pipeline.
1. In the Parser, accept default `Extract` mode, then set the **Library** to `AWS VPC Flow Logs`. As shown below, this
   will automatically set the **Type** to `Extended Log File Format`.
1. Save the Pipeline.


![Cribl Stream Parser Function configuration, showing Extract mode and AWS VPC Flow Logs Library selected.](parser-vpc-flow-logs.png)
{scale="70%" caption="Parser configuration for VPC Flow Logs"}


> You might choose to expand this Pipeline to add Tags, or to add other predefined Cribl Stream [Functions](/stream/functions/) to shape or narrow your data in specific ways. This scenario presents the simplest scenario to parse your data for Cribl Lake.
>
> The Cribl [AWS VPC Flow for Security Teams](https://packs.cribl.io/packs/cribl-vpc-flow-for-security-teams) Pack provides sample Pipelines that you can adapt and add to Cribl Stream. For usage details, see [Cribl Stream for InfoSec: VPC Flow Logs – Reduce or Enrich? Why Not Both?](https://cribl.io/blog/logstream-for-infosec-vpc-flow-logs-reduce-or-enrich-why-not-both/) You can also use the [Cribl Copilot Editor](/stream/copilot-editor/) (AI) feature to build a Pipeline from a natural-language description of the processing you want.
{.box .success}
{{% /tab-item %}}

{{% tab-item "cloudtrail" %}}
### Collect and Stage CloudTrail Logs {#collect-cloudtrail}

Your first steps take place in the Amazon Management Console. If you're not already collecting CloudTrail logs, the
Amazon [What Is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
documentation provides specific instructions.

1. [Set up trails within your AWS account](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/tutorial-trail.html) to log events of interest.
1. Configure [writing](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/tutorial-lake-S3.html) the CloudTrail
   logs to an Amazon S3 bucket.
1. [Enable S3 permissions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html) for Cribl Stream to read from the bucket.

### Shape the Data {#shape-cloudtrail}

Next, use Cribl Stream to ingest your CloudTrail data, and to parse the named fields that Lakehouse requires. (You'll
want to configure all Stream steps within your Cribl.Cloud Organization, to enable [routing to Cribl Lake](#route)
later.)

1. Configure a Cribl Stream [Amazon S3 Source](/stream/sources-s3/) to continuously collect (stream) data from your S3
   bucket.
1. Create or adapt a simple Cribl Stream [Pipeline](/stream/pipelines#access-and-view) that will process this data.
1. Add a single [Parser Function](/stream/parser-function/) to the Pipeline.
1. In the Parser, accept default `Extract` mode, then set the **Library** to
   `AWS VPC Flow Logs` (this library can handle CloudTrail logs, too). As shown below, this
   will automatically set the **Type** to `Extended Log File Format`.
1. Save the Pipeline.




![Cribl Stream Parser Function configuration, showing Extract mode and AWS VPC Flow Logs Library selected.](parser-vpc-flow-logs.png)
{border="false" scale="60%" caption="Parser configuration for CloudTrail logs"}


> You might choose to expand this Pipeline to add Tags, or to add other predefined Cribl Stream [Functions](/stream/functions/) to shape or narrow your data in specific ways. This scenario presents the simplest scenario to parse your data for Cribl Lake.
>
> The [Cribl Pack for AWS CloudTrail Data Collection](https://packs.cribl.io/packs/cribl-aws-cloudtrail-logs) provides sample Pipelines that you can adapt and add to Cribl Stream. You can also use the [Cribl Copilot Editor](/stream/copilot-editor/) (AI) feature to build a Pipeline from a natural-language description of the processing you want.
{.box .success}
{{% /tab-item %}}

{{% tab-item "cloudfront" %}}
### Collect and Stage CloudFront Logs {#collect-cloudfront}

If you're not already collecting CloudFront logs, the Amazon
[CloudFront and Edge Function Logging](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/logging.html)
documentation provides specific instructions.

1. In your Amazon account, set up CloudFront logging options of interest.
1. Configure [writing the CloudFront logs](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/standard-logging.html#send-logs-s3) to an Amazon S3 bucket.
1. [Enable S3 permissions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-iam.html) for Cribl Stream to read from the bucket.

### Shape (Parse) the Data {#shape-cloudfront}

Next, use Cribl Stream to ingest your CloudFront data, and to parse the named fields that Lakehouse requires. (You'll
want to configure all Stream steps within your Cribl.Cloud Organization, to enable [routing to Cribl Lake](#route)
later.)

1. Configure a Cribl Stream [Amazon S3 Source](/stream/sources-s3/) to continuously collect (stream) data from your S3
   bucket.
1. Create or adapt a simple Cribl Stream [Pipeline](/stream/pipelines#access-and-view) that will process this data.
1. Add a single [Parser Function](/stream/parser-function/) to the Pipeline.
1. In the Parser, accept default `Extract` mode, then set the **Library** to `AWS CloudFront`. As shown below, this
   will automatically set the **Type** to `Delimited values`.
1. Save the Pipeline.




![Cribl Stream Parser Function configuration, showing Extract mode and AWS CloudFront Library selected.](parser-cloudfront.png)
{scale="65%" caption="Parser configuration for CloudFront logs"}


> You might choose to expand this Pipeline to add Tags, or to add other predefined Cribl Stream [Functions](/stream/functions/) to shape or narrow your data in specific ways. This scenario presents the simplest scenario to parse your data for Cribl Lake.
>
> You can also use the [Cribl Copilot Editor](/stream/copilot-editor/) (AI) feature to build a Pipeline from a natural-language description of the processing you want.
{.box .success}
{{% /tab-item %}}

{{% /tabs %}}

## Create a Cribl Lake Dataset {#dataset}

In Cribl Lake, [create](managing-datasets#create) a Lake Dataset to store your shaped data.

## Route the Parsed Data to Cribl Lake {#route}

Back in Cribl Stream, route your shaped data to your Lake Dataset.

1. Add a [Cribl Lake Destination](/stream/destinations-cribl-lake#configure-a-cribl-lake-destination).
1. Within that Destination, select the Lake Dataset you've just created, and save the Destination.
1. You can use either QuickConnect or Data Routes to connect your S3 Source to your Lake Destination through the Pipeline you configured earlier. (The Destination topic linked above covers both options.)


![Screen capture of example Cribl Stream QuickConnect routing, connecting S3 Source to Cribl Lake Destination through simple Pipeline.](lakehouse-quickconnect.png)
{scale="100%" caption="Example QuickConnect configuration (data not yet flowing)"}

## Configure a Lakehouse For Low-Latency Analytics {#lakehouse}

With data flow now established from your infrastructure all the way to Cribl Lake, assign the Lake Dataset to a Lakehouse to enable fast analytics.

1. [Add](lakehouse#add-a-new-lakehouse) a Lakehouse, [sized](lakehouse#lakehouse-sizes) to match your expected daily
   ingest volume.

1. When the Lakehouse is provisioned,
   [assign](lakehouse#link) your Lake Dataset to the Lakehouse.

## Analyze Relevant Data {#search}


In [Cribl Search](/search/), define queries to extract relevant data from your
logs. Here is a sample query to retrieve and present rejected connections by
source address:

```kusto
dataset="<your_lakehouse_dataset>"
| action="REJECT"
| summarize by srcaddr
```

You can [schedule your searches](/search/scheduled-searches/) to run on defined intervals. Once a query is scheduled, you can configure corresponding alerts (Search [Notifications](/search/notifications/)) to trigger on custom conditions that you define.

Here is a sample query you could schedule to generate an hourly report that tracks traffic by source and destination:

```kusto
dataset ="<your_lakehouse_dataset>" srcaddr="*" dstaddr="*"
| summarize count() by srcaddr, dstaddr
| sort by count desc
```


> If you're moving high-volume log analysis from a traditional SIEM to Cribl, you might need to adapt existing queries to the KQL language that Cribl Search uses. See our [Build a Search](/search/build-a-search/) overview and our [Common Query Examples](/search/common-examples/).
>
> You can also [Write Queries Using Cribl Copilot](/search/copilot-kql/), enabling Cribl AI to suggest KQL queries from your natural-language prompts. If you already have searches in another system of analysis, try asking Copilot to translate them to KQL.
>
> The `cribl_search_sample` Dataset, built into Cribl Search, contains VPC Flow Logs events. You can use this Dataset to experiment with queries before you enable continuous data flow and analysis.
{.box .success}

## Visualize Your Analyzed Data {#dashboard}

In Cribl Search, you can build flexible [Dashboards](/search/dashboards/) to visualize your analytics.

As a starting point for displaying analytics on many log types, you can [import](/search/packs#dispensary) the [Cribl Search AWS VPC Flow Logs](https://packs.cribl.io/packs/cribl-search-aws-vpc-flow-logs) Pack. It contains a sample **AWS VPC Flow Logs Analysis** Dashboard that displays traffic volume, traffic accepted and rejected (both time-series and cumulative), traffic by protocol, and top destination ports.


![Image of sample Cribl Search Dashboard.](lakehouse-aws-dashboard.png)
{scale="100%" caption="Sample Cribl Search Dashboard"}


> Cribl AI can automatically suggest visualizations for your Dataset, and can build visualizations from your natural-language prompts. For these options, see [Add Visualizations Using Cribl Copilot](/search/copilot-dashboards/).
{.box .success}

## Next Steps {#next}

To refine your queries and visualizations, see our Cribl Search specialized topics on:

- [Searching Amazon S3](/search/search-s3/)
- [Optimize Cribl Search](/search/better-practices/)
- [Create a Dashboard](/search/creating-adding-to-dashboards/)
- [Edit a Dashboard](/search/editing-dashboards/)
