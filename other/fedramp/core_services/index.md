# The Cribl Product Suite

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality presented described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

This section provides an overview of the core products within the Cribl suite.

## Cribl Stream

Core functions include:
- Collect data from multiple Source types.
- Parse and restructure logs and metrics.
- Route data to various Destinations.
- Reduce data via filtering and aggregation.
- Transform and enriches events.

Learn more about Cribl Stream: [Cribl Stream Basic Concepts](/stream/basic-concepts/).

## Cribl Edge

Key capabilities include:
- Collect data locally from files, Windows events, and metrics.
- Pre-processes data before routing.
- Reduce network utilization through filtering and compression.
- Operate on Windows, Linux, and Kubernetes environments.
- Communicate with the Leader Node for centralized management.

Learn more about Cribl Edge: [Cribl Edge Basic Concepts](/edge/basic-concepts/).

## Cribl Search
Cribl Search functions as a query layer between users and multiple data storage systems. Key features include: 
- Offer federated search across multiple data stores without data centralization.
- Support multiple data formats and storage types.
- Execute queries on data in its original location.
- Implement consistent query language across diverse data Sources.
- Return result sets in standardized formats.

Learn more about Cribl Search: [Cribl Search Basic Concepts](/search/basic-concepts/).


## Cribl Lake
With Cribl Lake you can: 

- Access data via an S3-compatible API.
- Integrate directly with Stream for data ingestion.
- Use schema-on-read for flexible data models.
- Optimize for long-term data retention.
- Support data archiving and compliance requirements.
- Interface with Search for querying stored data.

Learn more about Cribl Lake: [About Cribl Lake](/lake/about/).

## Cribl Lakehouse

Lakehouse serves as both a primary storage solution and a Destination for processed data. Key features include: 

- Store data in a columnar format for efficient querying.
-  Use a tiered storage architecture for cost optimization.
-  Enforce schema for consistent data structure.
-  Index data for accelerated query performance.
-  Enforce retention policies.
-  Integrate with Cribl Stream for data ingestion.

Learn more about Cribl Lakehouses: [About Cribl Lakehouses](/lake/lakehouse/)