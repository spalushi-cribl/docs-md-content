# Data Operators


A list of data operators supported by Cribl Search.

---

Data operators transform, enrich, or manipulate events, enabling actions like renaming, joining, or exporting.


| Name                               | Description                                                                              |
| ---------------------------------- | ---------------------------------------------------------------------------------------- |
| [`centralize`](centralize)         | Forces subsequent operators to the coordinator.                                          |
| [`export`](export)                 | Exports search results to a [lookup](lookups) or a [Cribl Lake Dataset](/lake/datasets). |
| [`extend`](extend)                 | Appends fields created by expressions.                                                   |
| [`extract`](extract-operator)      | Extracts data from a field.                                                              |
| [`foldkeys`](foldkeys)             | Folds hierarchical field names into a nested structure.                                  |
| [`ip-lookup`](ip-lookup)           | Enriches events with IP address data.                                                    |
| [`join`](join)                     | Merges events from two different data scopes.                                            |
| [`lookup`](lookup)                 | Enriches events with lookup files.                                                       |
| [`mv-expand`](mv-expand)           | Expands an object into multiple events.                                                  |
| [`mv-pull`](mv-pull)               | Pulls key-value pairs from array objects into events or objects/bags.                          |
| [`project-rename`](project-rename) | Renames fields.                                                                          |
| [`union`](union)                   | Appends one set of results to another.                                                   |

