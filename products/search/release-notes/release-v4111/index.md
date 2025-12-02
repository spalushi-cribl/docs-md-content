# Cribl Search 4.11.1


Cribl Search 4.11.1 enables you to include search results in all Notification types, use the enhanced Event Details
drawer in the Table view, quickly see which Datasets are linked to a Lakehouse, and more.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl is deprecating [Dataset Acceleration](/search/acceleration) (a Preview feature) in preparation for a different
solution. This feature will be removed in a future release. Please continue to report issues through normal Cribl
[support channels](/support/), but assistance for this deprecated feature might be limited.

### New Cribl.Cloud Domain Rollback

In our previous release, we announced a plan to begin migrating Cribl.Cloud Organizations to a new unified domain
(`https://app.cribl.cloud/`). After evaluating customer feedback and internal considerations, we’ve decided **not to
move forward with this domain migration**.

Your Cribl.Cloud environment will continue to operate under the existing URLs you’ve always used — there is no change to
login or portal access. The previously communicated domain (`app.cribl.cloud`) will not be adopted, and no redirects
will be implemented.

This rollback requires **no action on your part**. All existing functionality and URLs remain unchanged.

## New Features

### Search Results in All Notification Types

You can now include a subset of search results in all of the supported Notification types, including
[Slack](slack-notification-targets), [PagerDuty](pager-duty-notification-targets), and
[Webhook](webhook-notification-targets). Before, you could do this only with [Email](email-notifications) and
[AWS SNS](aws-sns-notification-targets).

## UI/UX Improvements

### Enhanced Event Details View

On the **Search Home** [**Events**](search-overview#events) tab, the **Event Details** drawer is now available from the
**Table** view as well. We've enhanced the drawer with several new features: improved field and syntax highlighting, a
line-number indicator, and new Copy, Pin, and Time Conversion controls.


![Event Details drawer](search-events-drawer-2504.png)
{scale="100%" border="true"}

### Lakehouse Datasets: Indicators/Links

In **Search Home**, [Lakehouse](search-cribl-lake#search-lakehouse)-assigned Datasets now display an indicator that you
can select to navigate to the corresponding Lakehouse in [Cribl Lake](/lake/lakehouse). Also, the **Datasets** page
provides a new Lakehouse column.

### Saved Searches: Notification Links

The [**Saved Searches**](search-overview#saved) page now includes a **Notifications** column with links to any alerts
configured on each search.

### Dashboards: View/Edit Mode Transitions

[Dashboards](dashboards) now provide smoother transitions between View and Edit modes.

## Corrections

| ID                                                                                                           | Description                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <div style="width: 100px"> SEARCH-7367<br>SEARCH-7257<br>[Known issue](known-issues#SEARCH-7367-SEARCH-7257) | The [`distinct`](distinct) operator no longer generates search results previews when the [`disable_previews=true`](set#disable_previews) option is in use.                                                                                                         |
| SEARCH-9587                                                                                                  | Resources within Search [Packs](packs) (like [Dashboards](dashboards) and [Macros](macros)) no longer display an unsupported **Share** option.                                                                                                                     |
| SEARCH-9615<br>SEARCH-8319                                                                                   | You can now export an arbitrary number of events from the **Search Home** page, rather than the prior limit of 50,000 events.                                                                                                                                      |
| SEARCH-9548                                                                                                  | We've removed the limit on how many events [aggregation operators](aggregation-operators) can summarize. The practical limit will now vary with the number and cardinality of the aggregated events. Exceeding the bounds will now trigger an out-of-memory error. |
| SEARCH-9758                                                                                                  | Within [Usage Groups](usage-groups#usage-limits), you can now set a **Results limit** as high as 10,000,000 (10 million) events, up from the previous 200,000 limit.                                                                                               |
