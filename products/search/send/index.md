# send


The `send` operator forwards search results to Cribl Stream, or to compatible endpoints.

## Usage {#usage}

By default, `send` forwards search results to a [Cribl HTTP Source](/stream/sources-cribl-http) in a distributed Cribl Stream deployment. There, you can transform, route, and relay the results in multiple ways.

When you target a Cribl-managed Stream Worker Group within the same [Workspace](workspaces) over Cribl HTTP, you incur no additional ingest charges. You consume credits only for the CPU time that `send` uses in Cribl Search.

> The `send` operator can't target Cribl-managed Worker Groups in a Workspace that has enabled an [Access Control List](workspaces#set-up-acl) (ACL).
>
{.box .warning}

## Permissions {#permissions}

Search Editors and Admins can send events to any Cribl-managed Stream Worker Group in the same Organization.

Search Admins can also send events to any endpoint that accepts a POST request and NDJSON content. (Ingest charges might
apply outside the Cribl HTTP scenario outlined above.)

Here's a full breakdown of where different [Search Member Types](cloud-managing#search-member-roles) can send
events:

| Search Member Type | Default Stream Worker Group | Named Stream Worker Group | Custom URL |
| ------------------ | --------------------------- | ------------------------- | ---------- |
| Search User        | ✓                           |                           |            |
| Search Editor      | ✓                           | ✓                         |            |
| Search Admin       | ✓                           | ✓                         | ✓          |
| Organization Admin | ✓                           | ✓                         | ✓          |
| Organization Owner | ✓                           | ✓                         | ✓          |

## Rules

* Aggregate results are sent once the search completes, so you won't see results until then.
* By default, events are sent to the [Cribl HTTP Source](/stream/sources-cribl-http) in the same Cribl.Cloud Organization as your Cribl Search instance.
* Also use a Cribl HTTP Source to receive data on hybrid Worker Groups. 
* Use the [`centralize`](centralize) operator when sending results from Edge Worker Nodes that lack access to the internet or to your Cribl.Cloud Organization.
* The `group` parameter is only for Cribl-managed Worker Groups in Cribl.Cloud, and cannot be used together with the `URL` argument.
* If any events are dropped, the [search logs](search-details#logs) display an appropriate error message.
* The `send` operator sets the `Content-Type: application/x-ndjson` header on POST requests.

## Syntax

    `... | send [ tee=TeeBoolean ] [ group=WorkerGroup | "URL" ]`

### Arguments {#arguments}

* **TeeBoolean**: Defaults to `false`, where search results are not shown in Cribl Search. Instead, you get an event with the URL, the status, and the number of bytes and events sent or dropped. When set to `true`, the search results are displayed, and no stats are provided. For example, `tee=true`.
* **WorkerGroup**: The Cribl Stream Worker Group to send data to. The default value is literally `default`. For example, `group=default`.
* **URL**: Your domain for inbound data, with the appropriate port. Defaults to your Cribl.Cloud ingest address and port `10200`, the default port for the Cribl HTTP Source. The general format is:  
`https://<groupName>.<workspaceName>.<organizationId>.cribl.cloud:10200`

> An example with the most typical Worker Group and Workspace names, and with a fictitious Organization name:
`https://default.main.goat-farm.cribl.cloud:10200` 

> To check the ingest address for Cribl-managed Groups: From your Organization's top bar, select **Products** > **Cribl**. From the resulting sidebar, select **Data Sources**, then select the target **Group**. Under **Sources Enabled by Default**, find the `in_cribl_http` entry. 
>
> For a [hybrid Group](/stream/cloud-enterprise#hybrid) running on a host that you manage, the ingest address will typically be configured through your load balancer.
{.box .info}

## Examples

* Send events to the `default` Worker Group:

    ```kusto
    dataset=myDataset
    | send
    ```

* Send up to 100 events and display the results:

    ```kusto
    dataset=myDataset
    | limit 100 
    | send tee=true
    ```

* Send events to a Worker Group named Goat:

    ```kusto
    dataset=myDataset
    | send group=goat
    ```

* Send events to a hybrid Worker Group:

    ```kusto
    dataset=myDataset
    | send "https://in.your-tenant.com:10200"
    ```

* Send aggregate results by specifying the URL to your Cribl Stream internal Cribl HTTP Source:
    
    ```kusto
    dataset=myDataset
    | summarize count() by action
    | send "https://in.main-default-domain.cribl.cloud:10200"
    ```

* Send results to the Cribl.Cloud Stream default Worker Group:

    ```kusto {runSearch=true}
    dataset=$vt_dummy event<10 
    | extend _raw=iif(event%2>0, "This is a test event", "This is another event") 
    | send
    ```
