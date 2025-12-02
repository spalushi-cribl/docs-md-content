# Office 365 Message Trace Source


Cribl Stream supports receiving [Office 365 Message Trace](https://learn.microsoft.com/en-us/previous-versions/office/developer/o365-enterprise-developers/jj984335(v=office.15)) data. This mail-flow metadata can be used to detect and report on malicious activity including bulk emails, spoofed-domain emails, and data exfiltration.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
> TLS is enabled via the HTTPS protocol on this Source's underlying REST API.
> 
{.box .info} 

## Office 365 Setup {#o365-setup}

At a minimum, your Office 365 service account should include a role with `Message Tracking` and `View‑Only Recipients` permissions, assigned to the Office 365 user that will integrate with Cribl Stream. Assign these permissions in the Exchange admin center (https://admin.exchange.microsoft.com).

> For a complete, step-by-step walkthrough, see our Knowledge Base article [Office 365 App Registration for Cribl Sources](https://knowledge.cribl.io/microsoft-sentinel-47/office-365-app-registration-for-cribl-sources-1785).
{.box .success}

### Modern Authentication (OAuth 2.0) Setup {#o365-setup-oauth}

If you plan to use [OAuth](#oauth), [OAuth Secret](#oauth-secret), or [OAuth Certificate](#oauth-cert) authentication, you must configure an Application and its associated Service Principal within Microsoft Entra ID as part of your Office 365 setup.

1. In the [Microsoft Azure portal](https://portal.azure.com/), create a Microsoft Entra ID App Registration. This will automatically create a corresponding Service Principal in your tenant. For details, see the [Microsoft Azure documentation](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app).

1. Grant your Application the API permissions it needs to access Office 365 Message Trace data.
   * On your newly created App Registration, navigate to **API permissions** and select **Add a permission**.
   * On the **Microsoft APIs** tab, select **Office 365 Exchange Online**. If it's not immediately visible, search for `Office 365 Exchange Online`.
   * Under **Application permissions**, add the `ReportingWebService.Read.All` permission and consent to the changes.
   
    <br />For detailed steps on adding API permissions, see Microsoft's documentation on how to [add permissions to access web APIs](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-configure-app-access-web-apis#add-permissions-to-access-your-web-api).

1. Assign the Service Principal (your registered Application's identity in the tenant) at least one Azure AD role that will enable it to access the Reporting Web Service:
   * Global Reader
   * Global Administrator
   * Exchange Administrator

    <br />For detailed steps on assigning Azure AD roles to a Service Principal (Application), see Microsoft's documentation on how to [assign Microsoft Entra roles](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/manage-roles-portal). When assigning the role, you will search for your Application name (the Service Principal) to select it as a member.

> If you encounter an error with `statusCode: 401` and details showing `host:"reports.office365.com"` and `path:"/ecp/reportingwebservice/reporting.svc/MessageTrace"`, this indicates the App Registration (Service Principal) is not correctly authorized to access the Office 365 Message Trace service.
{.box .success}

## Configure Cribl Stream to Receive Office 365 Message Trace Data

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
     - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
     - **Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Stream will add `-CLONE` to the original **Input ID**.
     - **Description**: Optionally, enter a description.
     - **Report URL**: Enter the URL to use when retrieving report data. Defaults to: `https://reports.office365.com/ecp/reportingwebservice/reporting.svc/MessageTrace`.
     - **Poll interval**: How often (in minutes) to run the report. Must divide evenly into 60 minutes to create a predictable schedule, or Save will fail. See [About Polling Intervals](#intervals) below.
3. Under **Authentication**, select the **Authentication method** from the dropdown:
   - In the **Authentication** section, use the buttons to select an **Authentication method**: [Basic](#basic), [Basic (credentials secret)](#basic-secret), [OAuth](#oauth), [OAuth (text secret)](#oauth-secret), or [OAuth Certificate](#oauth-cert). The default is **OAuth**. All OAuth options rely on the OAuth 2.0 protocol.
4. Next, you can configure the following **Optional Settings**: 
    - Use the **Date range start** and **Date range end** settings to specify the time window for data collection. These settings help compensate for delays and gaps in Message Trace data. Use relative time formats for ongoing data collection and exact epoch values to backfill specific time gaps.
    - **Date range start**: Backward offset for the head of the search date range. Supports relative time format like `-3h@h` or exact epoch values like `1720536960`.
    - **Date range end**: Backward offset for the tail of the search date range. Supports relative time format like `-2h@h` or exact epoch values like `1720533360`.
    - **Log level**: For data collection's runtime log, set the verbosity level to one of `debug`, `info`, `warn`, or `error`. (If not selected, defaults to `info`.)
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 

##### Basic Authentication {#basic}

Selecting **Basic** exposes **Username** and **Password** fields, where you directly enter the HTTP Basic credentials to use on Message Trace API calls.

##### Basic Secret Authentication {#basic-secret}

Selecting **Basic (credentials secret)** exposes a **Credentials secret** drop-down, where you select an existing [stored secret](/stream/securing-secrets) that references your credentials on the Message Trace API. You can use the adjacent **Create** button to store a new, reusable secret.

##### OAuth Authentication {#oauth} 


The default **OAuth** authentication method exposes the following fields, all required:
- **Client secret**: Directly enter the `client_secret` to pass in the OAuth request parameter.
- **Tenant identifier**: Directory ID (tenant identifier) in Microsoft Entra ID.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.

##### OAuth Secret Authentication {#oauth-secret}

Selecting **OAuth (text secret)** exposes three of the same controls as the default [OAuth](#oauth) method, but – as you'd expect – you instead enter the **Client secret** by reference:
- **Client secret**: Use the drop-down to select an existing stored `client_secret` to pass in the OAuth request parameter. You can use the adjacent **Create** button to store a new, reusable secret.
- **Tenant identifier**: Directory ID (tenant identifier) in Microsoft Entra ID.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.

##### OAuth Certificate Authentication {#oauth-cert}

Selecting **OAuth (certificate)** exposes two of the same controls as the above OAuth methods (**Tenant identifier** and **Client ID**). But this authentication method – available in Cribl Stream 4.1.3 and newer – foregrounds controls to define the certificate you're using to authenticate. Treat all fields here as required, except for the optional **Passphrase**:
- **Certificate name**: Use the drop-down to select an existing certificate to pass in the OAuth request parameter. You can use the adjacent **Create** button to store a new, reusable cert.
- **Private key path**: Path to the private key to use. The key should be in PEM format. Can reference `$ENV_VARS`.
- **Passphrase**: If your private key requires a passphrase to decrypt it, enter that passphrase here.
- **Certificate path**: Path to the certificate to use. The cert should be in PEM format. Can reference `$ENV_VARS`.
- **Tenant identifier**: Directory ID (tenant identifier) in Microsoft Entra ID.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.
- **Subscription Plan**: Select the Office 365 subscription plan for your organization. Options include:
  - **Office 365 Enterprise**
  - **Office 365 GCC**
  - **Office 365 GCC High**
  - **Office 365 DoD**


##### About Polling Intervals {#intervals}

To poll the Office 365 Message Trace API, Cribl Stream uses the **Poll interval** field's value to establish the cron schedule. For example, `*/${interval} * * * *`.

Because the interval is set in minutes, it must divide evenly into 60 minutes to create a predictable schedule. Dividing 60 by intervals like `1`, `2`, `3`, `4`, `5`, `6`, `10`, `12`, `15`, `20`, or `60` itself yields an integer, so you can enter any of these values. 

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

**Request timeout (secs)**: Maximum time to wait for an individual Message Trace API request to complete. Defaults to `300` seconds (5 minutes). Enter `0` to disable metering, allowing unlimited response time. Because there is a single request to the Message Trace API per page of data, this timeout is applied at the page (request) level.

**Reschedule tasks**: Whether to automatically reschedule tasks that failed with non-fatal errors. Defaults to toggled on; does not apply to fatal errors.

**Task reschedule limit**: Maximum number of times a task can be rescheduled. Defaults to `1`.

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Disable time filter**: Disables Collector event time filtering when a date range is specified in [General Settings](#general-settings). Toggle off to allow filtering.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

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

The Office 365 Message Trace Source uses a scheduled REST Collector. It runs one collection task every **Poll interval**, and a single Worker will process the collection. The data is paginated, so the Worker might make multiple calls to fetch the data.

## Viewing Scheduled Jobs

This Source executes Cribl Stream's [scheduled collection jobs](collectors#scheduled). Once you've configured and saved the Source, you can view those jobs' results by reopening the Source's config modal and clicking its **Job Inspector** tab.

You can also view these jobs (among scheduled jobs for other Collectors and Sources) in the **Monitoring** > **System** > **Job Inspector** > **Currently Scheduled** tab.

#### Mitigating Stuck-Job Problems

Occasionally, a scheduled job fails, but continues running for hours or even days, until someone intervenes and cancels it. If left alone, such a "stuck", "orphaned," or "zombie" job will never complete. This can cause missing events in downstream receivers, along with `HTTP timeout` or similar errors in Cribl Stream's logs.

To keep stuck jobs from running excessively long:

* First, try setting **Advanced** > **Timeout (secs)** to a duration shorter than the default of `600` seconds (10 minutes).
* If adjusting **Timeout (secs)** does not fix the problem, try setting **Advanced** > **Job Timeout** – whose default of `0` allows a job to run indefinitely – to a desired maximum duration. 

Using these settings in tandem works like this:

* **Timeout (secs)** limits the time that Cribl Stream will wait for an HTTP request to complete.
* Then, if a job gets stuck and keeps running beyond that limit, **Job Timeout** can catch and terminate the job, because it monitors the overall time the job has been running.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## See Also {#see-also}

The Community-contributed [Microsoft Office Activity & Azure Logs Pack](https://packs.cribl.io/packs/microsoft-activity) provides sample Pipelines to parse and streamline ingested Office 365 and EntraID/Azure AD data. You can import these and adapt them to your own needs.

## Troubleshooting

[Snippet not found: content/shared/4.14/snippets/_sources-troubleshooting.md]

## Common Issues

### Job Fails After One Hour with statusCode: 401

A collection job fails after one hour with an HTTP error, `statusCode: 401`. This error could indicate an authentication failure, like an expired access token.

The cause could be from the temporary access token provided by Microsoft during the OAuth 2.0 authentication process. This token has a one-hour time to live (TTL) that is not configurable. Any collection job that exceeds this one-hour window is unable to refresh its authentication and fails.

To prevent job failure due to token expiration, you can:

* Limit the time range of the collection job to ensure it completes within the one-hour timeframe.
* Configure multiple Office 365 Message Trace Sources, each with a distinct, non-overlapping time range to sequentially collect the data in increments. This approach could duplicate events if the time ranges are not carefully managed.

[Snippet not found: content/shared/4.14/snippets/_common-errors-http-based-source.md]