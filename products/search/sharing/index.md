# Sharing Search Resources


Share your Datasets, Dataset Providers, search results, and Dashboards, with other Search Members.

---

Cribl Search enables Admins to share Datasets (including [Cribl Lake Datasets](/lake/datasets)), Dataset Providers, Dashboards, and search results with other Members and Teams of your Organization. There are two layers of Permissions for Members and Teams, and these determine what resources are available to them.

At the product level, a Search Member or Team can have the **User**, **Editor**, or **Admin** Permission. For details,
see [Search Member Permissions](cloud-managing#search-member-roles).

At the resource level, Members and Teams can be assigned the **Maintainer**, **Read Only**, or **No Access** Permission
on Datasets and Dataset Providers.

## Share Datasets and Dataset Providers {#datasets}

Members and Teams with the Search **User** [Permission](cloud-managing#search-member-roles) do not, by default, have access to view, edit, or search Datasets and Dataset Providers. (**Editor** and **Admin** Members/Teams do have such access by default – they automatically inherit the **Maintainer** Permissions on resources at this level.)

As an Admin, here is how to share a Dataset or Dataset Provider with a Member or Team whose Search Permission is **User**: 

1. Select **Data** from Cribl Search's sidebar. 

1. Select the **Datasets** or **Dataset Providers** upper tab.

1. In the **Actions** column of the resource you want to share, select **Share**.

In the resulting **Sharing** drawer, for Members or Teams whose Search Member Permission is **User**, the **Dataset Permission** drop-down enables you to select one of these resource-level Permissions:

- **Maintainer**: Full access to manage and search.
- **Read Only**: Can view and search configurations, but not modify them.
- **No access**: Cannot view, edit, or search.

### Revoke Dataset Access {#revoke}

If an Admin un-shares a Dataset from a User or Team, or removes a User from an authorized Team, the User's past interaction with the Dataset determines their remaining access.

If a User has performed a search against the Dataset, they retain access to its data and [search results](#results) in the [search history](search-overview#history).

However, the Dataset itself is no longer visible to the User, and the User cannot re-run searches against that Dataset.

## Share Search Results {#results}

You can set search results as either **Private** (the default) or **All Members**. **Private** results are visible only to you, and are hidden from other Members and Teams.

After running a search and viewing its results, you can open the **Actions** drop-down to change the search's **Results Access**.


![Share Search Results](search-share-search-results.png)
{scale="40%"}

You can also change the visibility of your searches with a [`set`](set) statement, using the
[`job_visibility`](set#job_visibility) option.


> The **Actions** menu provides more options on the **Fields** and **Charts** tab than are displayed in the above screenshot, which is from the **Events** tab.
>
{.box .info}

### Share Scheduled Search Results {#results-scheduled}

Open a [scheduled search](scheduled-searches)'s configuration modal to adjust its **Results Access** setting. Your saved setting applies to any subsequently created result sets from this search.


![Managing access to a scheduled search](search-scheduled-access.png)
{scale="100%"}

### Share Search Results via URL {#results-url}

You can share displayed search results by copying the URL from your browser's address bar.

This URL will encapsulate the search's details and state – the query string, time-range selection, sample-ratio
selection, and any chart and table settings applied in aggregated results.

Another Search Member, with appropriate Permissions, can paste this URL into their browser's address bar to reload the
search. This is equivalent to loading a cached search from History, so it consumes no additional credits.

## Share Dashboards {#dashboards}

You can share any [Dashboard](dashboards) on which you have the **Owner** or **Maintainer** Permission with other users.

To do this, select **Dashboards** from Cribl Search's sidebar, and locate your Dashboard. Select this Dashboard's Actions
![](actions-dropdown.light.svg) button, and select **Share**.

In the resulting drawer, for Members or Teams whose Search Member Permission is **User**, the **Dataset Permission** drop-down enables you to select one of these Permissions on the Dashboard:

- **Maintainer**: Can edit, share, and delete the Dashboard.
- **Read Only**: Can view the Dashboard.
- **No Access**: Cannot view or edit the Dashboard.


![Sharing a Dashboard](search-dashboard-sharing-4.9.png)
{scale="100%"}
