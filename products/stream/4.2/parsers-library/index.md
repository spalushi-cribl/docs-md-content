# Parsers Library


## What Are Parsers

Parsers are definitions and configurations for the [Parser Function](parser-function). You can find  the library from Cribl Stream's top nav under **Processing** > **Knowledge** > **Parsers**, and its purpose is to provide an interface for creating and editing Parsers. The library is searchable, and each parser can be tagged as necessary.

![Parsers Library](parsers-library.cacd616007.png)
{border="true"}

Parsers can be used to **extract** or **reserialize** events. See [Parser Function](parser-function) page for examples. 

### Supported Parser Types: 

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

To create a parser, follow these steps: 

  1. Go to **Knowledge > Parsers** and click **Add New**. 
  2. Enter a unique **ID**.
  3. Optionally, enter a **Description**.
  4. Select a **Parser type** (see the supported types above).
  5. Enter the **List of fields** expected to be extracted, in order.
    Click this field's Maximize icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.
  6. Optionally, enter any desired **Tags**.

![Adding a new parser](parser-new-add.5adb2e3684.png)
{border="true"}
