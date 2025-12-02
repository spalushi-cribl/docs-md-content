# Parquet Schemas


Cribl Edge supports two kinds of schemas:

- [Parquet schemas](#parquet-schemas) for writing data from a Cribl Edge Destination to Parquet files. Manage these in the UI at **Knowledge** > **Parquet Schemas**.
- [JSON schemas](schema-library) for validating JSON events. Manage these in the UI at **Knowledge** > **Schemas**.

These schemas obviously serve different purposes. Beware of assuming that operations that work for one kind of schema can be used with the other. For example, the validation method for JSON schemas – `C.Schema('<schema_name>').validate(<object_field>)` – can't be used to validate Parquet schemas.

## Configuring Parquet Schemas {#parquet-schemas}

Destinations whose **General Settings** > **Data format** drop-down includes a `Parquet` option can write out data as files in the [Apache Parquet](https://parquet.apache.org/docs/file-format/) columnar storage format.

Before configuring a Destination for Parquet output, you should add an existing Parquet schema, or create a new Parquet schema, that suits the data you're working with. You do not need to start from scratch: Cribl provides sample Parquet schemas for you to clone and then customize as needed.

From the top nav, select **Processing** > **Knowledge**, then select the **Parquet Schemas** left tab.

If you're adding a Parquet schema:
* Click **+ Add New** to open the **New Parquet schema** modal.

If you're creating a Parquet schema:
1. Click the name of the schema you want to start with, to open it in a modal. 
2. Click **Clone Parquet Schema** to open the **New Parquet schema** modal.
3. Give the new schema a name and description. 

From here, whether you're working with a brand-new or cloned schema:  

4. Add and/or edit fields as desired.  
5. Click **Save**.

![Creating a Parquet schema](schema-library-parquet.54c563b403.png)

Now, when you configure your Destination, the schema you created will be available from the **Parquet Settings** > **Parquet schema** drop-down.

### Working with Parquet in Cribl Edge {#working-with-parquet}

Different Parquet readers and writers behave differently. Keep the following guidelines in mind when working with Parquet in Cribl Edge.

#### The Parquet Schema Editor

In the Parquet Schema editor, you express your Parquet schema in JSON. This does not look like the examples in the Parquet spec, which have their own syntax (like the repetition/name/type triple), but they are functionally equivalent. See the [examples](#parquet-as-json) below.

The editor provides autocompletion and validation to guide you. (With deeply nested schemas, Cribl Edge does not yet fully support autocompletion, and v.4.0.3 or later is required for full validation support.)

#### File Extensions

For now, Cribl Edge can read Parquet files only if they have the extension `.parquet`, `.parq`, or `.pqt`. 

#### Field Content

When Cribl Edge writes to a Parquet file:

* If the data contains a field that **is not** present in the schema – i.e, an extra field – Cribl Edge writes out the parent rows, but omits the extra field.

* If the data contains a field that **is** present in the Parquet schema, but whose properties violate the schema, Cribl Edge treats this as a data mismatch. Cribl Edge drops the rows containing that field – it does not write those rows to the output Parquet file at all.

* If the data contains JSON, the JSON must be [stringified](https://www.w3schools.com/js/js_json_stringify.asp). Otherwise, Cribl Edge treats this as a data mismatch, and does not write out the row. For example, this valid (but not stringified) JSON will trigger a data mismatch:  
`{ "name": "test"}`.  

  The same JSON in stringified form will work fine:  
  `"{\"name\": \"test\"}"`.

#### Data Types

Cribl Edge supports:

* All primitive types.
* All logical types.
* All converted types. 

Converted types have been superseded by logical types, as described in the [Apache Parquet docs](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md). Cribl Edge can read Parquet files that use converted types, but will write out the same data using corresponding logical types.

#### Repetition Type

You have three alternatives when defining a field's Repetition type:
- Set `optional` to `true`.
- Set `repeated` to `true`.
- Set neither `optional` nor `repeated`. This **implicitly** sets the Repetition type to `required`, and it is the default. 

Usage guidelines:
- Do not set both `optional` **and** `repeated` to `true`. 
- Do not use the `required` key at all.
- Instead of omitting `optional`, you have the option to include it, but set it to `false`.
- Instead of omitting `repeated`, you have the option to include it, but set it to `false`.
- If any field's Repetition type is `repeated`, Cribl Edge represents this field as a single key whose value is an array – not as separate key-value pairs with identical keys.

#### Encodings

- Among the `*DICTIONARY` encodings, Cribl Edge supports only `DICTIONARY`. Trying to assign the unsupported encodings `PLAIN_DICTIONARY` or `RLE_DICTIONARY` will produce an error.

- `BYTE_STREAM_SPLIT` encoding can be used only with `DOUBLE` or `FLOAT` types, and otherwise produces errors.

- The `RLE` and all `DELTA*` encodings also produce errors.



## Parquet Schema Expressed as JSON {#parquet-as-json}

These examples should give you a hint of how to express Parquet schema in JSON. For a full explanation of the Parquet syntax, see the [Parquet spec](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md).

### List Example

In this example, the whole schema is named `the_list`, and its structure consists of a container named `list` whose zero or more elements are each named `element`. Note that in Parquet schema, to specify that a field is nested (i.e., contains other fields), you just give it the type `group`. 

```shell {title="Parquet Schema Example 1"}
message schema {
  REQUIRED group the_list (LIST) {
    REPEATED group list {
      REQUIRED BYTE_ARRAY element (STRING);
    }
  }
}
```

Expressed as JSON, the same schema is more spread-out, because to specify that a field is nested, you give it a sub-field named `fields`. (This is equivalent to declaring the nested field's type as `group`.)

```json {title="Parquet Schema Example 1 Expressed as JSON"}
{
  "the_list": {
    "type": "LIST",
    "fields": {
      "list": {
        "repeated": true,
        "fields": {
          "element": {
            "type": "STRING"
          }
        }
      }
    }
  }
}
```

### Map Example

In this example, the whole schema is named `the_map`, and its structure consists of a container named `key_value` whose zero or more pairs of elements each contain a `key` and a `value` element. Both the `key` and the `value` are strings in this case, but they could be other data types.

```shell {title="Parquet Schema Example 2"}
message schema {
  REQUIRED group the_map (MAP) {
    REPEATED group key_value {
      REQUIRED BYTE_ARRAY key (STRING);
      REQUIRED BYTE_ARRAY value (STRING);
    }
  }
}
```

```json {title="Parquet Schema Example 2 Expressed as JSON"}
{
  "the_map": {
    "type": "MAP",
    "fields": {
      "key_value": {
        "repeated": true,
        "fields": {
          "key": {
            "type": "STRING"
          },
          "value": {
            "type": "STRING"
          }
        }
      }
    }
  }
}
```
### Array of Arrays Example

In this example, the whole schema is named `array_of_arrays`, and its structure consists of a container named `list` whose zero or more elements – which are themselves lists – are each named `element`. Within each of those `element` lists are one or more additional `list`/`element` structures.

```shell {title="Parquet Schema Example 3"}
message schema {
  REQUIRED group array_of_arrays (LIST) {
    REPEATED group list {
      REQUIRED group element (LIST) {
        REPEATED group list {
          REQUIRED INT64 element;
        }
      }
    }
  }
}
```

Expressed as JSON, the same schema is cumbersome to read compared to Parquet, which was designed to express nested structures concisely.

```json title="Parquet Schema Example 3 Expressed as JSON"
{
  "array_of_arrays": {
    "type": "LIST",
    "fields": {
      "list": {
        "repeated": true,
        "fields": {
          "element": {
            "type": "LIST",
            "fields": {
              "list": {
                "repeated": true,
                "fields": {
                  "element": {
                    "type": "INT64"
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```