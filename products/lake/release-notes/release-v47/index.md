# Cribl Lake 4.7


## Datatypes

You can now [associate Datatypes](datasets#datatypes) with Cribl Lake Datasets.

Doing this helps break data into discrete events, timestamps them, and parses them as needed.
Multiple Datatypes can parse events into a fully structured object for use with Cribl Search, improving your search experience.

## Accelerated Datasets

You can now [accelerate](/search/acceleration) Cribl Lake Datasets in the same way you accelerate regular Cribl Search Datasets.
Use acceleration to pre-scan portions of data which speeds up search performance for very large Datasets.

## New Global Navigation
Building on the last release, in which we introduced the new Cribl.Cloud experience with [Workspaces](https://docs.cribl.io/stream/workspaces/), we’ve added a global navigation system. Now when you opt in to the new Cribl.Cloud user interface, you'll get a streamlined global navigation experience. When you open a Cribl product in a Workspace, a product navigation pane on the left helps you move around within that product. Click **Products** to switch to a different product or go back to Cribl.Cloud home.

## Corrections

Exporting search results to a Cribl Lake Dataset using the [`export`](/search/export) operator now includes the `_raw` field.
You can remove this field from results by using the `project-away` operator. For example:

```kusto
dataset="audit_logs" 
| project-away _raw
| export my_lake_dataset
```