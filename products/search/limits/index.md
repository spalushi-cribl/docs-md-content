# Limits


Configure limits that apply to all searches performed in your Cribl Search instance. 

---

You can configure limits that control search performance, search history, and the granularity of search summaries displayed in the UI.

Limits listed in this section apply to all searches performed in your Organization. (You can define specific limits per user by configuring [Usage Groups](usage-groups).)

Certain limits can be overridden by [`set` statements](set) in your queries.

## List of Configurable System Limits {#list}

The following limits are available at **Settings** > **Search** > **Limits**:

- **Length of the search job queue**: Sets the number of search queries that can be queued. If the maximum number of
  queries has already been reached, new queries will be added to a queue and executed later. The limit can be expressed
  either as a percentage of the maximum number of concurrent queries or as an absolute number, allowing you to control
  the length of the queue.
  - Default is `500%`.
  - Minimum of `0`.
- **Warm pool size**: Maximum number of warm coordinator search processes to run on the Leader or Worker Node. `auto`
  sets 1 process on the Leader and 0 on Worker Nodes.
  - Default is `auto`.
  - Minimum of `0`.
- **Search history TTL**: Amount of time to keep search artifacts around before removing them.
  - Default is `7d` (7 days).
  - Minimum of `0`, which instructs Cribl Search to not retain search artifacts at all.
  - You can override this limit for a specific scheduled search, using the
    [**Keep last executions**](scheduled-searches#advanced) setting.
  - [Notebooks](notebooks) have a hard-coded 30-day retention period to facilitate extended investigations. Exceeding the **Search history job limit** will cause other jobs to be removed before Notebook jobs, to respect this extension.
  {#jobs}
- **Search history job limit**: Maximum number of search jobs to retain before removing the oldest ones.
  - Default is `1000`.
  - Minimum of `0`.
  - You can override this limit for a specific scheduled search, using the
    [**Keep last executions**](scheduled-searches#advanced) setting.
- **Field summary nested depth limit**: Maximum depth supported when building [field summaries](search-overview#browser) on nested object values. For example, `x.y.z` corresponds to the default depth of `3`. Increase this default if you need to go deeper. For complex datasets with deeply nested structures, this limit prevents excessive resource consumption during field discovery.
  - Default is `3`.
  - Minimum of `1`.
- **Field summary field limit**: Maximum number of distinct fields to include (before truncation) when generating [field summaries](search-overview#browser) on objects. Reduce this limit to improve performance with complex datasets.
  - Default is `200`.
  - Minimum of `0`.

- **Compress cache**: When set to **Yes**, object cache artifacts get compressed.
  - Default is **No**.
- **Prevent revealing Dataset Provider secrets** (Admin only): When enabled, credentials used for configuring Dataset Providers (such as secret keys) are obfuscated.
  - Default is **Yes**.

## List of Fixed System Limits {#fixed}

The following limits are not configurable:

- **Field size limit**: Maximum per-field payload size enforced on fields (like `_raw`). When results are rendered, Cribl Search will truncate or omit oversized payloads to maintain responsiveness. This limit applies to all searches.
  - Set to `2` MB per field, and  not configurable.
