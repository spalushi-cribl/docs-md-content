# Parsers Library


## What Are Parsers

Parsers are definitions and configurations for the [Parser Function](parser-function).
The purpose of the Parsers Library is to provide an interface for creating and editing Parsers.
The library is searchable, and each parser can be tagged as necessary.

To find the library, select **Worker Groups** from the sidebar, and choose a Worker Group.
Then, on the **Worker Groups** submenu, select **Processing**, then **Knowledge**, then **Parsers**.

![Parsers Library](parsers-library-4.9.png)
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

1. In the sidebar, select **Worker Groups** (Cribl Stream) or **Fleets** (Cribl Edge) from the sidebar, and choose a Worker Group or Fleet.
1. On the **Worker Groups**/**Fleets** submenu, select **Processing** (Cribl Stream) or **More** (Cribl Edge), then **Knowledge**, then **Parsers**.
1. Select **Add Parser**. 
1. Enter a unique **ID**.
1. Optionally, enter a **Description**.
1. Select a **Type** (see the supported types above).
1. Enter the **List of fields** expected to be extracted, in order.
   Select this field's Maximize icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.
1. Optionally, enter any desired **Tags**.

![Adding a new parser](parser-new-add.c821bba64d.png)
{border="true"}
