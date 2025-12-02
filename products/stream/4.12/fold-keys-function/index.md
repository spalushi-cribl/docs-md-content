# Fold Keys


The Fold Keys Function transforms flat field names that include a common separator (such as `.` or `/`) into a more logical, nested structure. Use this Function to normalize hierarchical data that expresses logical relationships through field names. The Fold Keys Function restructures the field names into grouped, nested objects without changing their values.

Use this Function to:

- **Normalize data from systems that encode hierarchy in field names:** Standardize keys from sources that encode similar data differently. For example, you could transform keys such as `http.status`, `http.method`, and `http.url` into a nested structure under `http`.
- **Reduce field name redundancy for more efficient storage and easier parsing:** Group similar fields under a parent key to reduce clutter and improve readability. With nested structures also give you the option to apply logic to an entire group (`http.*`) rather than individual fields using another Function in your Pipeline.
- **Improve schema compatibility or prepare data for JSON-based Destinations or downstream receivers that expect nested objects:** Structure your data for downstream consumption by tools such as Elasticsearch or certain APM platforms that expect hierarchical data structures for querying or visualizing.

See [Usage Examples](#usage) for an in-depth example of how this Function processes data.

## How the Fold Keys Function Works

The Fold Keys Function is a simpler version of the [Eval Function](eval-function), designed specifically to group related fields by a common separator in their names. If you're familiar with Cribl Search, the Function behaves like the [foldkeys](/search/foldkeys) operator, but for use within Cribl Stream.

The Fold Keys Function processes data in the following order:

1. **Search:** It looks for field names that contain the specified separator string (such as a period or underscore). If a field name does not include the separator, the Function skips it and passes it through unchanged.

1. **Group:** For each matching field name, the Function splits the name into parts based on the separator. It uses these parts to organize related fields under a shared top-level key.

1. **Nest:** The Function creates a nested data structure schema based on the grouped parts. For example, a field like `http.status.code` would become:

   ```
   "http": {
     "status": {
       "code": 404
       }
     }
   ```

See [Usage Examples](#usage) for a practical, real-world example of how this Function processes data.

## Configure the Fold Keys Function

To configure this Function:

1. Add a Source and Destination to begin generating data. See [Integrations](integrations) for more information.

1. Add a new Pipeline or open an existing Pipeline. Add or capture sample data test your Pipeline as you build it. See [Pipelines](pipelines) for more information.

1. At the top of the Pipeline, select **Add Function** and search for `Fold Keys`, then select it.

1. In the **Fold Keys** modal, configure the following general settings:

   - **Filter:** A JavaScript filter expression that selects which data to process through the Function. Defaults to `true`, meaning it evaluates all data events. If you don't want to use the default, you can use a JavaScript expression to apply this Function to specific data events. See [Build Custom Logic to Route and Process Your Data](filter-and-transform-data) for examples.
   - **Description:** A brief description of how this Function modifies your data to help other users understand its purpose later. Defaults to empty.
   - **Delete original:** When toggled on (default), the data output only includes the folded field names, meaning it removes the original data from the raw location. When toggled off, the data output includes both the folded and original field names.
   - **Separator string:** Set a separator character to use as the path separator. For example: `/` or `_`. This separator determines how to group the fields. Defaults to `.` (dot).
   - **Selection regular expression (optional):** Input a regular expression. If defined, the Function only groups field names that match this expression, along with the separator string.

1. Test that you configured the Function correctly by comparing sample incoming data with outgoing data in the Data Preview pane and Pipeline Diagnostics tool. See [Data Preview](data-preview) for more information.

## Usage Examples {#usage}

The following is an example of raw data before applying the Fold Keys Function:

```
{
  "_raw": "Mar 27 23:45:05 PA-VM 2020/05/07 02:40:14,44A1B3FC68F5304,TRAFFIC,end,192.0.2.0,192.0.2.255,untrusted,trusted,ethernet1/3,ethernet1/2,tcp,allow,aged-out,United States",
  "_time": 1743144305,
  "source": "cribl",
  "data_metrics_bytes": 12345678,
  "data_metrics_count": 987,
  "data_metrics_latency_p95": 123,
  "data_metrics_latency_p90": 56,
  "host_ip": "203.0.113.0"
}
```

In this example, there are multiple fields that begin with `data_metrics`. You could use the Fold Keys Function to group these field names together into a more logical, nested data structure.

In this example, the Fold Keys Function uses these settings:

- **Delete original:** Toggled on
- **Separator string:** `_`
- **Selection regular expression:** `^data`

Together, these settings nest all fields that start with `data_` and creates two hierarchies using the underscore `_` character as the separator string. The Regular Expression `^data` matches any string that starts with the word `data`.

After the Fold Key Function processes the data, the output looks like this:

```
{
  "_raw": "Mar 27 23:45:05 PA-VM 2020/05/07 02:40:14,44A1B3FC68F5304,TRAFFIC,end,192.0.2.0,192.0.2.255,untrusted,trusted,ethernet1/3,ethernet1/2,tcp,allow,aged-out,United States",
  "_time": 1743144305,
  "source": "cribl",
  "data": {
    "metrics": {
      "bytes": 12345678,
      "count": 987,
      "latency": {
        "p95": 123,
        "p90": 56
      }
    }
  },
  "host_ip": "203.0.113.0"
}
```
