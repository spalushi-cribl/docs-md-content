# Cribl Search 4.10.0


Cribl Search 4.10 applies improved type coercion, adds numerous new functions for manipulating objects and numbers,
enables you to share searches and search state via URL, and more.

## Important Changes

These changes to Cribl Search might require you to check or modify existing queries, or other configuration, especially
on saved searches.

### Deprecation Notice: Dataset Acceleration

Cribl is deprecating [Dataset Acceleration](acceleration) (a Preview feature) in preparation for a different solution.
This feature will be removed in a future release. Please continue to report issues through normal Cribl
[support channels](/support/support-contact), but assistance for this deprecated feature might be limited.

### Deprecation Notice: Pack Export Modes

Cribl has deprecated the **Merge safe** and **Default only** options previously selectable when [exporting](packs#export) a Cribl Search Pack. (Selecting these options triggered conflict errors upon some valid exports.) All Search Pack exports now automatically use the option previously labeled **Merge**. This change does not affect Stream/Edge Packs. 

### Deprecation Notice: AI Field Summaries Prompt

Cribl Search no longer prompts you to **Generate Field Summaries** when you use Cribl Copilot to generate a KQL query from natural language. (Cribl Search now automatically maintains field summaries in the background, and updates them as your Datasets evolve.)

### Concatenate Strings with `strcat` Instead of `+`

To concatenate strings, Cribl Search now enforces using the [`strcat`](strcat) function, to build expressions of the
form: `strcat("string1", "string2")`. Check and update any existing expressions in which other functions or operators
relied on undocumented `"string1" + "string2"` notation, because these concatenations will now return [`null`](null).

### Improved Type Coercion

Cribl Search now applies stricter type matching to filter terms. This will make filtering more predictable, but it might
change the results returned by existing queries that were built around looser type evaluation. In existing searches –
especially [scheduled searches](scheduled-searches) – check all comparison expressions, and verify that sampled searches
still return expected results.

The most important changes to type matching rules are:

- [More Precise Definition of `bool`](#bool)
- [More Consistent Treatment of `null`](#null)
- [More Consistent Number Matching (including numeric strings)](#number-matching)
- [More Consistent Behavior of Type Conversion Functions](#type)
- [New Type and Function Aliases](#alias)

#### More Precise Definition of `bool` {#bool}

Cribl Search now uses a stricter, more straightforward definition of the [`bool`](bool) type, ensuring more predictable
behavior when working with [`bool`](bool) values. For the latest documentation and examples, see [bool](bool).

In a related fix, [`bool`](bool) comparisons now work correctly with the implicit [`cribl`](cribl) operator, so you can
create expressions of this form:

```kusto {runSearch=true}
dataset="cribl_logs" job._definition.disableNotifications = true | limit 100
```

Previously, in comparisons, the [`cribl`](cribl) operator treated [`bool`](bool) values as [`string`](string) values. So
Cribl Search would have implicitly evaluated the above expression as:

```kusto
dataset="cribl_logs" job._definition.disableNotifications = "true" | limit 100
```

#### More Consistent Treatment of `null` {#null}

In certain edge cases, Cribl Search now treats the [`null`](null) value more consistently. For the latest documentation
and more examples, see [null](null).

| Example          | Result Before | Result Now |
| ---------------- | ------------- | ---------- |
| `10 - null`      | `10`          | `null`     |
| `10 * null`      | `0`           | `null`     |
| `5 > null`       | `true`        | `null`     |
| `tostring(null)` | `"null"`      | `null`     |
| `toint(null)`    | `0`           | `null`     |
| `tobool(null)`   | `false`       | `null`     |
| `not(null)`      | `true`        | `null`     |

#### More Consistent Number Matching {#number-matching}

In certain cases, number matching (including numeric strings) works differently now, making the overall experience more
consistent.

| Example        | Result Before | Result Now |
| -------------- | ------------- | ---------- |
| `"1" == "1.0"` | `false`       | `true`     |
| `"30" < "100"` | `false`       | `true`     |

#### More Consistent Behavior of Type Conversion Functions {#type}

In certain edge cases, type conversion functions ([`tobool`](tobool), [`todouble`](todouble), [`toint`](toint),
[`tolong`](tolong), [`toreal`](toreal), and [`tostring`](tostring)) now work more consistently.

| Example       | Result Before | Result Now |
| ------------- | ------------- | ---------- |
| `toint(null)` | `0`           | `null`     |
| `toint(“”)`   | `0`           | `null`     |

#### New Type and Function Aliases {#alias}

Some of the changes mentioned above have turned a few types and functions into aliases. Many were already functioning as
aliases but weren’t documented as such, while others had their behavior slightly adjusted in specific edge cases.

Here’s the list of functions affected:

| Function               | Aliases                                                                            |
| ---------------------- | ---------------------------------------------------------------------------------- |
| [`tobool`](tobool)     | [`bool`](bool)                                                                     |
| [`todouble`](todouble) | [`decimal`](decimal)<br>[`double`](double)<br>[`real`](real)<br>[`toreal`](toreal) |
| [`toint`](toint)       | [`int`](int)<br>[`long`](long)<br>[`tolong`](tolong)                               |

And for the up-to-date list of type aliases, see [Types](types).

## New Features

This release includes the following new features.

### New Dynamic (Bag) Functions

The new [dynamic scalar functions](dynamic-functions) (also known as “bag functions”) allow you to manipulate objects by
operating on [`dynamic`](dynamic) values, including dynamic arrays and property bags:

- [`bag_has_key`](bag-has-key)
- [`bag_keys`](bag-keys)
- [`bag_merge`](bag-merge)
- [`bag_pack`](bag-pack)
- [`bag_pack_columns`](bag-pack-columns)
- [`bag_remove_keys`](bag-remove-keys)
- [`bag_set_key`](bag-set-key)
- [`bag_zip`](bag-zip)
- [`make_bag`](make-bag)
- [`make_bag_if`](make-bag-if)
- [`zip`](zip)

### New Operator: `mv-pull`

The new [`mv-pull`](mv-pull) operator pulls key-value pairs from array objects into a top-level event, or into a
dedicated object/bag.

### New Function: `ceiling`

The new [`ceiling`](ceiling) function rounds up a numeric expression’s value to the nearest matching integer. You can
use [`ceil`](ceil) as an alias for this function.

### Packs Expansion (Preview)

In Cribl Search [Packs](packs) (a Preview feature), you can now share saved searches, [Datatypes](datatypes), and
Knowledge objects ([Macros](macros) and [Lookups](lookups)), in addition to the [Dashboards](dashboards) supported in
previous releases. We've also added new UI options to move Dashboards and Datatypes between Packs, and between Packs and
your Search Organization's global context.

### Share Searches and State Details via URL

When Cribl Search executes a query, the resulting URL now captures the query string, time-range selection, sample-ratio
selection, and any chart and table settings that were applied in aggregated results. You can copy and share this URL.
With appropriate Permissions, pasting the URL will replicate the search with all these state details.

### Manage Saved Searches as JSON

You can now edit your saved searches in a JSON editor built into Cribl Search, and import and
export JSON files that encapsulate complete configurations.

### Generate Visualizations from Datasets

On the **Search Home** page, hovering over a Dataset now displays a **Visualize** button. Select this button to prompt Cribl Copilot to directly [suggest visualizations](copilot-dashboards) that might be useful in displaying this data. You can then select, modify, or replace these suggestions, and add your chosen visualization to a Dashboard.

## UI/UX Improvements

This release includes the following improvements to the Cribl Search UI/UX.

### Expanded Sidebar

We added a few items to the Cribl Search sidebar:

- **History** (previously in **Search Home** > **History**)
- **Saved Searches** (previously in **Search Home** > **Saved**)
- **Knowledge** (where you’ll now find [Lookups](lookups), [Parsers](parsers), [Regexes](regexes),
  [Grok patterns](grok-patterns), and [Macros](macros)).

Also, **Data** now includes [Datatypes](datatypes) (previously in **Settings** > **Search** > **Datatypes**).

| Before                                                | After                                               |
| ----------------------------------------------------- | --------------------------------------------------- |
| ![Sidebar before 4.10](search_sidebar_410_before.png) | ![Sidebar after 4.10](search_sidebar_410_after.png) |

### Wrap Cells in the Event Viewer

The [**Events**](search-overview#events) tab now allows you to [wrap cells](search-overview#toggle), to prevent values
from overflowing.

### Customize Your Results Tables

When [viewing events in a table](search-overview#toggle), you can now
[select which fields you want to include](search-overview#events-shortcuts), making the results much easier to scan.

Also, tables displaying below Dashboard visualizations now support column sorting.

### Single-Value Chart Formatting

[Single-value charts](chart-single) now display their single value with a comma (`,`) as the thousands separator. This
is optional but enabled by default.

### New Login Failure Message

To reflect authentication scenarios beyond username/password authentication, the displayed login failure message in the
UI now reads “Authentication unsuccessful. Please try again.”

## Corrections

This release includes numerous fixes to various areas of Cribl Search, most notably:

| Reference                                                                              | Description                                                                                                                                                                                                                                                                                                                                  |
| -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <div style="width: 100px">SEARCH-8150<br>[Known issue](known-issues#SEARCH-8150)</div> | Single-value interactions now respect the time-range picker instead of defaulting to the last hour.                                                                                                                                                                                                                                          |
| SEARCH-7773<br>[Known issue](known-issues#SEARCH-7773)                                 | Pack-imported Dashboards can now be shared from the global Dashboards page without triggering an error.                                                                                                                                                                                                                                      |
| SEARCH-8283<br>SEARCH-8282                                                             | The [`coalesce`](coalesce) function now correctly returns `false` or `0` (respectively) when evaluating expressions like `coalesce(false, true)` or `coalesce(0, 1)`. Previously, `coalesce` skipped expressions with `false` or `0` first argument. This correction might affect saved searches that relied on the incorrect skip behavior. |
| SEARCH-7966                                                                            | We've clarified the behavior of comparison and string operators when used with (respectively) the [`cribl`](cribl) and [`where`](where) operators. At the sections linked here, you'll find updated details about case-sensitivity, and about wildcard and regex support, along with added examples.                                         |
| SEARCH-7785                                                                            | Charts using the [`project`](project) operator now display the X axis correctly.                                                                                                                                                                                                                                                             |
| SEARCH-6973                                                                            | Double semicolons `;;` are now working as expected.                                                                                                                                                                                                                                                                                          |
| SEARCH-8543                                                                            | When you select **Search the Results** to [query the results of a previous search](search-results#search-the-results), the time range is now set to **All time** by default.                                                                                                                                                                |
| SEARCH-8186                                                                            | The [`match_regex`](match_regex) operator no longer throws an error when the second argument is an empty string. For example, this works correctly now: `match_regex('hello', '')`.                                                                                                                                                          |
| SEARCH-8121                                                                            | The [`unixtime_milliseconds_todatetime`](unixtime-milliseconds-todatetime) function works correctly now, no longer replicating [`unixtime_seconds_todatetime`](unixtime-seconds-todatetime).                                                                                                                                                 |
| SEARCH-8036                                                                            | The [time range](cribl#time-syntax) setting now requires the earliest value to always be earlier than latest.                                                                                                                                                                                                                                |
| SEARCH-7212                                                                            | History now correctly displays queries that begin with a comment line.                                                                                                                                                                                                                                                                       |
| SEARCH-8531                                                                            | The [`indexof`](indexof) function now returns `null` for invalid arguments, preventing infinite loops.                                                                                                                                                                                                                                       |
| SEARCH-3571                                                                            | Members with the [”User” Search Member Permissions](cloud-managing#search-member-roles) can no longer see the **New Dataset Provider** wizard, whose use is restricted to Admins and Editors.                                                                                                                                                |
