# Office 365 Services Source


Cribl Stream supports receiving data from the Microsoft Graph [service communications API](https://docs.microsoft.com/en-us/graph/api/resources/service-communications-api-overview?view=graph-rest-1.0&preserve-view=true). This facilitates analyzing the status and history of service incidents on multiple Microsoft cloud services, along with associated incident and Message Center communications. For details, see Microsoft's [Overview](https://docs.microsoft.com/en-us/graph/service-communications-concept-overview) of the Graph API.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **YES**
> 
> TLS is enabled via the HTTPS protocol on this Source's underlying REST API.
> 
> [Use a REST/API Collector](/stream/usecase-rest-ms-graph) to collect from other Microsoft Graph API resources.
>
{.box .info}

## Prerequisites

You need to have a [registered application](https://learn.microsoft.com/en-us/graph/auth-register-app-v2) with the Microsoft identity platform to provide Cribl Stream access to make requests to the Microsoft Graph API.

From the registered application, you'll need its **Application (client) ID**, **Directory (tenant) ID**, and [credentials](https://learn.microsoft.com/en-us/graph/auth-register-app-v2#add-credentials) to configure in the Cribl Stream Source.

> For a complete, step-by-step walkthrough, see our Knowledge Base article [Office 365 App Registration for Cribl Sources](https://knowledge.cribl.io/microsoft-sentinel-47/office-365-app-registration-for-cribl-sources-1785).
{.box .success}

### Microsoft Permissions

The application representing your Cribl Stream instance needs the following [application permissions](https://learn.microsoft.com/en-us/graph/permissions-overview) to pull data. The permission **Type** for both must be `Application` –  `Delegated` is not sufficient:

- `ServiceHealth.Read.All`
- `ServiceMessage.Read.All`

For the full list of permissions exposed by Microsoft Graph, see the [Microsoft Graph permissions reference](https://learn.microsoft.com/en-us/graph/permissions-reference).

## Configure Cribl Stream to Receive Data from the Service API   

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
     - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
     - **Input ID**: Enter a unique name to identify this Office 365 Services definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
     - **Description**: Optionally, enter a description.
     - **Tenant ID**: Enter the Office 365 Azure **Directory (tenant) ID**.
     - **App ID**: Enter the Office 365 Azure **Application (client) ID**.
3. Under **Authentication**, select the **Authentication method** from the dropdown:
     - **Manual**: This default option provides a **Client secret** field, where you directly enter the required Office 365 Azure client secret.
     - **Secret**: This option instead exposes a **Client secret (text secret)** drop-down, from which you select a stored text secret to authenticate with. Select **Create** to configure a new secret.
4. Next, you can configure the following **Optional Settings**: 
    - **Subscription Plan**: See [Subscription Plan](#plan) below. 
    - **Content Types**: See the [Content Types](#content-types) section below.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 


#### Subscription Plan 
 Select the Office 365 subscription plan for your Organization. Options include:
  - **Office 365 Enterprise**
  - **Office 365 GCC**
  - **Office 365 GCC High**
  - **Office 365 DoD**

#### Content Types {#content-types}

Here, you can configure polling separately for the following types of data from the Office 365 Service Communications API:

* **Current Status**: Get a real-time view of current and ongoing service incidents.
* **Messages**: Find incident and Message Center communications. 
   
As of this revision, this Microsoft API provides data for Office 365, Yammer, Dynamics CRM, and Microsoft Intune cloud services. For each of these content types, this section provides the following controls:

**Enabled**: Toggle on for each service that you want to poll.

**Interval**: Optionally, override the default polling interval. See [About Polling Intervals](#intervals) below.

**Log level**: Set the verbosity level to one of `debug`, `info` (the default), `warn`, or `error`.

##### About Polling Intervals {#intervals}

To poll the Office 365 Service Communications API, Cribl Stream uses the **Interval** field's value to establish the search date range and the cron schedule, for example: 
`*/${interval} * * * *`

Therefore, intervals set in minutes – those for **Current Status** – must divide evenly into 60 minutes to create a predictable schedule. Dividing 60 by intervals like `1`, `2`, `3`, `4`, `5`, `6`, `10`, `12`, `15`, `20`, or `60` itself yields an integer, so you can enter any of these values. 

Cribl Stream will reject intervals like `23`, `42`, or `45`, or `75` – which would yield non-integer results, meaning unpredictable schedules.

### Processing Settings {#processing}

#### Fields 

[Snippet not found: content/shared/4.14/snippets/_sources-add-fields.md]

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Retries {#retries}

**Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.

**Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the limit specified in **Retry limit**. 

**Retry limit**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.

**Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.

**Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.

**Honor Retry-After header**: When toggled on (the default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Stream always ignores `retry-after` headers that specify a delay longer than 20 seconds.
  * Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).
  * When toggled off, Cribl Stream ignores all `retry-after` headers.

**Retry connection timeout**: Toggle on to automatically retry a single connection attempt after a timeout (`ETIMEDOUT`) to ensure data continuity.

**Retry connection reset**: Toggle on to automatically retry a connection after a peer reset (`ECONNRESET`) to maintain data flow.

### Advanced Settings {#advanced-tab}

**Request timeout (secs)**: The maximum time period for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `300` (5 minutes). Enter `0` to disable timeout metering.

**Job timeout**: Maximum time the job is allowed to run (for example, `30`, `45s`, or `15m`). Units are seconds, if not specified. Enter `0` for unlimited time. Defaults to `0`.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__final`
  * `__inputId`
  * `__isBroken`
  * `__source`

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## How Cribl Stream Pulls Data

The Office 365 Services Source retrieves data using Cribl Stream scheduled Collection jobs, which include Discover and Collection phases. The Discover phase task returns the URL of the content to collect.

In the Source's **General Settings** > **Content Types** > **Interval** column, you configure the polling schedule for each Content Type independently.

The job scheduler spreads the Collection tasks across all available Workers. The collected content is paginated, so the collection phase might include multiple calls to fetch data.

## Viewing Scheduled Jobs

This Source executes Cribl Stream's [scheduled collection jobs](collectors#scheduled). Once you've configured and saved the Source, you can view those jobs' results by reopening the Source's config modal and selecting its **Job Inspector** tab.

Each content type that you enabled gets its own separate scheduled job.

You can also view these jobs (among scheduled jobs for other Collectors and Sources) in the **Monitoring** > **System** > **Job Inspector** > **Currently Scheduled** tab.

#### Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Stream's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Advanced** > **Timeout (secs)** to a duration shorter than the default of `300` seconds (5 minutes).
* If adjusting **Timeout (secs)** does not fix the problem, try the global setting **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. You'll find **Job Timeout** among the [task manifest and buffering limits](collectors-job-limits#task-manifest-and-buffering-limits).

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Stream will wait for an HTTP request to complete. 
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## See Also {#see-also}

The Community-contributed [Microsoft Office Activity & Azure Logs Pack](https://packs.cribl.io/packs/microsoft-activity) provides sample Pipelines to parse and streamline ingested Office 365 and EntraID/Azure AD data. You can import these and adapt them to your own needs.

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_sources-troubleshooting.md]

### Common Issues

[Snippet not found: content/shared/4.14/snippets/_common-errors-http-based-source.md]