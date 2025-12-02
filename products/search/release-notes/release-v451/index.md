# Cribl Search 4.5.1


Starting with Cribl Search 4.5.1, you can visualize search results in scatter plots, better customize your map charts,
and quickly connect to your OpenSearch and Elasticsearch data.

### Scatter Charts

A new chart type, [scatter chart](chart-scatter), can help you visualize relationships between two variables. This
allows you to spot trends, clusters, and outliers more effectively.

### GeoIP Map Charts

We've added a new type of [map chart](chart-map) that allows you to easily visualize GeoIP data.

### Speed Up CloudTrail and VPC Flow Logs Processing

Two new [event breakers](datatypes#event-breaker-types) let you process your AWS
[VPC Flow Logs](datatypes#aws-vpc-flow-log-breaker) and [CloudTrail events](datatypes#aws-cloudtrail-breaker) much
faster.

### Connect to OpenSearch and Elasticsearch

We've added dedicated Dataset Provider types for [OpenSearch](set-up-opensearch) and
[Elasticsearch](set-up-elasticsearch), allowing you to quickly reach your index data.

### More Control Over Your Metadata

The new `.drop all metadata` command lets you quickly delete all Cribl Search metadata from a Dataset. 

>This command was later deprecated and removed, as of Cribl Search 4.13.1.
{.box .info}

### AssumeRole Authentication

[AWS API](set-up-aws) Datasets now support
[`AssumeRole`](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) credentials.

### Search Progress Visualization

The Cribl Search UI now provides more information about your search progress.
