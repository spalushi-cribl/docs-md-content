# CEF Serializer



The CEF Serializer takes a list of fields and/or values, and formats them in the Common Event Format (CEF) standard. CEF defines a syntax for log records. It is composed of a standard prefix, and a variable extension formatted as a series of key-value pairs.

### Format

`CEF:Version|Device Vendor|Device Product|Device Version|Device Event Class ID|Name|Severity|[Extension]`

## Usage


**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Output field**: The field to which the CEF formatted event will be output. Nested addressing supported. Defaults to `_raw`.

### Header Fields

CEF Header field definitions. The field values below will be written pipe (`|`)–delimited in the Output Field. Names cannot be changed. Values can be computed with a JavaScript expression, or can be constants. 

   - **cef_version**: Defaults to `CEF:0`.
   - **device_vendor**: Defaults to `Cribl`. 
   - **device_product**: Defaults to `Cribl`. 
   - **device_version**: Defaults to `C.version`. 
   - **device_event_class_id**: Defaults to `420`. 
   - **name**: Defaults to `Cribl Event`. 
   - **severity**: Defaults to `6`. 

### Extension Fields

CEF Extension field definitions. Field names and values will be written in `key=value` format. Select each field's Name from the drop-down list. Values can be computed with a JavaScript expression, or can be constants.

## Example

For each CEF field, allowed values include strings, plus any custom Cribl function. For example, if using a lookup:

Name: `Name`
Value expression: `C.Lookup('lookup-exact.csv', 'foo').match('abc', 'bar')`

This can be used for any of the `CEF` **Header Fields**.

![](cef-serializer-fields.07afe769aa.png)
{border="true"}

The resulting event has the following structure for an **Output Field** set to `_CEF_out`:

`_CEF_out:CEF:0|Cribl|Cribl|42.0-61c12259|420|Business Group 6|6|c6a1Label=Colorado_Ext_Bldg7`