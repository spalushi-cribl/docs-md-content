# Explore Cribl Products


> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

## Cribl Stream

Cribl Stream helps you process machine data – logs, instrumentation data, application data, metrics, etc. – in real time, and deliver them to your analysis platform of choice.  

Core capabilities include:
- Collect data from multiple Source types.
- Parse and restructure logs and metrics.
- Route data to various destinations.
- Reduce data via filtering and aggregation.
- Transform and enrich events.

> The linked tutorials and guides will help you orient you to our Cribl Product Suite. Cribl.Cloud Government offers comparable features to our commercial platform, and these guides are applicable to both environments.
>
{.box .info}

| Tutorial                                                    | Description                                                                                                                               |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [About Cribl Stream]( /stream/about/) | Overview of core features and use cases.                                                    |
| [Get Started with Cribl Stream](/stream/getting-started-guide/)      | Hands-on tutorial that guides you through the core features and workflows.              |
| [Cribl Stream Tour](/stream/tour/)          | Self-guided tour of key features in Cribl Stream.   |

## Cribl Edge

Cribl Edge is a lightweight, intelligent agent that runs directly on your hosts (such as macOs, Linux, Windows, or Kubernetes environments).

Key capabilities include:
- Collect data locally from files, Windows events, and metrics.
- Pre-process data before routing.
- Reduce network utilization through filtering and compression.
- Operate on MacOS, Windows, Linux, and Kubernetes environments.
- Communicate with the Leader Node for centralized management.

| Tutorial                                                    | Description                                                                                                                               |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [About Cribl Edge](/edge/about/) | Overview of core features and use cases.                                                    |
| [Get Started with Cribl Edge](/edge/getting-started-guide/)      | Hands-on tutorial that guides you through the core features and workflows.              |
| [Explore Cribl Edge on Linux](/edge/explore-edge/)          | Self-guided tour of Cribl Edge on Linux.   |
| [Explore Cribl Edge on Windows](/edge/explore-edge-win/)          | Self-guided tour of Cribl Edge on Windows.   |

## Cribl Search

Cribl Search functions as a query layer between users and multiple data storage systems. Key features include: 
- Federated search across multiple data stores without data centralization.
- Support multiple data formats and storage types.
- Execute queries on data in its original location.
- Implement consistent query language across diverse data Sources.
- Return result sets in standardized formats.
| Tutorial                                                    | Description                                                                                                                               |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [About Cribl Search](/search/about/) | Overview of core features and use cases.                                                    |
| [Quick Setup](/search/getting-started-guide/)      | Hands-on tutorial that guides you to your first search.             |
| [Cribl Search Tour](/search/search-overview/)          | Self-guided tour of Cribl Search.  |
| [Common Search Examples](/search/common-examples/)          | Common ways of searching data.  |


## Cribl Lake 

Cribl Lake is a cost-effective, long-term storage solution for all your data. 

Key features include: 

- Access data via an S3-compatible API.
- Integrate directly with Stream for data ingestion.
- Use schema-on-read for flexible data models.
- Optimize for long-term data retention.
- Support data archiving and compliance requirements.
- Interface with Search for querying stored data.

| Tutorial                                                    | Description                                                                                                                               |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [About Cribl Lake](/lake/about/) | Overview of core features and use cases.                                                    |
| [Lake Datasets](/lake/datasets/)      | Introduction to datasets             |
| [Search Cribl Lake](/lake/search-cribl-lake/)          | How to search a dataset. |

## Cribl Lakehouse 

Cribl Lakehouse serves as both a primary storage solution and a Destination for processed data. Key features include: 

- Store data in a columnar format for efficient querying.
-  Use a tiered storage architecture for cost optimization.
-  Enforce schema for consistent data structure.
-  Index data for accelerated query performance.
-  Enforce retention policies.
-  Integrate with Cribl Stream for data ingestion.

[Learn more about Cribl Lakehouse](/lake/lakehouse/).

## Cribl Guard

Cribl Guard provides real-time sensitive data detection and control within data pipelines. Key functions include:

- Classification of PII, credit cards, passports, and custom data types with reduced false positives.
- Configurable redaction, masking, or blocking actions based on data classification.
- Native integration with Cribl Stream for in-line data inspection and transformation.
- Automated cataloging of sensitive data types for compliance auditing.
- Granular controls with custom policy templates and sensitivity-level configurations.

[Learn more about Cribl Guard](/guard/).