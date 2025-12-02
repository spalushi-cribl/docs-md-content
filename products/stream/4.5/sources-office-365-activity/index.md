# Office 365 Activity


Cribl Stream supports receiving data from the [Office 365 Management Activity API](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-reference). This facilitates analyzing actions and events on Azure Active Directory, Exchange, and SharePoint, along with global auditing and Data Loss Prevention data.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
{.box .info}

TLS is enabled via the HTTPS protocol on this Source's underlying REST API.

## Azure AD Permissions

In Azure Active Directory, the application representing your Cribl Stream instance must be granted the following permissions to pull data. Each permission's **Type** must be `Application` – `Delegated` is not sufficient:


- `ActivityFeed.Read` – Required for all Content Types except `DLP.All`.
- `ActivityFeed.ReadDlp` – Required for the `DLP.All` Content Type.

![Registered application permissions](ofc-365-activity-permissions.4c44fd790e.png)
{border="true"}



## Office 365 Subscriptions

Cribl Stream does not support starting/stopping Office 365 subscriptions. You can start subscriptions either via another Office 365 API client, or simply via `curl` commands. We document the `curl` command method below in [Starting Content Subscriptions](#start-subscriptions).

## Configuring Cribl Stream to Receive Data from the Activity API   

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Office 365**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] *Office 365**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Office 365 Activity definition.

**Tenant ID**: Enter the Office 365 Azure tenant ID.

**App ID**: Enter the Office 365 Azure application ID.

**Subscription Plan**: Select the Office 365 subscription plan for your organization. Options include:
- **Office 365 Enterprise**
- **Office 365 GCC**
- **Office 365 GCC High**
- **Office 365 DoD**

### Authentication Settings

**Authentication method**: Select one of the following buttons.
 - **Manual**: This default option provides a **Client secret** field, where you directly enter the required Office 365 Azure client secret.
 - **Secret**: This option instead exposes a **Client secret (text secret)** drop-down, from which you select a stored text secret to authenticate with. Click **Create** to configure a new secret.

### Optional Settings

