# Cribl Lake 4.12.2


Cribl Lake 4.12.2 enables you to run Cribl Search queries against multiple Datasets simultaneously, at Lakehouse speed. It also provides several Lake-specific corrections. 

## New Features

This release includes the following enhancement:

### Multiple-Dataset Lakehouse Searches

A [Cribl Search](/search/) query against multiple [Lakehouse](lakehouse)-assigned Datasets can now execute at Lakehouse speed. (Previously, the Lakehouse performance boost worked only with one Dataset at a time). Beyond the Lakehouse assignment, your query must also meet one of these conditions:

- Include only operators from the following group: [`cribl`](/search/cribl), [`centralize`](/search/centralize), [`extend`](/search/extend), [`extract`](/search/`extract), [`foldkeys`](/search/foldkeys), [`limit`](/search/limit), [`mv-expand`](/search/mv-expand), [`pivot`](/search/pivot), [`project`](/search/project), [`project-away`](/search/project-away), [`project-rename`](/search/project-rename), [`search`](/search/search), and [`where`](/search/where).

- Or, if you use any other operators, insert the [`centralize`](/search/centralize) or [`limit`](/search/limit) operator before them. (Result counts will be further constrained by [usage group](/search/usage-groups#usage-limits) limits.)

If neither of the above conditions is met, or if your query includes non-Lakehouse Datasets, the query will run at standard speed.

## Corrections

This release includes the following fixes:

| ID | Description |
| -- | ----------- |
| <div style="width: 100px">LAKE-868</div> | Cribl Search now returns results, as intended, for DDSS-formatted Cribl Lake Datasets populated using [Direct Access](direct-access). |
| LAKE-850 | Cribl Lake Datasets whose names begin with a number can now be assigned to [Lakehouses](lakehouse/). |
| LAKE-883 | The Lakehouse [Monitoring dashboard](lakehouse#monitor-lakehouse-usage) now correctly refreshes when you switch between Lakehouses. |
| LAKE-876 | Adding new Lake Datasets on the [Free plan](https://cribl.io/pricing/lake/) no longer results in errors. |
