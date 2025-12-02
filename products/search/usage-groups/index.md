# Usage Groups


Control access to resources for specific Search Members, by creating Usage Groups.

---

Usage Groups enable you to assign limits to searches by specific users. By setting limits, you can prevent inefficient queries either by disallowing them, or by terminating their execution if they exceed a limit.


> To set up general limits that apply to all searches in your Organization, see the [limits](limits) topic.
>
{.box .info}

Cribl Search provides two Usage Groups by default:

- **system** – Provides system-level limits that apply to all searches.
- **default** – Covers all ad hoc searches, unless covered by a different group.

## Creating Usage Groups

To create a Usage Group, select **Settings** > **Search** > **Usage Groups**, then select **Add Usage Group**. This opens a **New Usage Group** modal.

On the modal's **Configuration** tab, give the group a name, and set up desired [limits](#usage-limits) that members of this group will be
bound by. Activate the group by making sure the **Enabled** toggle is selected before you save the configuration.

To assign users to a group, on the **Members** tab, select **Add Members** . All users who have the **User** or **Editor** [Search Member Permissions](cloud-managing#search-member-roles) are eligible to be group Members.

## Usage Limits

You can configure the following usage limits for each group:

- **Earliest relative time range** – How far back in time the search can go. For example: `60m`, `30d`, `6mon`, `1y`. To
  specify time in seconds, enter a number with no time unit. This limit can be overridden by
  [`max_earliest_relative`](set#max_earliest_relative).
- **Time range limit** – Maximum time range for one search query, for example 3 days (`3d`). This limit can be
  overridden by [`max_timerange_width`](set#max_timerange_width).

  > When managing each of the following three **concurrent** limits: Indicate a percentage (of CPU capacity on the Leader) by appending `%`. Reduce the default `100%` if you notice an impact on UI responsiveness, or on workloads like Stream Collector jobs. A Leader with only 1 CPU will treat all values as `100%`. You also have the option to allocate an absolute number of CPUs on the Leader, by omitting the `%`.
  {.box .info}

- **Concurrent ad hoc search limit** – Maximum volume of ad hoc searches that all Members of this Group can run simultaneously. Queries exceeding this limit will get queued. This limit can be overridden by
  [`max_searches_per_user`](set#max_searches_per_user), and by the **Overall concurrent search limit** value in this modal.
- **Concurrent scheduled search limit** – Maximum volume of scheduled searches that all Members of this Group can run simultaneously. Queries exceeding this limit will get queued. This limit can be overridden by [`max_scheduled_searches_per_user`](set#max_scheduled_searches_per_user), and by the **Overall concurrent search limit** value in this modal.
- **Overall concurrent search limit** – Maximum number of simultaneous searches allowed across your Organization.
  - Percentage values for all **concurrrent** limts are calculated as:  
  `Math.min(1, numCPU x maxConcurrentSearches/100)`.
  - Cribl Search will be entirely disabled if this number is set to `0`.
- **Results limit** – Maximum number of events that can be returned per search.
  - Default is `50000`.
  - Minimum of `1`.
  - Maximum of `10000000`.
  - This limit can be overridden by [`max_results_per_search`](set#max_results_per_search).
- **Executors limit** – Maximum number of executors dispatched per search.
  - Default is `50`.
  - Minimum of `1`.
  - This limit can be overridden by [`max_executors`](set#max_executors).
- **Byte limit** – Maximum number of bytes that can be read per search. When the limit is exceeded, the search gets
  canceled. This limit can be overridden by [`max_bytes_read_per_search`](set#max_bytes_read_per_search).
- **Running time limit** – Maximum time, in seconds, that a single search is allowed to run.
  - Default is `28800` seconds (8 hours). 
  - This limit can be overridden by
    [`max_running_time_per_search`](set#max_running_time_per_search).
- **Coordinator heap memory limit** – Memory allocation (in MB) for the search coordinator process.
  - Default is literally labeled `default`, which delegates to the Node.js default. 
  - Set a slightly higher value if you experience out-of-memory errors due to high Dataset cardinality or other conditions.
  - Be aware that very high settings might compromise other applications.

## Overriding Limits

Certain usage limits can be overridden by [`set`-statement options](set#available-set-statement-options), for example:
`set max_searches_per_user=100000`.

Admins can also run a query as a specific Usage Group with: `set usage_group="secops"`.

The following [`set`](set) options correspond to the limits configured in the UI described above:

| Limit                                      | Setting                                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------ |
| **Earliest relative time range**           | [`max_earliest_relative`](set#max_earliest_relative)                     |
| **Time range limit**                       | [`max_timerange_width`](set#max_timerange_width)                         |
| **User concurrent ad hoc search limit**    | [`max_searches_per_user`](set#max_searches_per_user)                     |
| **User concurrent scheduled search limit** | [`max_scheduled_searches_per_user`](set#max_scheduled_searches_per_user) |
| **Results limit**                          | [`max_results_per_search`](set#max_results_per_search)                   |
| **Executors limit**                        | [`max_executors`](set#max_executors)                                     |
| **Byte limit**                             | [`max_bytes_read_per_search`](set#max_bytes_read_per_search)             |
| **Running time limit**                     | [`max_running_time_per_search`](set#max_running_time_per_search)         |
