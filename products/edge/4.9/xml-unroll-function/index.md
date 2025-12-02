# XML Unroll


The XML Unroll Function accepts a proper XML event with a set of elements, and converts the elements into individual events. 

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Unroll elements regex**: Path to the array to unroll. E.g.: `^root\.child\.ElementToUnroll$`

**Copy elements regex**: Regex matching elements to copy into each unrolled event. 
E.g.: `^root\.(childA|childB|childC)$`

**Unroll index field**: Cribl Edge will add a field with this name, containing the 0-based index at which the element was located within the event. In Splunk, this will be an index-time field. Supports nested addressing. Name defaults to `unroll_idx`.

**Pretty print**: Whether to pretty print the output XML.

## Examples 

Assume that the following sample is ingested as a single event:

```xml {title="sample.xml"}
<?xml version="1.0" encoding="UTF-8"?>
<Parent>
    <myID>123456</myID>
    <branchLocation>US</branchLocation>
    <Child>
        <state>NY</state>
        <city>New York</city>
    </Child>
    <Child>
        <state>NJ</state>
        <city>Edgewater</city>
    </Child>
    <Child>
        <state>CA</state>
        <city>Oakland</city>
    </Child>
    <Child>
        <state>CA</state>
        <city>San Francisco</city>
    </Child>
</Parent>
```



> If you insert this sample using **Preview** > **Add a Sample** > **Paste a Sample**, adjust [Event Breaker](event-breakers) settings to add the sample as a single event. One way to do this is to add a regex Event Breaker that (by design) will not match anything present in the sample. For example: `/[\n\r]+donotbreak(?!\s)/`. In current Cribl Edge versions, you can also use the built-in **Do Not Break** Ruleset.
>
{.box .info}

Set up the XML Unroll Function using these settings: 

**Unroll elements regex**: `^Parent\.Child$` 
**Copy elements regex**: `^Parent\.(myID|branchLocation)$`

Output 4 Events:

```xml {title="Resulting Events"}
# Event 1
<?xml version="1.0"?>
<Child>
  <myID>123456</myID>
  <branchLocation>US</branchLocation>
  <state>NY</state>
  <city>New York</city>
</Child>

# Event 2
<?xml version="1.0"?>
<Child>
  <myID>123456</myID>
  <branchLocation>US</branchLocation>
  <state>NJ</state>
  <city>Edgewater</city>
</Child>

# Event 3
<?xml version="1.0"?>
<Child>
  <myID>123456</myID>
  <branchLocation>US</branchLocation>
  <state>CA</state>
  <city>Oakland</city>
</Child>

# Event 4
<?xml version="1.0"?>
<Child>
  <myID>123456</myID>
  <branchLocation>US</branchLocation>
  <state>CA</state>
  <city>San Francisco</city>
</Child>
```

## See Also {#see-also}

- [Reducing Windows XML Events](/stream/usecase-win-xml/), which demonstrates using several Cribl Stream Functions to parse WindowsXML events and significantly reduce their volume.
