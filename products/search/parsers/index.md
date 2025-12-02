# Parsers


Define data to extract, using Parsers.

---

Parsers in Cribl Search are definitions for [Datatypes](datatypes) and the [`extract`](extract-operator) operator.

To open the **Parsers** page, select **Knowledge** > **Parsers**. The Parsers page provides an interface for
creating and editing Parsers. The table is searchable, and you can add Tags to each Parser as necessary.


![Parsers page](search-parsers-page.png)
{scale="90%"}


> For details about who can access and modify this resource, see [Search Member Permissions](cloud-managing#member-roles).
>
{.box .info}

### Supported Parser Types

- **CSV**: Comma-separated values.
- **Extended Log File Format**: [Extended Log File Format](https://www.w3.org/TR/WD-logfile.html).
- **Common Log Format**: [Common Log Format](https://en.wikipedia.org/wiki/Common_Log_Format).
- **Key=Value Pairs**: A set of data that represents two associated groups through a key and a value.
- **JSON Object**: JavaScript Object Notation (JSON) is a standard text-based format for representing structured data
  based on JavaScript object syntax.
- **Delimited values**: A character identifies the beginning or the end of a character string.
- **Regular Expression**: A sequence of characters that specifies a match pattern in text.
- **Grok**: A string of special characters and regular expressions (pattern) that match data.

### Creating a Parser

To create a Parser, follow these steps:

1. Go to **Knowledge** > **Parsers** and select **Add Parser**.
1. Enter a unique **ID**.
1. Optionally, enter a **Description**.
1. Optionally, enter any desired **Tags**.
1. Select a Parser **Type** (see the supported types above).
1. Enter the **List of fields** expected to be extracted, in order. Select this field's advanced mode icon (far right)
   if you'd like to open a modal where you can work with sample data and iterate on results.
1. Based on the **Type** selected, you'll also have additional configurations. See the below sections for details,
   either [Key=Value Pairs](#kvp), [Delimited Values](#delim), [Regular Expressions](#regex), or [Grok](#grok).
1. Select **Save** when you're finished.

#### Key=Value Pairs {#kvp}

- **Clean fields**: Whether to clean field names by replacing non-alphanumeric characters `[a-zA-Z0-9]` with an
  underscore `_`.
- **Allowed key characters**: A list of characters that can appear in a key name, even though they're normally separator
  or control characters.
- **Allowed value characters**: A list of characters that can appear in a value name, even though they're normally
  separator or control characters.

#### Delimited Values {#delim}

- **Delimiter**: Delimiter character to use to split values.
- **Quote character**: Character used to quote values. Required if values contain the delimiter character.
- **Escape character**: Character used to escape characters within a value.
- **Null value**: String value that should be treated as null or undefined.

#### Regular Expression {#regex}

- **Regex**: Regex literal with named capturing groups, for example, `(?<foo>bar)`. Or with `_NAME_` and `_VALUE_`
  capturing groups, for example, `(?<_NAME_0>[^ =]+)=(?<_VALUE_0>[^,]+)`.
- **Additional regex**: Add another regular expression to match against other data.
- **Max exec**: The maximum number of times to apply the regex to the source field when the global flag is set, or when
  using named capturing groups.
- **Field name format expression**: JavaScript expression to format field names when `_NAME_n` and `_VALUE_n` capturing
  groups are used. The original field name is in the global variable `name`. For example, to append `XX` to all field
  names: `${name}_XX` (backticks are literal). If empty, names will be sanitized using this regex:
  `/^[_0-9]+|[^a-zA-Z0-9_]+/g`. You can access other field values via `__e.<fieldName>`.
- **Overwrite existing fields**: Overwrite existing event fields with extracted values. If set to **No**, existing
  fields will be converted to an array.

#### Grok {#grok}

- **Pattern**: Grok pattern to extract fields. Syntax supported: `%{PATTERN_NAME:FIELD_NAME}`.
- **Additional Grok patterns**: Add another pattern to match other data.
