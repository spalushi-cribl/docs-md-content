# Cribl Search 4.8.2


Starting with Cribl Search 4.8.2, you can include search results in AWS SNS notifications, use scheduled searches as
base searches in Dashboards, access multiple previous executions of a scheduled search, and more.



## Scheduled Searches: Retrieve Results from Multiple Previous Runs

Before, when using the [`$vt_results`](vt_results) virtual table to
[retrieve the output of a scheduled search](search-results#reuse-query), you could access only the most recent result
set. Now, you can also view the outputs from earlier runs, by using the new `execution` predicate.

For example, to get the results of the last three executions in total (the latest one plus two earlier ones):

```kusto
dataset="$vt_results" jobName="myLittleJob" execution > -2
```

## Include Search Results in AWS SNS Notifications

[AWS SNS notifications](aws-sns-notification-targets) can now
[include a subset of search results](aws-sns-notification-targets#results) in the notification payload.

## Manage Dashboards as JSON More Easily

You’ll now find it faster to [edit, import, or export your Dashboards as JSON](editing-dashboards#json). You can access
these options directly from the Dashboard's Actions ••• menu.

## Access Search Details Directly from a Dashboard

When viewing a Dashboard visualization, select the new **Open Search Details** option to quickly view the
[**Details**](search-details) of the underlying search.

