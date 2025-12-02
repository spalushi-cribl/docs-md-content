# Wiz API Collection


This topic covers how to configure Cribl Stream [REST Collectors](collectors-rest) and [Event Breakers](event-breakers) to gather data from the [Wiz](https://www.wiz.io/) cloud security platform. The REST Collectors will communicate with APIs that your organization's Wiz portal exposes. You'll create and configure four REST Collectors, one for each of these Wiz APIs: Audit Logs, Configuration Findings (sometimes called Cloud Configuration), Issues, and Vulnerabilities.

Instead of configuring REST Collectors setting-by-setting, you will first download them from Cribl’s [Collector Templates repository](https://github.com/criblio/collector-templates/) and then import them into Cribl Stream using the **Manage as JSON** [feature](collectors#advanced-collector-configuration), replacing placeholders in the configuration with values from your Wiz portal. This approach is faster and less error-prone than clicking your way through the relevant UIs, especially since the configurations required to interact with the Wiz APIs are quite complicated.

## Prerequisites From Wiz {#pre-reqs}

This topic assumes that when you became a Wiz customer, Wiz sent the following items:

Item | Value
--- | ---
**Client ID** | 53-character string
**Client secret** | 64-character string
**Auth URL** | `https://auth.app.wiz.io/oauth/token` or similar
**API endpoint** | `https://api.us17.app.wiz.io/graphql` or similar

These items should be the same across all Wiz APIs. The Wiz platform differentiates requests to one API from requests to another by inspecting the request body. (The configurations supplied later in this topic include an appropriate request body for each API.)

Note the values that Wiz sent you, and keep them handy for when you [configure the REST Collectors](#config-collectors) later on.

## Downloading the Collectors {#download-configs}

Click the links to open the following files from Cribl's Collector Templates repository, and download the files to a convenient location on your local system. 

1. Audit Logs Collector: [`collector-wiz-auditlogentries.json`](https://github.com/criblio/collector-templates/blob/main/collectors/rest/wiz/collector-wiz-auditlogentries.json)
2. Configuration Findings Collector: [`collector-wiz-configurationfindings.json`](https://github.com/criblio/collector-templates/blob/main/collectors/rest/wiz/collector-wiz-configurationfindings.json)
3. Issues Collector: [`collector-wiz-issues.json`](https://github.com/criblio/collector-templates/blob/main/collectors/rest/wiz/collector-wiz-issues.json)
4. Vulnerabilities Collector: [`collector-wiz-vulnerabilities.json`](https://github.com/criblio/collector-templates/blob/main/collectors/rest/wiz/collector-wiz-vulnerabilities.json)

## Configuring the Event Breaker

Although you will create four REST Collectors, you only need to add a single Event Breaker whose ruleset contains four rules – one for each Wiz API – enabling it to break events for all four REST Collectors.

The Event Breaker strips headers from events, leaving only the records of interest; and, it captures timestamps in the proper way.

1. Navigate to **Manage** > **Processing** > **Knowledge** > **Event Breaker Rules**.  
2. Click **Add Ruleset**. 
3. Click **Manage as JSON** at lower left to open the JSON editor.
4. Click [this link](https://github.com/criblio/collector-templates/blob/main/collectors/rest/wiz/breaker.json) to open the Event Breaker file (`breaker.json`) in the Collector Templates repository, then click the **Copy icon** to copy the Event Breaker.
5. Back in the Cribl Stream UI, paste the Event Breaker into the editor (replacing the default object that the editor opens with).
6. Click **Save**.

![The Event Breaker UI before you add the ruleset](wiz-api-01.629b432d96.png)

When you have finished adding the Event Breaker, you should see that it contains four rules:

![The complete Event Breaker ruleset](wiz-api-02.333df859d2.png)

## Configuring the REST Collectors {#config-collectors}

> You'll need to complete the procedure in this section four times. Each time, you'll create one REST Collector to handle a particular Wiz API.
> 
> To learn more about REST Collectors, see our [REST/API Endpoint](collectors-rest) and 
> [Scheduling and Running](collectors-schedule-run) topics.
>
{.box .info}

From the top nav of a Cribl Stream instance or Group, select **Data** > **Sources**, then select **Collectors** > **REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST** > **New Collector** modal.

1. Click **Manage as JSON** to open the configuration editor.
2. Click **Import** and select the desired Collector from among the four you downloaded above: 
   * Audit Logs Collector (`collector-wiz-auditlogentries.json`)
   * Configuration Findings Collector (`collector-wiz-configurationfindings.json`)
   * Issues Collector (`collector-wiz-issues.json`)
   * Vulnerabilities Collector (`collector-wiz-vulnerabilities.json`)
1. In the **Replace Placeholder Values** modal, enter the values you noted [earlier](#pre-reqs) into the appropriate fields:
    * For `loginUrl`, enter the value of **Auth URL**.
    * For `clientSecretParamValue`, enter the value of **Client secret**.
    * In the `authRequestParams` array, find the object where `name` has the value `client_id`, and replace the value of `value` with the value of **Client ID**.
    * For `collectUrl`, enter the value of **API endpoint**.
2. Click **Replace Values** to close the modal.
3. Click **OK** to finish configuring the Collector.

 Once you've created all four REST Collectors, you'll be ready to start collecting from the Wiz APIs.

## Discovering and Collecting

You can perform the procedures in this section with any of the four REST Collectors you've created. To configure time ranges, you'll use **Earliest** and **Latest** values.

* If you enter **Earliest** and **Latest** values, Cribl Stream will pass them into the API query.  
* If you leave these fields blank, Cribl Stream will query the API for the 24 hours before the job is scheduled to run.

We'll start with the Discovery run:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector. 
1. In the resulting **Run Collector** modal, select **Mode** > **Discovery**. 
1. Configure a **Time range**, if desired.
1. Click **Run** to retrieve Discovery results.

After inspecting these results, launch the Collector run:

1. Back on the **Manage REST Collectors** page, again click **Run** beside your new Collector.
1. In the resulting **Run Collector** modal, this time select **Mode** > **Full Run**. 
1. Configure a **Time range**, if desired.
1. Click **Run**.

#### Automating your Collector Runs

1. On the **Manage REST Collectors** page, click **Schedule** in the **Actions** column for your new Collector. The **Run Collector** modal will open.
1. Toggle **Enable** on and configure the scheduling options as desired.
1. Click **Save**.



## Beware of Wiz API Results Limits {#limits}

To avoid degrading the performance of your Wiz environment, follow the principles described in this section.

* Do not exceed three API requests per second. Cribl Stream is generally not used in a manner that would approach this limit but if you do, the Wiz API will likely respond with `HTTP 429 Too Many Request` errors.
* Use filters in the query string to ensure that your queries pull only the relevant information.
* If you choose to capture historical data during your initial integration, keep each API's results limits in mind when running ad-hoc API collections.
* For ongoing collection, use incremental collections. To do this, schedule the REST collector to run over a timeframe that suits the task. For example, the Wiz Vulnerabilities Report API is for incremental use only and is best scheduled to run daily, pulling results only from the previous day.

The table below shows the results limits for each Wiz API. 

API | Limit (results)
--- | ------
Audit Logs | 10,000
Cloud Configuration | 10,000
Issues | no limit
Vulnerability | no limit

> Once your query hits the limit of results for a given Wiz API, the API will send no further results. 
>
{.box .warning}

## Troubleshooting {#troubleshooting}

The REST Collector treats all non-`200` responses from configured URL endpoints as errors. On Discover, Preview, and most Collect jobs, it interprets these as fatal errors. On Collect jobs, a few exceptions are treated as non-fatal:
* Where a Collect job launches multiple tasks, and only a subset of those tasks fail, Cribl Stream places the job in failed status, but treats the error as non-fatal. (Note that Cribl Stream does not retry the failed tasks.)
* Where a Collect job receives a 3xx redirection error code, it follows the error's treatment by the underlying library, and does not necessarily treat the error as fatal.

Detailed logging is available for each Collector run to help troubleshoot issues or explore the results of each collector run.  To view the jobs, click Monitoring->System->Job Inspector as shown below.

![The Job Inspector](wiz-api-03.6df344eeb5.png)
