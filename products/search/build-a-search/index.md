# Build a Search


Learn about query syntax, operators, time ranges, sampling, and more.

---

A search needs a query and a time range. Queries specify the data to search and run functions and operators from our
ergonomic and powerful search language.

Queries contain zero or more operators separated by a pipe `|`. Data flows, or is piped, from one operator to the next.
Events are filtered or manipulated at each operation and then fed into the following operation.

You can think of the data flow as a funnel, where you start out with selected events. Each time the data passes through
another operator, it is filtered, rearranged, or summarized. Because the piping of information from one operator to
another is sequential, the query operator order is important and can affect both results and performance. At the end of
the funnel, you're left with a refined output.

The following diagram shows how each operation in a query changes search results.


![Data flow diagram](data-funnel-flow.d769ea1bec.svg)
{scale="100%" border="false"}

## Query Syntax {#syntax}

`[Statements;] Scope [| Processing [| ... ]]`

- **Statements** - Optional statements that can [set query options](set) and [assign names](let) to expressions (which
  allows you to use [`join`](join) or [`union`](union) later in the query). Each statement needs to end with a semicolon
  (`;`).
- **Scope** - Define [Datasets](basic-concepts#datasets) and use the [`cribl`](cribl) operator to specify the events to search.
  Wildcards `*` represent one or more characters. For example, `source=/*/foo.log "error"` returns lines from
  `/*/foo.log` containing the term `error`.
  - Dataset syntax: `dataset=DatasetID`. E.g, `dataset=myCDNLogs`.
    - Data from different Datasets can be easily combined in a search. For example,
      `dataset=myVPCFlowlogs or dataset=myCDNLogs`.
  - The `default` Dataset is searched when no Dataset is defined.
  - The [`cribl`](cribl) operator is implicit when an operator isn't specified. You can stop this behavior by enabling
    **Strict Kusto Mode** in the [UI Options](#search-options).
  - The [`cribl`](cribl), [`externaldata`](externaldata), and [`find`](find) operators are used in the scope.
  - The [`search`](search) and [`timestats`](timestats) operators may be used in the scope.
- **Processing** - Optional [operators](operators) that can be used together with [context](context-functions),
  [cribl](cribl-functions), [statistical](statistical-functions), and [scalar](scalar-functions) functions. Use a pipe
  `|` between operators.

Every stage of a query can also include [Macros](macros) (for example, `${MacroID}`).

### Examples

- Find events with `error` in the `cribl_internal_logs` Dataset:
  ```kusto {runSearch=true}
  dataset="cribl_internal_logs" "error"
  ```
- Find and return up to 1,000 logs:
  ```kusto {runSearch=true}
  dataset="cribl_search_sample"
  | limit 1000
  ```
- Aggregate events by the `method` field:
  ```kusto {runSearch=true}
  dataset="cribl_internal_logs"
  | limit 1000
  | summarize count() by method
  ```
- Count the events in each minute, grouping by the `method` field, from the last 15 minutes:
  ```kusto {runSearch=true}
  dataset="cribl_internal_logs" earliest=-15m method=*
  | limit 1000
  | timestats span=1m count() by method
  ```
- Merge the events of two different Datasets:

  ```kusto
  let query1 = dataset="dataset1" | where x < 10;

  dataset="dataset2"
  | join query1 on commonField
  ```

## Syntax Highlighting {#highlighting}

Within the query box, the following color coding distinguishes components of your query:

Component           | Light Mode | Dark Mode
------------------- | ---------- | ---------
Literal             | <span style="color:#722ED1">Magenta</span> | <span style="color:#B37FEB">Magenta</span>
Field name          | <span style="color:#780650">Pink</span>    | <span style="color:#FFADD2">Pink</span>
Search term         | <span style="color:#AD4E00">Orange</span>  | <span style="color:#FFD591">Orange</span>
Operator            | <span style="color:#2F54EB">Blue</span>    | <span style="color:#85A5FF">Blue</span>
Function            | <span style="color:#006D75">Teal</span>    | <span style="color:#5CDBD3">Teal</span>
Connecting keyword  | <span style="color:#876800">Amber</span>   | <span style="color:#D4B106">Amber</span>
Error               | <span style="color:#CF1322">Red</span>     | <span style="color:#FF4D4F">Red</span>
Comment             | <span style="color:#237804">Green</span>   | <span style="color:#73D13D">Green</span>

(Dark mode uses lighter shades to reduce contrast.)

## Field Names {#field-names}

Each event field has a unique name that is used as its reference in a query. Each field contains a key-value pair, where
the field name is the key. Field names follow these rules:

- They're case-sensitive.
- Support letters, digits, underscores `_`, spaces, dots `.`, and dashes `-`.
- Fields containing a special character (spaces, dots, or dashes) require quoting (see below).

### Quoting Keywords

Field names that are identical to some query language keywords, or have special characters, require quoting when they're
referenced directly by a query:

| Query text       | Description                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------- |
| `field`          | Field names that don't include special characters or map to some language keyword don't require quoting. |
| `['field‑name']` | Field names that include special characters must be quoted using `['` and `']` or using `["` and `"]`.   |
| `["where"]`      | Field names that are language keywords must be quoted using `['` and `']` or using `["` and `"]`.        |


> You can normalize field names that contain separators, using the [`foldkeys`](foldkeys) operator.
{.box .info}

For example, events from the `cribl_internal_logs` Dataset have a `time` field which is a reserved keyword requiring
quoting. To reference the field in a query you'd specify it as:

```kusto
['time']="2023-03-31T12:24:10.770Z"
```

Similarly, to exclude `null` fields from the search results, you must bracket and quote the `null` keyword. To see how this works, let's start with the following query. Here, the `method` field has a value of null. We'll want to remove that field:

```kusto {runSearch=true}
// Count of method over time with a null field value.
dataset="cribl_internal_logs" earliest=-15m 
 | limit 1000
 | timestats count() by method
```

This first attempt will **not** work, because `null` is unquoted:

```kusto {runSearch=true}
// This won't work because null is a keyword and must be quoted within brackets.
dataset="cribl_internal_logs" earliest=-15m 
 | limit 1000
 | timestats count() by method
 | project-away null
```

Here is the same query, with `null` properly bracketed and quoted. If you run it, watch the `null` field from the original query go away:

```kusto {runSearch=true}
// This will work because null is properly quoted.
dataset="cribl_internal_logs" earliest=-15m 
 | limit 1000
 | timestats count() by method
 | project-away ["null"]
```

### Generated Fields

Cribl Search generates fields from expressions and returns them in your search results. The expression is evaluated and
all non-word characters are replaced with underscores `_`. For example, `count(myField)` returns as `count_myField`.

If there's only one field, a single underscore is appended to it. For example, `count` returns as `count_`.

### Maximum Field Length

Generated fields can be a maximum of 20 characters long. Fields are truncated from the left of the expression after the
function name to keep within the 20-character limit. For example, `countif(channel, elapsedMS > 0)` returns as
`countif_elapsedms_0`.

If the left field does not start with an alphabetic character, the farthest right field is truncated.

### Built-in Fields {#built-in}

Cribl Search uses several fields to assist in the handling of data. These "meta" fields are not part of an event, but
they are accessible, and can be referenced in queries.

| Field         | Description                                                                                  |
| ------------- | -------------------------------------------------------------------------------------------- |
| `_raw`        | The raw event. Where an event contains no `_raw` field, Cribl Search constructs one by serializing all the event's top-level and nested fields. |
| `_time`       | The raw event's timestamp. If Cribl Search detects no timestamp, it uses the last modified timestamp. |
| `cribl_fleet` | The name of the Edge Fleet that contained the event.                                         |
| `dataset`     | The Dataset that was queried to retrieve the event.                                          |
| `datatype`    | The [Datatype](datatypes) processed against the event.                                       |
| `host`        | The hostname of the device that contained the event.                                         |
| `source`      | The name of the data object that contained the event.                                        |

If an event has a field with the same name as a Cribl Search built-in field (except `_time`), the event's field is
prepended with `data_`. For example, `data_source`.

## Autocomplete

As you type in the query box, a drop-down modal with suggestions is displayed to make query writing easier. Use the down
and up keyboard keys to highlight a suggestion. Press Enter/Return or Tab to add the highlighted suggestion to your
query, or select a suggestion.

Autocomplete adapts its suggestions as you type, referencing built-in fields, Dataset IDs, and the query language.


![Autocomplete](search-autocomplete.png)
{scale="100%"}

The bottom section of the autocomplete modal provides a list of recent queries. When clicked, the query is added to the
query box.


> You can reuse query text across different searches, by using [Macros](macros).
{.box .info}

### Look Up the Query Language Docs

Operators and functions have a greater-than character `>` displayed on the right of the autocomplete suggestion when
highlighted. When clicked, autocomplete expands to display its [documentation](operators) for reference.


![Viewing the docs inside Cribl Search](search-autocomplete-readMore.png)
{scale="100%"}

## Preview

Preview provides a testing mode where operators can be applied to a previously run query, allowing you to quickly
preview the changes without executing the search on the actual data source. You can preview operators as many times as
you need, without incurring the time or cost of executing them on actual data providers until you're ready.

- Up to 100 results from the initial search are used in Preview.
- The [`centralize`](centralize) and [`send`](send) operators are not supported.

To open Preview, hover over an operator and select **Preview**. For non-aggregate queries, you can first select bars in
the [histogram](search-overview#histogram) to view results for only the selected times. Use shift+select to select
multiple bars.


![Preview button](search-preview-operator.png)
{scale="100%"}

Type the operators you'd like to test against the initial search results and press Enter/Return or select **Preview**.


![Preview example](search-preview-extend-example.png)
{scale="100%"}

Notice the results table switches to **Out** instead of **In**, indicating the displayed results are what the operator
would return.

- New data is highlighted in green.
- Modified data is highlighted in orange.
- Deleted data is highlighted in red.

Select **Apply** to have your test query added to your original query.

## Date and Time Range {#time-range}

You can use the **Date Range** modal to find events within a specified date or time range. This narrows down event
results, adding another layer of granularity to your search.


> Some operators, like [`cribl`](cribl) and [`timestats`](timestats), support
> their own time arguments that can override the time range set in the UI.
>
> [Subqueries](common-examples#time-range) can also use their own time arguments,
> but the main query time range still applies to the final results.
>
{.box .info}

To begin, select the time field at the top right of **Search Home**.


![The date/time field](search-date-time-field.png)
{scale="100%"}

### Copying Date Ranges

To copy and paste predefined date ranges into other Search browser windows, hover over the time field and select the
**Copy** button. Navigate to another browser containing Search, hover over the time field, and select the **Paste** button.

Good deal! You just saved yourself a few minutes of keyboard fatigue.

### Time Range Options

Once you click into the time field, the date and time modal will appear. Here, you can select or enter a relative time,
enter a specific date/time range, or define a duration of time around a specific date.

#### Relative Time {#relative-time}

To find events that occurred between the selected time and now, use Relative time.

Select one of the following options:

- **seconds ago**: Number of seconds in the past.
- **minutes ago**: Number of minutes in the past.
- **hours ago**: Number of hours in the past.
- **days ago**: Number of days in the past.
- **All time**: All events.


> For [scheduled searches](scheduled-searches), `now` is the **scheduled** time,
> not the actual execution time.
>
> For example, if you schedule a search for 1:00, but an unplanned outage causes
> the search to run at 1:05, the value of `now` will be 1:00, not 1:05.
{.box .info}

#### Relative Tab

The **Relative** tab is another way to find events within a specified time range. To find events that occurred between
two times, select the **Relative** tab and enter an **Earliest** (-90 days maximum) and a **Latest** time.

In this example, the defined Relative time range will return events that occurred between 14 days ago and 10 days ago.


![Earliest and Latest relative time range](search-relative-tab.png)
{scale="50%"}

#### Date Range {#date-range}

To define a custom date and time range to search within, use the **Date Range** tab. Select the desired start and end
dates and times from the calendar. Select **Apply** to show search results for the date range.


![Custom date and time range options in the Date Range tab](search-date-range-tab.png)
{scale="70%"}

#### Around {#around}

To select a custom date and specify a duration of time "around" that date, use the **Around** tab.

For example, you can select 07-03-2023 at 10:00 +/- 2 hours, the results of which will show you all events that happened
from 8:00 am to 12:00 pm.

This is useful for finding important events that you know happened around a particular date, but you don't know exactly
when.


![The Around tab](search-date-range-around.png)
{scale="50%"}

#### Time Zone {#time-zone}

To apply a specific time zone to your search, select the time zone under **Current Timezone**. You can select a time
zone, or search by time zone, country, city, or offset (+8 for example).


> ##### Notes
>
>  - Any selection made in the time zone selector is automatically applied to
>    the **Time** column on the **Events** tab.
>  - The **Local** time zone is based on the time zone set of the computer that is
>    running Search.
>
{.box .info}

## Sampling {#sampling}

Sampling uses a ratio to reduce the number of results returned from a search. For example, if a search returns 1,000 events, a 1:10 sampling ratio returns about 100 events.

Select **Sampling** to the left of the time range box to set a sampling ratio.

## Comments

You can add comments to your query with double forward slashes `//` prepending the comment. Any characters on the same
line after `//` are part of a comment. For example:

```kusto
goats | summarize count() // count the goats
```

## Autoformat Query Text

To make a complex query more organized and easier to read, you can autoformat its text.

When in the query box, press the `Ctrl` + `|` (Linux, Windows) or `Command` + `|` (MacOS) key combination.

## UI Options {#search-options}

Select the gear button to the left of the query box to open the options menu.


![Search options](search-query-box-options.png)
{scale="60%"}

- **Auto Height**: Enabled by default. Automatically adjusts the height of the query box based on the size of the query.
  When disabled, hover over the bottom line of the query box, then select and drag the line down or up to adjust its
  size.
- **Submit on ENTER**: Enabled by default. Runs the search when you press the Enter/Return key, so you don't have to
  select **Search**.
- **Strict Kusto Mode**: Disabled by default. Stops the [`cribl`](cribl) operator from being implicit in the **scope**.
  When enabled, a term is interpreted as a Dataset ID.



