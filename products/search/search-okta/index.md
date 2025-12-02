# Searching Okta API


Learn how to search Okta API endpoints.

---

Cribl Search supports searching [Okta API](https://developer.okta.com/docs/reference/) endpoints. You need a
Dataset Provider and a Dataset, see the setup guide for [Okta API](set-up-okta) for details.

This document provides examples of searching endpoints from the Okta API. To start, navigate to **Search Home**, where [searches](build-a-search) are run.

## Examples {#examples}

The following examples reference a Dataset with the **ID** `okta_dataset`.

### Users API Search

This search interrogates the `users` endpoint for active and expired passwords.

```kusto
dataset="okta_dataset" endpointName="users"
| summarize count () by status
```

![Sample Okta Search: User status](search-okta-basic-search.png)
{scale="100%"}

### Groups API Search

This search hits the `groups` API endpoints and summarizes the results by type.

```kusto
dataset="okta_dataset" endpointName="groups"
| summarize count () by type
```

![Sample Okta Search: Groups by Type](search-okta-basic-search2.png)
{scale="100%"}

Cribl Search comes with rich [charting options](charting) out of the box, allowing you to adjust Charts as needed.

