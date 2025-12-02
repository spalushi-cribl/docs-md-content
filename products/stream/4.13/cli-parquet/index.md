# parquet


For any Parquet file: view (`cat`) the file, its metadata (`details`), or its `schema`.

> This command is available only on Linux.
>
{.box .info}

**Sub-commands:**

- [`parquet cat`](#parquet-cat)
- [`parquet details`](#parquet-details)
- [`parquet schema`](#parquet-schema)

## Sub-commands and Options

### `parquet cat` {#parquet-cat}

Cat Parquet events to console.

**Usage:**

To view a Parquet file:

```text
./cribl parquet cat -f /some_parquet_file
```

**Sample response:**

```shell
{"__bytes":274,"Unnamed: 0":0,"name":"apples","quantity":10,"price":2.6,"date":"2018-04-02 20:29:00","day":"2017-11-26","finger":"b'FNORD'","inter":"b'*\\x00\\x00\\x00\\x17\\x00\\x00\\x00\\t\\x03\\x00\\x00'","stock":"[{'quantity': array([10]), 'warehouse': 'A'}\n {'quantity': array([20]), 'warehouse': 'B'}]","colour":"['green' 'red']","meta_json":null}
...
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-f <file>` | Path to Parquet file. |
| `-m <max>` | Maximum events to print. |

### `parquet details` {#parquet-details}

View all Parquet details (schema and statistics) and metadata, including encoding and compression.

**Usage:**

```text
./cribl parquet details -f /some_parquet_file
```

**Sample response:**

```shell
{
  "schema": {
    "Unnamed: 0": {
      "optional": true,
      "type": "INT64"
    },
    ...
  },
  "path": "../../../src/sluice/js/util/__tests__/data/parquet/single/small_fruits.parquet",
  "num_rows": 40000,
  "num_cols": 11,
  "num_real_cols": 11,
  "num_row_groups": 1,
  "created_by": "parquet-cpp-arrow version 11.0.0",
  "schema_root_name": "schema",
  "metadata": {
    "pandas": "{\"index_columns\": [{\"kind\": \"range\", \"name\": null, \"start\": 0, \"stop\": 40000, \"step\": 1}],
    ...
  },
  "version": "PARQUET_2_6",
  "metadataSize": 6072,
  ...
    }
  ]
}
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-f <file>` | Path to Parquet file. |

### `parquet schema` {#parquet-schema}

View a Parquet file's schema (not including encoding or compression), expressed as JSON like in the [Parquet Schema editor](parquet-schemas#the-parquet-schema-editor).

**Usage:**

```text
./cribl parquet schema -f /some_parquet_file
```

**Sample response:**

```shell
{
  "Unnamed: 0": {
    "optional": true,
    "type": "INT64"
  },
  "name": {
    "optional": true,
    "type": "STRING"
  },
  "quantity": {
    "optional": true,
    "type": "DOUBLE"
  },
  "price": {
    "optional": true,
    "type": "DOUBLE"
...
  }
}
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-f <file>` | Path to Parquet file. |