# Office 365 Message Trace


Cribl Stream supports receiving [Office 365 Message Trace](https://techcommunity.microsoft.com/t5/azure-sentinel/office-365-email-activity-and-data-exfiltration-detection/ba-p/1169652) data. This mail-flow metadata can be used to detect and report on malicious activity including bulk emails, spoofed-domain emails, and data exfiltration.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
{.box .info}

TLS is enabled via the HTTPS protocol on this Source's underlying REST API. 

## Office 365 Setup {#o365-setup}

At a minimum, your Office 365 service account should include a role with `Message Tracking` and `View‑Only Recipients` permissions, assigned to the Office 365 user that will integrate with Cribl Stream. Assign these permissions in the Exchange admin center (https://admin.exchange.microsoft.com).

### Modern Authentication (OAuth 2.0) Setup {#o365-setup-oauth}

If you plan to use [OAuth](#oauth) or [OAuth Secret](#oauth-secret) authentication, your Office 365 setup must include at least one role with the corresponding required permissions:



1. In the [Azure portal](https://portal.azure.com/), create a Microsoft Entra ID App Registration. (For details, see documentation from [Microsoft](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal), [Splunk](https://github.com/splunk/splunk-add-on-microsoft-azure/wiki/Create-an-Azure-AD-App-Registration), or [Splunkbase](https://splunkbase.splunk.com/app/3720/#/details).)

2. Assign the application at least one Azure AD role that will enable it to access the Reporting Web Service:  
   - Global Reader
   - Global Administrator
   - Exchange Administrator

Make sure that at least one role includes the `ReportingWebService.Read.All` permission. For detailed steps, see Microsoft's [Assign  Azure AD Roles to Users](https://docs.microsoft.com/en-us/azure/active-directory/roles/manage-roles-portal) topic.

## Configuring Cribl Stream to Receive Office 365 Message Trace Data   

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Office 365** > **Message Trace**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] **Office 365** > **Message Trace**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Office 365 Message Trace definition.

**Report URL**: Enter the URL to use when retrieving report data. Defaults to: `https://reports.office365.com/ecp/reportingwebservice/reporting.svc/MessageTrace`.

**Poll interval**: How often (in minutes) to run the report. Must divide evenly into 60 minutes to create a predictable schedule, or Save will fail. See [About Polling Intervals](#intervals) below.

### Authentication Settings

In the **Authentication** section, use the buttons to select an **Authentication method**: [Basic](#basic), [Basic (credentials secret)](#basic-secret), [OAuth](#oauth), or [OAuth (text secret)](#oauth-secret). The default is **OAuth**. Both OAuth options rely on the OAuth 2.0 protocol.

##### Basic Authentication {#basic}

Selecting **Basic** exposes **Username** and **Password** fields, where you directly enter the HTTP Basic credentials to use on Message Trace API calls.

##### Basic Secret Authentication {#basic-secret}

Selecting **Basic (credentials secret)** exposes a **Credentials secret** drop-down, where you select an existing [stored secret](/stream/securing-and-monitoring#secrets) that references your credentials on the Message Trace API. You can use the adjacent **Create** button to store a new, reusable secret.

##### OAuth Authentication {#oauth}

The default **OAuth** authentication method exposes the following fields, all required:
- **Client secret**: Directly enter the `client_secret` to pass in the OAuth request parameter.
- **Tenant identifier**: Directory ID (tenant identifier) in Azure Active Directory.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: https://outlook.office365.com.

##### OAuth Secret Authentication {#oauth-secret}

Selecting **OAuth (text secret)** exposes three of the same controls as the default [OAuth](#oauth) method, but – as you'd expect – you instead enter the **Client secret** by reference:
- **Client secret**: Use the drop-down to select an existing stored `client_secret` to pass in the OAuth request parameter. You can use the adjacent **Create** button to store a new, reusable secret.
- **Tenant identifier**: Directory ID (tenant identifier) in Azure Active Directory.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: https://manage.office.com.

### Optional Settings

**Date range start**: Backward offset for the head of the search date range. (E.g., `-3h@h`.) Message Trace data is delayed; this parameter (with **Date range end**) compensates for delay and gaps.

**Date range end**: Backward offset for the tail of the search date range. (E.g., `-2h@h`.) Message Trace data is delayed; this parameter (with **Date range start**) compensates for delay and gaps.

**Log level**: For data collection's runtime log, set the verbosity level to one of `debug`, `info`, `warn`, or `error`. (If not selected, defaults to `info`.)

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

##### About Polling Intervals {#intervals}

To poll the Office 365 Message Trace API, Cribl Stream uses the **Poll interval** field's value to establish the cron schedule. (e.g.: `*/${interval} * * * *`).

Because the interval is set in minutes, it must divide evenly into 60 minutes to create a predictable schedule. Dividing 60 by intervals like `1`, `2`, `3`, `4`, `5`, `6`, `10`, `12`, `15`, `20`, or `60` itself yields an integer, so you can enter any of these values. 

Cribl Stream will reject intervals like `23`, `42`, or `45`, or `75` – which would yield non-integer results, meaning unpredictable schedules.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Keep Alive time (seconds)**: How often Workers should check in with the scheduler to keep their job subscription alive. Defaults to `60`.

**Worker timeout (periods)**: The number of Keep Alive Time periods before an inactive Worker will have its job subscription revoked. Defaults to `3`.

**Timeout (secs)**: Maximum time to wait for an individual Message Trace API request to complete. Defaults to `600` seconds (10 minutes). Enter `0` to disable metering, allowing unlimited response time. Because there is a single request to the Message Trace API per page of data, this timeout is applied at the page (request) level.



**Disable time filter**: Disables Collector event time filtering when a date range is specified in [General Settings](#general-settings). Toggle to `No` to allow filtering.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

  * `__final`
  * `__inputId`
  * `__isBroken`
  * `__source`

## How Cribl Stream Pulls Data

The Office 365 Message Trace Source uses a scheduled REST Collector. It runs one collection task every **Poll interval**, and a single Worker will process the collection. The data is paginated, so the Worker might make multiple calls to fetch the data.

## Viewing Scheduled Jobs

This Source executes Cribl Stream's [scheduled collection jobs](collectors#scheduled). Once you've configured and saved the Source, you can view those jobs' results by reopening the Source's config modal and clicking its **Job Inspector** tab.

You can also view these jobs (among scheduled jobs for other Collectors and Sources) in the **Monitoring** > **System** > **Job Inspector** > **Currently Scheduled** tab.
