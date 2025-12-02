# Context Functions


Context functions return contextual information about your search.


| Name                | Description                                                                             |
| ------------------- | --------------------------------------------------------------------------------------- |
| `createdTime()`     | Time when the search was created, in seconds.                                           |
| `earliestTime()`    | Beginning of the search's time range, in seconds.                                       |
| `jobID()`           | Unique search identifier.                                                               |
| `latestTime()`      | End of the search's time range, in seconds.                                             |
| `query()`           | Query string.                                                                           |
| `user()`            | Username of the user who created the search.                                            |
| `displayUsername()` | Friendly display name (typically first + last name) of the user who created the search. |

## Examples

Return multiple details about a search:

```kusto {runSearch=true}
dataset="cribl_search_sample" action=="*"
| summarize eventsPerSecond = (count() / (latestTime() - earliestTime())) by action
| project action, eventsPerSecond, reportTime=createdTime(), reportId=jobID(), reportQuery=query(), reportRunner=strcat(displayUsername()," ",user())
```

Reference the search's time range:

```kusto
dataset=myDataset
| summarize eventsPerSecond = (count() / (latestTime() - earliestTime())) by host
```

Filter events by the current user:

```kusto
dataset=myDataset
| where user == user()
```

Enrich results with search information:

```kusto
dataset=myDataset
| summarize NumTransactions=count(), Total=sum(UnitPrice * NumUnits) by Fruit, StartOfMonth=startofmonth(SellDateTime)
| extend query=query(), searchid=jobID()
```
