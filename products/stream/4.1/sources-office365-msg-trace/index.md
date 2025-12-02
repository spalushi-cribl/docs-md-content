# Office 365 Message Trace


Cribl Stream supports receiving [Office 365 Message Trace](https://techcommunity.microsoft.com/t5/azure-sentinel/office-365-email-activity-and-data-exfiltration-detection/ba-p/1169652) data. This mail-flow metadata can be used to detect and report on malicious activity including bulk emails, spoofed-domain emails, and data exfiltration.

> Type: **Pull**  |  TLS Support: **YES** | Event Breaker Support: **YES**
>
{.box .info}

TLS is enabled via the HTTPS protocol on this Source's underlying REST API. 

## Office 365 Setup {#o365-setup}

At a minimum, your Office 365 service account should include a role with `Message Tracking` and `View‑Only Recipients` permissions, assigned to the Office 365 user that will integrate with Cribl Stream. Assign these permissions in the Exchange admin center (https://admin.exchange.microsoft.com).

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

## Configuring Cribl Stream to Receive Office 365 Message Trace Data   

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Office 365** > **Message Trace**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] **Office 365** > **Message Trace**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Office 365 Message Trace definition.

**Report URL**: Enter the URL to use when retrieving report data. Defaults to: `https://reports.office365.com/ecp/reportingwebservice/reporting.svc/MessageTrace`.

**Poll interval**: How often (in minutes) to run the report. Must divide evenly into 60 minutes to create a predictable schedule, or Save will fail. See [About Polling Intervals](#intervals) below.

### Authentication Settings

In the **Authentication** section, use the buttons to select an **Authentication method**: [Basic](#basic), [Basic (credentials secret)](#basic-secret), [OAuth](#oauth), [OAuth (text secret)](#oauth-secret), or [OAuth Certificate](#oauth-cert). The default is **OAuth**. All OAuth options rely on the OAuth 2.0 protocol.

##### Basic Authentication {#basic}

Selecting **Basic** exposes **Username** and **Password** fields, where you directly enter the HTTP Basic credentials to use on Message Trace API calls.

##### Basic Secret Authentication {#basic-secret}

Selecting **Basic (credentials secret)** exposes a **Credentials secret** drop-down, where you select an existing [stored secret](/stream/securing-and-monitoring#secrets) that references your credentials on the Message Trace API. You can use the adjacent **Create** button to store a new, reusable secret.

##### OAuth Authentication {#oauth}

The default **OAuth** authentication method exposes the following fields, all required:
- **Client secret**: Directly enter the `client_secret` to pass in the OAuth request parameter.
- **Tenant identifier**: Directory ID (tenant identifier) in Azure Active Directory.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.

##### OAuth Secret Authentication {#oauth-secret}

Selecting **OAuth (text secret)** exposes three of the same controls as the default [OAuth](#oauth) method, but – as you'd expect – you instead enter the **Client secret** by reference:
- **Client secret**: Use the drop-down to select an existing stored `client_secret` to pass in the OAuth request parameter. You can use the adjacent **Create** button to store a new, reusable secret.
- **Tenant identifier**: Directory ID (tenant identifier) in Azure Active Directory.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.

##### OAuth Certificate Authentication {#oauth-cert}

Selecting **OAuth (certificate)** exposes two of the same controls as the above OAuth methods (**Tenant identifier** and **Client ID**). But this authentication method – available in Cribl Stream 4.1.3 and later – foregrounds controls to define the certificate you're using to authenticate. Treat all fields here as required, except for the optional **Passphrase**:
- **Certificate name**: Use the drop-down to select an existing certificate to pass in the OAuth request parameter. You can use the adjacent **Create** button to store a new, reusable cert.
- **Private key path**: Path to the private key to use. The key should be in PEM format. Can reference `$ENV_VARS`.
- **Passphrase**: If your private key requires a passphrase to decrypt it, enter that passphrase here.
- **Certificate path**: Path to the certificate to use. The cert should be in PEM format. Can reference `$ENV_VARS`.
- **Tenant identifier**: Directory ID (tenant identifier) in Azure Active Directory.
- **Client ID**: The `client_id` to pass in the OAuth request parameter.
- **Resource**: Resource parameter to pass in the OAuth request parameter. Defaults to: `https://outlook.office365.com`.

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

**Job timeout**: Maximum time a job is allowed to run. Defaults to `0`, for unlimited time. Units are seconds if not specified. Sample entries: `30`, `45s`, `15m`.

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

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Retry Logic

If this Source receives an  HTTP `429` response code, and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Stream will use the header's `delay-seconds` or `http-date` value to determine how long to wait before retrying the request. Cribl Stream will log a warning message with the value retrieved from the header. 

If the `retry-after` header is absent, Cribl Stream will make five retries, using a backoff algorithm, at the following intervals (in milliseconds): `200`, `400`, `800`, `1600`, `3200`.
