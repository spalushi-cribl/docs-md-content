# extract



The `extract` function gets a match for an RE2 regular expression from a source string. Optionally, convert the extracted substring to the indicated type.

> To extract fields, field values, or key-value pairs from events, using a wide range of flexible options, see the `extract` [operator](extract-operator).
{.box .info}

## Syntax

    `extract( Regex, CaptureGroup, Source [, TypeLiteral] )`

### Arguments

* **Regex**: An RE2 regular expression.
* **CaptureGroup**: A positive `int` constant indicating the capture group to extract. 0 stands for the entire match, 1 for the value matched by the first '('parenthesis')' in the regular expression, 2 or more for subsequent parentheses.
* **Source**: A string to search.
* **TypeLiteral**: An optional type literal (for example, `typeof(long)`). If provided, the extracted substring is converted to this type.

## Results

If regex finds a match in **Source**: the substring matched against the indicated capture group **CaptureGroup**, optionally converted to **TypeLiteral**.

If there's no match: a blank string.

If the type conversion fails: `null`.

## Examples

### Extract a Single Field Value

This example works with Apache access log events of this form:

 ```
 128.241.220.82 - - [07/Mar/2025:18:30:13 +0000] "GET /product.screen?product_id=Goats&JSESSIONID=SD3SL1FF7ADFF8 HTTP/1.1" 200 3222 "https://criblshop.com" "BlackBerry8520/5.0.0.681 Profile/MIDP-2.1 Configuration/CLDC-1.1 VendorID/120"
 ```

 You could extract the `JSESSIONID` value into a field called `sessionId` using this expression:

 ```kusto {runSearch=true}
 dataset="cribl_search_sample" dataSource="access_combined" 
 | limit 10000 
 | extend sessionId = extract(@'JSESSIONID=(.*?)\s', 1, _raw)
 ```

### Extract and Convert a Capture Group

This example works with VPC Flow Logs events of this form:

```
2 602320997947 eni-7vv3s9v4cng64k53i 10.0.0.138 10.0.0.138 22 3389 6 60 32460 1741637275 1741637302 ACCEPT OK
```

By default, a Cribl Search [Datatype rule](datatypes#aws-vpc-flow-log-event-breaker) parses a `start` field that identifies the session start time – in seconds, in Unix epoch time – represented as a `string`. But this format makes it hard to identify when a session started. Using `extract`, we parse the session start time from the raw log, and also convert the result to a `datetime` object. This creates a new field called `sessionStart`, which represents the start time in human-readable form.

```kusto {runSearch=true}
 dataset="cribl_search_sample" dataSource="vpcflowlogs"
| limit 1000
// Extract the flow 'start' time and convert it from a string to a datetime object.
| extend sessionStart = extract(@'^(?:[^\s]+\s){10}([^\s]+)', 1, _raw, typeof(datetime))
```

Given the example event shown above, this expression would parse the start value of `1741638523` from the raw log, and show it as `2025-03-10T20:28:43.000Z`.