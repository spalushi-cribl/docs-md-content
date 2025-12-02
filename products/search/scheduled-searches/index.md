# Scheduled Searches


Run a search at a scheduled frequency, and set up conditional Notifications.

---

Scheduled searches allow you to automate data analysis and save valuable time. By scheduling saved searches you can
effectively monitor systems, optimize workflows, and send Notifications based on the evaluation of search results
against a specific condition. Use scheduled searches to aggregate data, compare results, identify anomalies, and analyze
long-term trends.

For example, instead of manually running a search to gather information about login types and failures from the previous
month, you can schedule a search to automatically aggregate this data at midnight on the first day of each month.

Scheduled searches can also send [Notifications](notifications#configure), based on specific conditions. This helps you stay informed about important events with no manual effort. You can send Notifications to different [targets](notifications#notification-targets), including:




- System Messages: Banner messages in the Cribl Search UI (the default target).
- [Email](email-notifications) messages.
- [Amazon SNS](aws-sns-notification-targets) topics.
- [PagerDuty](pager-duty-notification-targets) accounts.
- [Slack](slack-notification-targets) channels.
- [Webhook](webhook-notification-targets) connections.


## Schedule a Search

To schedule a search, save or edit an existing [saved search](search-overview#saved), and then toggle **Schedule** on.
The option to save a search is available in two places:

- At the bottom right of the query box, under the **Actions** menu. 

- In [**Saved Searches**](search-overview#saved), through the **Add Search** button.

Either way, the resulting modal provides these configuration options:

**Name**: Give the search a meaningful name that will help you identify it easily.

**Description**: A description is optional.

**Results Access**: Defaults to **Private** where only you can view the result set. Select **All Members** to
[share](sharing) the results. When a result set is created its access level is defined. If you edit this setting, only
subsequent results will have the new setting.

You can also use the Cribl API to save and schedule searches. Read [Save Searches and Get Alerts](/cribl-as-code/save-search-get-alerts) for more information.

### Query {#search-string}

**Search string**: This is the query; edit it if needed. For details, see [Build a Search](build-a-search).

- **Sampling**: Sampling uses a ratio to reduce the number of results returned from a search. For example, if a search
  returns 1,000 events, a 1:10 sampling ratio returns 100 events.
- **Time range**: Set the query time range, relative to the time the search is
  scheduled to run. See [time range](build-a-search#time-range) for details.

### Schedule

**Schedule**: Enable **Run on schedule** to configure the run frequency. You can then set the frequency either by using the drop-downs, or by entering a custom [cron expression](https://crontab.cronhub.io/). The cron expression defaults to `0 0 * * *`, running every day at 12:00 AM.

- **Timezone**: Select the TZ for the scheduled search to use. Defaults to UTC.

### Notifications {#notifications}

For details about setting options on the **Notifications** tab, see [Configure Notifications](notifications#configure).

### Advanced

**Advanced** provides a **Keep last executions** control. This determines the minimum number of result sets stored. Every hour, Cribl Search rotates result sets, preserving at least this number of past results. The minimum allowed is `1`, the default is `2`, and the maximum is `1000`. This setting overrides the **Search history maximum jobs** and **Search history TTL** [limits](limits).

## Rules

- You can edit the schedule or query of a scheduled search and it will be used on subsequent runs.
- Daylight savings time is automatically incorporated based on the selected time zone.

## Results {#results}

Result sets from scheduled searches can be utilized by:

- using the [send](send) operator to automatically send the results to Cribl Stream, for further routing and filtering.
- opening the search from the [History](search-overview#history) tab for review.

**History** provides a filter on the top right of the table for **Scheduled** searches. The table provides
details of each scheduled search, including its query, status, user, last run duration, and latest run time.

- Select a row to view the scheduled search's first result set.
- Select **N Items** to view all of the result sets. From the shown result sets:
  - Select a row to view the results.
  - Select the **Search ID** to view the [search's details](search-details), helpful for troubleshooting.
- The **Actions** column has options to **Rerun** and **Save** the search.
- Delete one or many result sets by selecting check boxes on the left of a column and then clicking
  **Delete Selected Jobs** at the bottom left of the table.


![Result Sets](search-history-scheduled-searches.png)
{scale="100%"}

## Manage Scheduled Searches

You can view and manage your scheduled searches from **Saved Searches**. The saved searches table has a **Scheduled**
filter and columns for **Schedule** and **Next Run**.

- Select a row to view, edit, or delete a saved search.
- The **Actions** column has options to **Rerun** and **Clone** the search.
- To stop a scheduled search, toggle its schedule off when editing it.

If you want to view the results of a previously run scheduled search, go to **History**. See the above
[Results](#results) section for details.