**Publisher identifier**: Use in API requests as described [here](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-reference#start-a-subscription). If not defined, defaults to Microsoft Office 365 tenant ID.

**Content Types**: See the [Content Types](#content-types) section below.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

#### Content Types {#content-types}

Here, you can configure polling independently for the following types of audit data from the Office 365 Management Activity API:

* **Active Directory**
* **Exchange**
* **SharePoint**
* **General**: All workloads not included in the above content types
* **DLP.All**: Data Loss Prevention events only, for all workloads
   
For each of these content types, the **Content Types** table provides the following controls:

**Interval Description**: This column is informational only.

**Interval**: Optionally, override the default polling interval. See [About Polling Intervals](#intervals) below.

**Log Level**: Set the verbosity level to one of `debug`, `info` (the default), `warn`, or `error`.

**Enabled**: Toggle this to `Yes` for each service that you want to poll.

##### About Polling Intervals {#intervals}

To poll the Office 365 Management Activity API, Cribl Stream uses the **Interval** field's value to establish the search date range and the cron schedule (e.g.: `*/${interval} * * * *`).

Therefore, intervals set in minutes must divide evenly into 60 minutes to create a predictable schedule. Dividing 60 by intervals like `1`, `2`, `3`, `4`, `5`, `6`, `10`, `12`, `15`, `20`, or `60` itself yields an integer, so you can enter any of these values. 

Cribl Stream will reject intervals like `23`, `42`, or `45`, or `75` – which would yield non-integer results, meaning unpredictable schedules.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Retries {#retries}

**Retry type**: The algorithm to use when performing HTTP retries. Options include `Backoff` (the default), `Static`, and `Disabled`.

**Initial retry interval (ms)**: Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute). A value of `0` means retry immediately until reaching the retry limit in **Max retries**. 

**Max retries**: Maximum number of times to retry a failed HTTP request. Defaults to `5`. Maximum: `20`. A value of `0` means don't retry at all.

**Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means that Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.

**Retry HTTP codes**: List of HTTP codes that trigger a retry. Leave empty to use the defaults (`429` and `503`). Cribl Stream does not retry codes in the `200` series.

**Honor Retry-After header**: When toggled to `Yes` (the default) and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Stream always ignores `retry-after` headers that specify a delay longer than 20 seconds.

Cribl Stream will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).

When toggled to `No`, Cribl Stream ignores all `retry-after` headers.

### Advanced Settings

**Ingestion lag (minutes)**: How far back into the past the Office 365 Activity API should look for events to retrieve. This is necessary because there can be a lag of 60 to 90 minutes (or longer) before Office 365 events become available via the API. The default value of `0` means don't look back. When it's very important to avoid missing events, consider running multiple overlapping jobs, as described [below](#overlapping). 

**Keep alive time (seconds)**: How often Workers should check in with the scheduler to keep their job subscription alive. Defaults to `30`.

**Worker timeout (periods)**: The number of **Keep alive time** periods before an inactive Worker will have its job subscription revoked. Defaults to `3`.

**Request timeout (secs)**: The maximum time period for an HTTP request to complete before Cribl Stream treats it as timed out. Defaults to `300` (i.e., 5 minutes). Enter `0` to disable timeout metering.

**Job timeout**: Maximum time the job is allowed to run (e.g., `30`, `45s`, or `15m`). Units are seconds, if not specified. Enter `0` for unlimited time. Defaults to `0`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
  * `__collectible`
  * `__final`
  * `__inputId`
  * `__isBroken`
  * `__source`

##  Starting Content Subscriptions  {#start-subscriptions}

Content subscriptions (a different concept from the O365 subscription plans) are required in order for Cribl Stream to be able to begin retrieving O365 data. There is a separate subscription required for each Content Type. If you are using an existing Azure-registered application ID that already has subscriptions started, then you can ignore this section. But if you are:

- Using a newly registered application ID, and therefore never had any subscriptions started, or 
- Reusing an application ID that had subscriptions started, but are currently stopped

...then you will need to use this procedure to manually start the necessary subscriptions. Follow either of the two methods below, using (respectively) PowerShell or `curl`.

### Using PowerShell

This sample PowerShell script will enable all subscriptions for you. Update the appropriate variables as required:

```shell
# Create app of type Web app / API in Azure AD, generate a Client Secret, and update the client id and client secret here
# Get the tenant GUID from Properties | Directory ID under the Azure Active Directory section.
$AppID = "<APP_ID>"
$ClientSecret = "<CLIENT_SECRET>"
$TenantID = "<TENANT_ID>"
$loginURL = "https://login.microsoftonline.com/"

# For $resource, use one of these endpoint values based on your subscription plan:
# * Enterprise - manage.office.com
# * GCC - manage-gcc.office.com
# * GCC High - manage.office365.us
# * DoD - manage.protection.apps.mil
$resource = "https://manage.office.com"

$body = @{grant_type="client_credentials";resource=$resource;client_id=$AppID;client_secret=$ClientSecret}
$oauth = Invoke-RestMethod -Method Post -Uri $loginURL/$TenantID/oauth2/token?api-version=1.0 -Body $body
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

Invoke-WebRequest -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/list"

Invoke-WebRequest -Method Post -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/start?contentType=Audit.Exchange"
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/start?contentType=Audit.SharePoint"
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/start?contentType=Audit.General"
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "$resource/api/v1.0/$TenantID/activity/feed/subscriptions/start?contentType=DLP.All"
```

### Using curl

This is a two-step process. The first command obtains an auth token, which is used in the second command to actually start the subscription. To execute these commands, you'll need the same information (i.e., client secret, application ID, and tenant ID) that you already require to configure this Source in Cribl Stream's GUI. Replace those three variables as appropriate in the commands below.

1. `curl -d "client_secret=<client secret>&resource=https://manage.office.com&client_id=<app id>&grant_type=client_credentials" -X POST https://login.windows.net/<tenant id>/oauth2/token`

2. `curl -d "" -H "Authorization: Bearer <access token>" -X POST https://manage.office.com/api/v1.0/<tenant id>/activity/feed/subscriptions/start?contentType=<content_type_name>`

Here is an example of each command executed and expected output:

#### Example Command #1

`$ curl -d "client_secret=abcdefghijklmnopqrstuvwxyz12345678&resource=https://manage.office.com&client_id=00000000-ffff-ffff-ffff-aaaaaaaaaaaa&grant_type=client_credentials" -X POST https://login.windows.net/12345678-aaaa-4233-cccc-160c6c30154a/oauth2/token`

##### Output:

`{"token_type":"Bearer","expires_in":"3599","ext_expires_in":"3599","expires_on":"1622089429","not_before":"1622085529","resource":"https://manage.office.com","access_token":"eyJ0...long JWT here...MRDvw"}`

#### Example Command #2

`$ curl -d "" -H "Authorization: Bearer eyJ0...long JWT here...MRDvw" -X POST "https://manage.office.com/api/v1.0/12345678-aaaa-4233-cccc-160c6c30154a/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"`

##### Output:

`{"contentType":"Audit.AzureActiveDirectory","status":"enabled","webhook":null}`

Note there is no output when executing this second command with a `stop` operation.

You'll need to execute the second command for each Content Type whose logs you wish to collect. Use the exact strings below to specify Content Types in that command:

- `Audit.AzureActiveDirectory`
- `Audit.Exchange`
- `Audit.SharePoint`
- `Audit.General`
- `DLP.All`

## How Cribl Stream Pulls Data

The Office 365 Activity Source retrieves data using Cribl Stream [scheduled Collection jobs](collectors#scheduled), which include Discover and Collection phases. The Discover phase task returns the URL of the content to collect.

In the Source's **General Settings** > **Content Types** > **Interval** column, you configure the polling schedule for each Content Type independently. Each Content Type that you enable gets its own separate scheduled job.

The job scheduler spreads the Collection tasks across all available Workers. The collected content is paginated, so the collection phase might include multiple calls to fetch data.

## Viewing Scheduled Jobs

Once you've configured and saved the Source, you can view the Collection jobs' results by reopening the Source's config modal and clicking its **Job Inspector** tab.

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

## Multiple Overlapping Jobs {#overlapping}

You can schedule multiple Collection jobs where each job uses a different ingestion lag, e.g., three different jobs with **Ingestion lag** set to `120` (two hours), `720` (twelve hours), and `5760` (four days), respectively.

Although this approach will collect data over partially overlapping time ranges – meaning that you'll have duplicate events – it can improve the chances that you'll catch "straggler" events affected by the known [lag times](https://learn.microsoft.com/en-us/microsoft-365/compliance/audit-log-search?view=o365-worldwide) in the Office 365 Management Activity API.
