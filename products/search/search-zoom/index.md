# Searching Zoom API


Learn how to search Zoom API endpoints.

---

Cribl Search supports searching
[Zoom API](https://developers.zoom.us/docs/api/rest/reference/zoom-api/methods/#overview) endpoints. You need a
Dataset Provider and a Dataset, see the setup guide for [Zoom API](set-up-zoom) for details.

This document provides examples of searching endpoints from the Zoom API. To start, navigate to **Search Home**, where [searches](build-a-search) are run.

## Examples {#examples}

The following examples reference a Dataset with the **ID** `myzoom`.

### User API Search

This search specifies the Dataset and aggregates the results by timezone.

```kusto
dataset="myzoom" endpointName="users"
| summarize count () by timezone
```

![Sample Zoom Search: Aggregate by Timezone](search-zoom-basic-search.png)
{scale="100%"}

### Meeting API Search

This search hits the `pastMeetings` API endpoints and summarizes the results by topic.

```kusto
dataset="zoom_cribl1" endpointName="pastMeetings"
| summarize count () by topic
| order by count_desc
```

![Sample Zoom Search: Past Meetings by Topic](search-zoom-basic-search2.png)
{scale="100%"}

Cribl Search comes with rich [charting options](charting) out of the box, allowing you to adjust Charts as needed.

