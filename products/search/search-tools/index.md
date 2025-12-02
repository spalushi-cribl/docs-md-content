# Usage Settings


Manage Notification targets, Usage Groups, and capacity limits.

---

You can manage Cribl Search settings (as well as Cribl global settings) by selecting **Settings** from the sidebar. When
you do so from within Cribl Search, the **Settings** > **Search** upper tab automatically gains focus.

You'll now see left tabs where you can configure the Cribl Search–specific settings listed below. (Adjacent tabs provide
access to **Global** settings that govern your whole Organization, and to settings specific to other Cribl products.)

- **[Notification targets](notifications#notification-targets)** receive notifications from
  [scheduled searches](scheduled-searches).
- **[Usage Groups](usage-groups)** define groups for controlling limits.
- **[Limits](limits)** control searches and the UI.


> Certain Cribl Search settings can be overridden by [`set` statements](set) in your queries.
{.box .info}

## Set the Limits of Search Results {#limits}

By default, Cribl Search returns up to **50,000** results per search.

You can change this limit at three levels:

- [Organization](#organization) level (between 1 and 10 million).
- [Usage Group](#usage-group) level (between 1 and the Organization limit).
- [Per-search](#search) level (between 1 and the Usage Group limit).

Certain Dataset Provider types also have their own results limits:

- [Elasticsearch Results Limit](set-up-elasticsearch#limit)
- [OpenSearch Results Limit](set-up-opensearch#limit)

### Set the Results Limit for the Organization {#organization}

A Search Admin can set the system-level limit that applies to all searches in the Organization, by changing the
**Results limit** parameter of the **system** Usage Group:

1. Go to the **Usage Groups** page in Cribl Search: On the top bar, select **Products** > **Search** > **Settings** >
   **Usage Groups**.
1. Select the **system** Usage Group.
1. Change the **Results limit** to any value between `1` and `10000000`.
1. Save your changes.

Now, all searches in your Organization will be limited to the number of results you specified.

### Set the Results Limit for a Usage Group {#usage-group}

A Search Admin can set the results limit for a specific Usage Group. This limit will apply to all searches run by the
group's members, and will be capped by the Organization limit.

1. Go to the **Usage Groups** page in Cribl Search: On the top bar, select **Products** > **Search** > **Settings** >
   **Usage Groups**.
1. Select the Usage Group you want to change.
1. Change the **Results limit** to any value between `1` and `10000000`.
1. Save your changes.

### Set the Results Limit for a Search {#search}

Apart from [using the `limit` operator](your-first-query#rows), any user can set the results limit for a specific search
by adding the [`max_results_per_search`](set#max_results_per_search) option in a `set` statement at the beginning of the
query.

```kusto {runSearch=true}
set max_results_per_search=1000;

dataset="cribl_search_sample"
```

For a regular user, this limit will be capped by their Usage Group limit, which in turn is capped by the Organization
limit. For a Search Admin, the `max_results_per_search` value can override the Organization limit up to the maximum of
10 million.
