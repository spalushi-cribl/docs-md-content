# Serialize


Use the Serialize Function to convert an event's content into a predefined format.

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Type**: Data output format. Defaults to `CSV`. Options:

* **CSV**
* **Extended Log File Format** – [Extended Log File Format](https://www.w3.org/TR/WD-logfile.html).
* **Common Log Format** – [Common Log Format](https://en.wikipedia.org/wiki/Common_Log_Format).
* **Key=Value Pairs** – selecting this type displays additional options:
  * **Clean fields**: Toggle on to clean field names by replacing non-alphanumeric characters with `_`. This will also strip leading and trailing `"` symbols.
  * **Pair delimiter**: Delimiter used to separate `key=value` pairs. Defaults to a single space character. Multiple characters are allowed. Cannot be an equal sign (`=`).
* **JSON Object**
* **Delimited values** – selecting this type displays additional options:
  * **Delimiter**: Delimiter character to split value. If left blank, defaults to comma (`,`). You can also specify pipe (`|`) or tab characters.
  * **Quote char**: Character used to quote literal values. If left blank, defaults to `"`.
  * **Escape char**: Character used to escape delimiter or quote characters. If left blank, defaults to **Quote char**.
  * **Null value**: Field value representing the null value. These fields will be omitted. Defaults to: `-`

**Library**: Browse Parser/Formatter library.

**Fields to serialize**: List of fields to include in the output after this Function processes the event. For `CSV`, `ELFF`, `CLF`, and `Delimited values` formats, you must list each field explicitly. Wildcards (`*`) are not supported for these formats. For all other formats, you can use wildcard (`*`) field lists, which support both:

- Wildcards (`*`) to include multiple fields.
- Nested addressing, where you can reference a parent field and all its children using a wildcard. For example, if you convert `_raw` into an object, use `_raw*` to refer to `_raw` and all nested fields inside it. See [Wildcard Lists](filter-and-transform-data#wildcard-lists) for more information.

> When using wildcard lists, all exclusions (such as `!fieldname`) must come before any inclusions (such as `fieldname` or `*`).
>
{.box .warning}

**Source field**: Field containing the object to serialize. Leave blank to serialize top-level event fields.

**Destination field**: Field to serialize the data into. Defaults to `_raw`.

## Examples

### Scenario A: JSON to CSV

Assume a simple event that looks like this: `{"time":"2019-08-25T14:19:10.240Z","channel":"input","level":"info","message":"initializing input","type":"kafka"}`

We want to serialize these fields: `_time`, `channel`, `level`, and `type` into a single string, in CSV format, stored in a new destination field called `test`.

To properly extract the key-value pairs from this event structure, we’ll use a built-in Event Breaker:

1. Copy the above sample event to your clipboard.
2. In the **Preview** pane, select **Paste a Sample**, and paste in the sample event.
3. Under **Select Event Breaker**, choose **ndjson** (newline-delimited JSON), and select **Save as a Sample File**.

Now you’re ready to configure the Serialize Function, using the settings below:

   Type: `CSV`
   Fields to Serialize: `_time channel level type`
   Destination Field: `test`
   Source Field:  [leave empty]
   **Result**: `test: 1566742750.24,input,info,kafka`

In the new `test` field, you now see the `time`, `channel`, `level`, and `type` keys extracted as top-level fields.

### Scenario B: CSV to JSON
Let’s assume that a merchant wants to extract a subset of each customer order, to aggregate anonymized order statistics across their customer base. The transaction data is originally in CSV format, but the statistical data must be in JSON.

Here’s a CSV header (which we don’t want to process), followed by a row that represents one order:

`orderID,custName,street,city,state,zip`
`20200622102822,john smith,100 Main St.,Anytown,AK,99911`

To convert to JSON, we’ll need to first parse each field from the CSV to a manipulable field in the Pipeline, which the Serialize Function will be able to reference. In this example, the new manipulable field is `message`.

Use the `Parser` Function:

Filter: `true`
Operation mode: `Extract`
Type: `CSV`
Source field: `_raw`
Destination field: `message`
List of fields: `orderID custName street city state zip`

Now use the Serialize Function:

Filter: `true`
Type: `JSON`
Fields to serialize: `city state`
Source field: `message`
Destination field: `orderStats`
