# Flatten


The Flatten Function flattens fields out of a nested structure. It pulls up nested key-value pairs (fields) to a higher level in the object. You can specify: 
- Individual fields to flatten.
- The depth at which to flatten fields. 
- The delimiter to use when concatenating keys.
- An optional prefix to add to transformed field names.

> The Flatten Function creates fully qualified names for promoted fields. If you simply need to promote fields without transforming their names, use the `eval` [Function](eval-function).
>
{.box .success}

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Fields**: List of top-level fields to include for flattening. Defaults to an empty array, which means all fields. Limit to specific fields by typing in their names, separated by hard returns. Supports wildcards (`*`). Supports double-underscore (`__`) internal fields only if individually enumerated – not via wildcards.

**Prefix**: Prefix string for flattened field names. Defaults to empty.

**Depth**: Number representing the nested levels to consider for flattening. Minimum `1`. Defaults to `5`.

**Delimiter**: Delimiter to use for flattening. Defaults to `_` (underscore).

## Example

Add the following test sample in **Preview** > **Paste a Sample**:

```json {title="input"}
{
	"accounting": [{
		"firstName": "John",
		"lastName": "Doe",
		"age": 23
	}, {
		"firstName": "Mary",
		"lastName": "Smith",
		"age": 32
	}],
	"sales": [{
		"firstName": "Sally",
		"lastName": "Green",
		"age": 27
	}, {
		"firstName": "Jim",
		"lastName": "Galley",
		"age": 41
	}]
} 
```

Under **Select Event Breaker**, choose **ndjson** (newline-delimited JSON), and click **Save as a Sample File**.

Here's sample output with all settings at default:

```json {title="output"}
{
  "accounting_0_firstName": "John",
  "accounting_0_lastName": "Doe",
  "accounting_0_age": 23,
  "accounting_1_firstName": "Mary",
  "accounting_1_lastName": "Smith",
  "accounting_1_age": 32,
  "sales_0_firstName": "Sally",
  "sales_0_lastName": "Green",
  "sales_0_age": 27,
  "sales_1_firstName": "Jim",
  "sales_1_lastName": "Galley",
  "sales_1_age": 41,
}
```

Using the Flatten Function’s default settings, we successfully create top-level fields from the nested JSON structure, as expected.