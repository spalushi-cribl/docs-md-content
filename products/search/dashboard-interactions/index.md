# Add Interactions to Your Cribl Search Dashboard


Enable Dashboard viewers to drill down into specific values in a visualization panel.

---

## Why Use Dashboard Interactions {#why}

Interactions enable Dashboard viewers to take action directly from visualizations. By clicking on data points, users can
trigger context-driven searches, follow external URLs, or open other Dashboards filtered by the value clicked.

This helps you design a seamless workflow that connects your Dashboard to other Cribl or third-party tools and
resources, resulting in a smooth data experience for people consuming your Dashboards.

## Dashboard Interaction Types {#actions}

An Interaction controls what happens when you click on a specific data point on your Dashboard. Each Interaction can
trigger one of the following actions:

- [Run a new search](#new-search), filling the query with the value clicked and other contextual information.
- [Open an external link](#external-link), enriching the URL with the value clicked or with metadata about the user's
  Cribl.Cloud Workspace.
- [Open another Dashboard](#open-dashboard), populating one of its [Inputs](editing-dashboards#add-inputs) with the
  value clicked or other context.

## Add a Dashboard Interaction {#add-interactions}

{{% tabs "existing" "existing" "To an Existing Visualization"
"newviz" "To a New Visualization"
"newdash" "To a New Dashboard" %}}

{{% tab-item "existing" %}}

1. Go to the **Dashboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. Open a Dashboard.
1. Select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top right of the visualization panel you want to modify, select the **Edit** button. The **Edit
   Visualization** drawer opens.
1. At the bottom of the drawer, select **Interactions**.
1. Configure the Interaction you want. For more information, see [Interaction Types](#actions).
1. At the top of the Dashboard, select **Save**.

{{% /tab-item %}}

{{% tab-item "newviz" %}}

1. Go to the **Dashboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. Open a Dashboard.
1. Select **Edit** ![](edit-button.light.svg) at the top right, or press **E** on your keyboard.
1. At the top, select **Add** > **Visualization**. The **Edit Visualization** drawer opens.
1. At the bottom of the drawer, select the **Interactions** tab.
1. Run the Visualization query and configure the visualization as needed.
1. At the bottom of the drawer, select **Interactions**.
1. Configure the Interaction you want. For more information, see [Interaction Types](#actions).
1. At the top of the Dashboard, select **Save**.

{{% /tab-item %}}

{{% tab-item "newdash" %}}

1. Start [creating a new Dashboard](creating-adding-to-dashboards).
1. Configure the Dashboard, and select **Save**.
1. Select **Add Visualization**. The **Edit Visualization** drawer opens.
1. Run the Visualization query and configure the visualization as needed.
1. At the bottom of the drawer, select **Interactions**.
1. Configure the Interaction you want. For more information, see [Dashboard Interaction Types](#actions).
1. At the top of the Dashboard, select **Save**.

{{% /tab-item %}}

{{% /tabs %}}

## Run a New Search From a Visualization {#new-search}

You can set up a visualization so that clicking on it runs a new search.

For this, add a **Run a New Search** Interaction:

1. Start [adding](editing-dashboards#add) or [editing](editing-dashboards#edit) a Dashboard visualization panel.
1. At the bottom of the **Edit Visualization** drawer, select **Interactions**.
1. From the **Action** drop-down, select **Run a new search**.
1. Enter your search query. (See the examples below.)
1. At the top of the Dashboard, select **Save**.

Your search query can reference the following [tokens](#tokens):

- [`$earliest$`](#earliest)
- [`$environment.baseUrl$`](#environment-baseurl)
- [`$environment.type$`](#environment-type)
- [`$environment.workspace$`](#environment-workspace)
- [`$field.name$`](#field-name)
- [`$field.value$`](#field-value)
- [`$latest$`](#latest)
- [`$rowData.<fieldname>$`](#rowdata-fieldname)
- [`$value$`](#value)

{{% tabs "example1" "example1" "Example 1" "example2" "Example 2" %}}

{{% tab-item "example1" %}}

If you configure a **Run a new search** Interaction with this query using the $value$ token...

```kusto
dataset="users" email=$value$ | project id, email, last_login
```

...then clicking on `sam@example.com` triggers this query...

```kusto
dataset="users" email="sam@example.com" | project id, email, last_login
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22users%22%20email%3D%22sam%40example.com%22%20%7C%20project%20id%2C%20email%2C%20last_login&et=-1h&lt=now&run=true
```

{{% /tab-item %}}

{{% tab-item "example2" %}}

If you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample"
 | where host==$value$ and $field.name$=="$field.value$" and status==$rowData.status$
```

...then clicking on a data point where $value$ is `server1`, $field.name$ is `email`, $field.value$ is `sam@example.com`,
and $rowData.status$ is `200` triggers this query...

```kusto
dataset="cribl_search_sample"
 | where host==server1 and email=="sam@example.com" and status==200
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20where%20host%3D%22server1%22%20and%20email%3D%22sam%40example.com%22%20and%20status%3D200&et=-1h&lt=now&run=true
```

{{% /tab-item %}}

{{% /tabs %}}


> For an Interaction to pass time parameters, it must either link to a [**Time Range**](editing-dashboards#time-range) Input directly, or have no time picker at all.
> If an Interaction links to a parent search that has a time picker, it will fail.
{.box .info}

## Open a URL From a Visualization {#external-link}

You can set up a visualization so that clicking on it opens an external URL.

For this, add an **Open External Link** Interaction:

1. Start [adding](editing-dashboards#add) or [editing](editing-dashboards#edit) a visualization on your Dashboard.
1. At the bottom of the **Edit Visualization** drawer, select **Interactions**.
1. From the **Action** drop-down, select **Open external link**.
1. Enter a URL. (See the example below.)
1. At the top of the Dashboard, select **Save**.

The URL can reference the following [tokens](#tokens):

- [`$earliest$`](#earliest)
- [`$environment.baseUrl$`](#environment-baseurl)
- [`$environment.type$`](#environment-type)
- [`$environment.workspace$`](#environment-workspace)
- [`$field.name$`](#field-name)
- [`$field.value$`](#field-value)
- [`$latest$`](#latest)
- [`$rowData.<fieldname>$`](#rowdata-fieldname)
- [`$value$`](#value)

#### Example

If you configure an **Open external link** Interaction with this URL...

```text
https://www.virustotal.com/gui/domain/$value$
```

...then clicking on a data point with the value of `server1` opens this URL:

```text
https://www.virustotal.com/gui/domain/server1
```

### Include Your Organization and Workspace in a URL {#url}

To construct a URL that includes your Cribl.Cloud Organization and Workspace, use the
[`$environment.baseUrl$`](#environment-baseurl) token.

For example, if you configure an **Open external link** Interaction with this URL...

```text
$environment.baseUrl$
```

...then clicking on the visualization in a Workspace named `main`, in an Organization called `amazing-heady-brcc3nr`, opens
this URL:

```text
https://main-amazing-heady-brcc3nr.cribl.cloud
```

## Open Another Dashboard From a Visualization {#open-dashboard}

You can set up a visualization so that clicking on it opens another Dashboard.

You can also pass the value clicked to one of the target Dashboard's [Inputs](dashboard-inputs), effectively filtering
the new Dashboard by that value.

1. Start [adding](editing-dashboards#add) or [editing](editing-dashboards#edit) a visualization on your Dashboard.
1. At the bottom of the **Edit Visualization** drawer, select **Interactions**.
1. From the **Action** drop-down, select **Add value to dashboard input**.
1. From the **Dashboard** drop-down, select the target Dashboard.
1. From the **Input** drop-down, select one of the target Dashboard's Inputs.
1. At the top of the Dashboard, select **Save**.


![Interaction with an Input](search-interactions-input.png)
{scale="60%"}

## Dashboard Interaction Tokens Reference {#tokens}

Tokens are placeholders that get replaced with actual values when an Interaction is triggered.

A token can be a **static** value, such as a specific string, or **dynamic**, such as the value of a field that was
clicked.

You can use the following tokens in your Interactions:

| Token                                                | Type    | Description                                                                               |
| ---------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------- |
| [`$earliest$`](#earliest) <div style="width: 100px"> | Static  | The start timestamp of a time range to use.                                               |
| [`$environment.baseUrl$`](#environment-baseurl)      | Static  | The base URL of the user's Cribl.Cloud Organization.                                      |
| [`$environment.type$`](#environment-type)            | Static  | The type of the user's Cribl.Cloud Workspace (`development`, `staging`, or `production`). |
| [`$environment.workspace$`](#environment-workspace)  | Static  | The name of the user's Cribl.Cloud Workspace.                                             |
| [`$field.name$`](#field-name)                        | Dynamic | The name of the field clicked.                                                            |
| [`$field.value$`](#field-value)                      | Dynamic | The value of the field clicked.                                                           |
| [`$latest$`](#latest)                                | Static  | The end timestamp of a time range to use.                                                 |
| [`$rowData.<fieldname>$`](#rowdata-fieldname)        | Dynamic | The value of the specified field in the clicked row.                                      |
| [`$value$`](#value)                                  | Dynamic | The value of the data point clicked.                                                      |

### `$earliest$` {#earliest}

The start timestamp of a time range to use with the [`timestats`](timestats) operator.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Static     | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample" | timestats count by host | where _time >= $earliest$
```

...and the current time range is `earliest=1729290112 latest=1729376512`, clicking the visualization triggers this
query...

```kusto
dataset="cribl_search_sample" | timestats count by host | where _time >= 1729290112
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20timestats%20count%20by%20host%20%7C%20where%20_time%20%3E%3D%201729290112&et=-1h&lt=now&run=true
```

### `$environment.baseUrl$` {#environment-baseurl}

The fully qualified base URL of the Cribl.Cloud Organization in which the user triggered the Interaction.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Static     | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

The base URL includes:

- `https://` prefix
- Workspace name ([`$environment.workspace$`](#environment-workspace))
- Organization ID (e.g., `amazing-heady-brcc3nr`)
- Environment type ([`$environment.type$`](#environment-type))
- `cribl.cloud` suffix

For example, if you configure an **Open external link** Interaction with this URL...

```text
$environment.baseUrl$/status
```

...for an Organization with the base URL `https://main-amazing-heady-brcc3nr.cribl.cloud`, the Interaction opens this
URL:

```text
https://main-amazing-heady-brcc3nr.cribl.cloud/status
```

### `$environment.type$` {#environment-type}

The type of the Cribl.Cloud Workspace in which the user triggered the Interaction. Possible values are: `development`,
`staging`, or `production`.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Static     | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure an **Open external link** Interaction with this URL...

```text
status.$environment.type$.example.com
```

...for a Workspace of type `production`, the Interaction opens this URL:

```text
status.production.example.com
```

### `$environment.workspace$` {#environment-workspace}

The name of the Cribl.Cloud Workspace in which the user triggered the Interaction.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Static     | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Open external link** Interaction with this URL...

```text
dashboards.example.com/$environment.workspace$
```

...for a Workspace named `marketing`, the Interaction opens this URL:

```text
dashboards.example.com/marketing
```

### `$field.name$` {#field-name}

The name of the field clicked.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Dynamic    | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample" | where $field.name$="foo"
```

...then clicking on a field named `host` triggers this query...

```kusto
dataset="cribl_search_sample" | where host="foo"
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20where%20host%3D%22foo%22&et=-1h&lt=now&run=true
```

### `$field.value$` {#field-value}

The value of the field clicked.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Dynamic    | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample" | where status=$field.value$
```

...then clicking on a field with the value `200` triggers this query...

```kusto
dataset="cribl_search_sample" | where status="200"
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20where%20status%3D%22200%22&et=-1h&lt=now&run=true
```

### `$latest$` {#latest}

The end timestamp of a time range to use with the [`timestats`](timestats) operator.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Static     | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample" | timestats count by host | where _time <= $latest$
```

...and the current time range is `earliest=1729290112 latest=1729376512`, clicking the visualization triggers this
query...

```kusto
dataset="cribl_search_sample" | timestats count by host | where _time <= 1729376512
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20timestats%20count%20by%20host%20%7C%20where%20_time%20%3C%3D%201729376512&et=-1h&lt=now&run=true
```

### `$rowData.<fieldname>$` {#rowdata-fieldname}

The value of a specific field in the clicked row (regardless of which row was actually clicked). Replace `<fieldname>`
with a real field name.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Dynamic    | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

#### Example


![A visualization panel of the **Events** type](search-interaction-rowdata.png)

If you configure the visualization above with a **Run a new search** Interaction like this...

```kusto
dataset="cribl_search_sample" | where method=$rowData.method$ & count=$rowData.count_$
```

...then no matter whether the user selects `count\_: 66` or `method: GET`, the Interaction runs a query with both...

```kusto
dataset="cribl_search_sample" | where method=GET & count=66
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20where%20method%3DGET%20%26%20count%3D66&et=-1h&lt=now&run=true
```

### `$value$` {#value}

The value of the data point clicked.

| Token Type | Interactions Types Supported                                               |
| ---------- | -------------------------------------------------------------------------- |
| Dynamic    | [Open an external link](#external-link)<br>[Run a new search](#new-search) |

For example, if you configure a **Run a new search** Interaction with this query...

```kusto
dataset="cribl_search_sample" | where host=$value$
```

...then clicking on a data point with the value of `server1` triggers this query...

```kusto
dataset="cribl_search_sample" | where host="server1"
```

...encoded in this URL:

```text
https://<yourWorkspace>-<yourCloudInstance>.cribl.cloud/search/new?q=dataset%3D%22cribl_search_sample%22%20%7C%20where%20host%3D%22server1%22&et=-1h&lt=now&run=true
```
