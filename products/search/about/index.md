# About Cribl Search


Get a a quick walk-through of Cribl Search capabilities, compatibility, and usage.

---

Cribl Search helps you explore data in place, right where it lives. You can query logs, metrics, or any sort of machine data immediately, without first moving it to specialized storage. You can automate your searches, visualize and share the results, and get alerts about changing trends. Cribl Search offers native support for a wide array of data sources:

- Data lakes – including [Cribl Lake](/lake/), Amazon Security Lake, and Amazon S3 and S3-compatible lakes.

- Object stores – including Amazon S3, Azure Blob Storage, Google Cloud Storage, and more.

- Cloud data warehouses – including Snowflake and Clickhouse.

- Analytics services and platforms – including Azure Data Explorer, Elasticsearch, OpenSearch, Prometheus, and more.

- API endpoints – including Azure, AWS, Google Workspace, Okta, Zoom, and even a Generic HTTP API option that enables you to query any HTTP API.

 Beyond Cribl Lake, Cribl Search seamlessly integrates with [Cribl Stream](/stream/) and [Cribl Edge](/edge/). 
 
 Depending on the data provider you're searching, Cribl Search either handles all the data processing itself, or takes advantage of the data provider's native compute resources to offload and optimize processing.

For the full list of natively supported data sources, see: [List of Supported Data Sources](connect-to-data#list).

## What Can You Do with Cribl Search? {#what}

Cribl Search enables you to search, transform, and analyze your data, regardless of its original source or format.

Cribl Search is offered as a service via Cribl.Cloud. Your data can reside anywhere – in the public or private cloud, on-prem, etc. Cribl Search processes log events in the location where they're stored, requiring no indexes.

## Who Is Cribl Search For? {#who}

Cribl Search is built for administrators, managers, and users of operational/DevOps and security intelligence products and services. This includes system administrators, data scientists, incident responders, and threat hunters.

## How Does Cribl Search Work? {#how}

Cribl Search leverages the [Kusto Query Language](https://learn.microsoft.com/en-us/kusto/query/?view=microsoft-fabric) (KQL). Even if you are unfamiliar with Kusto, Cribl Search offers typeahead help in building your queries – and even an AI assistant that can translate your native-language queries to KQL syntax.

Cribl Search then queries data in place, using the specific language or technology associated with the data's storage location. By automatically converting your KQL query into the required target query language, Cribl Search spares users from needing to learn the query syntax of each underlying system. 

Cribl Search dynamically spins up many parallel search executors to ensure that your search runs in the fastest wall-clock time possible. Cribl Search coordinates and federates the results streaming in from multiple systems, interprets the results, and presents a single, unified view of your result set. Cribl Search can extract structured field names and values even from unstructured source data.

## Cribl Search in Action {#action}

Let's see a basic search. Try this yourself in Cribl Search, or just watch the animation:

1. Log in to Cribl Search as an [Admin or Editor](cloud-managing#search-member-roles).
1. From the sidebar, select **Search Home**.
1. From the list of **Available Datasets**, select `cribl_search_sample` to place a basic query in the query box.
1. Press Enter/Return, or select the **Search** button.
1. Results for this search will appear on the **Events** tab, with a histogram.


![Basic search](search-4.10-animation.gif)
{scale="100%"}



## What's Next? {#next}

Ready to put Cribl Search to work? See [Basic Concepts](basic-concepts).
