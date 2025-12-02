# Cribl Search 4.2.2


## New Features

### Query Language

The new [mv-expand](mv-expand) operator expands an array or object within an event into multiple events, where each of the generated events contains a single value of the original array or object.

The new [externaldata](externaldata) operator fetches external data from HTTP(S) URLs.

The [lookup](lookup) operator can match fields with different names with two new supported syntax variations, map and join.

The [export](export) operator that creates or updates a lookup table now supports compression.

The [send](send) operator now includes start and end messages for reporting its status. These messages indicate the initiation and completion of the operator, point out any process errors, and contribute to monitoring the overall process status.

### Date and Time Range Enhancements

You can now hover over the time selector field to activate the copy and paste icons, allowing you to quickly copy defined time ranges into other Cribl Search browser windows.

A new **Relative** tab allows you to select time ranges that do not end with [now](now). For example, you can define a search from 10 minutes ago to 5 minutes ago instead of from 10 minutes ago to [now](now) (like in the **Date Range** tab).

### Dashboards

You can now export [Dashboard panels](dashboards) as .jpg or .png files.

