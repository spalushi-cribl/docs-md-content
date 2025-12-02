# extract_all


The `extract_all` function gets all matches for an RE2 regular expression from a source string. Optionally, retrieve a subset of matching groups.

## Syntax

    `extract_all(Regex, [CaptureGroups,] Source)`

### Arguments

* **Regex**: A regular expression containing between 1 and 16 capture groups.
    * Example of a valid regex: `@"(\d+)"`
    * Example of an invalid regex: `@"\d+"`
* **CaptureGroups**: A dynamic array constant that indicates the capture group to extract. Valid values are from 1 to the number of capturing groups in the regular expression. Named capture groups are allowed. 
    * Named capture group syntax: `?<groupName>`
* **Source**: A string to search.

## Results

* If **Regex** finds a match in **Source**: Returns dynamic array including all matches against the indicated capture groups **CaptureGroups**, or all of capturing groups in the **Regex**.
* If number of **CaptureGroups** is 1: The returned array has a single dimension of matched values.
* If number of **CaptureGroups** is more than 1: The returned array is a two-dimensional collection of multi-value matches per **CaptureGroups** selection, or all capture groups present in the regex if **CaptureGroups** is omitted.
* If there's no match: `null`.

## Examples

### Extract two capture groups

Breaks `interface_id` into two parts, `Family` and `Name`:

```kusto {runSearch=true}
dataset="cribl_search_sample" | extend interfaceParts=extract_all(@"(?<Family>\w{3})\-(?<Name>\S+)\s?", interface_id)
```

### Extract a single capture group

Returns hex-byte representation (two hex-digits) of the GUID. Let Id=`"82b8be2d-dfa7-4bd1-8f63-24ad26d31449"`:

```kusto
| extend guid_bytes = extract_all(@"([\da-f]{2})", Id)
```

ID|guid_bytes
-----|-----
82b8be2d-dfa7-4bd1-8f63-24ad26d31449|`["82","b8","be","2d","df","a7","4b","d1","8f","63","24","ad","26","d3","14","49"]`

### Extract several capture groups

Uses a regular expression with three capturing groups to split each GUID part into first letter, last letter, and whatever is in the middle. Let Id=`"82b8be2d-dfa7-4bd1-8f63-24ad26d31449"`:

```kusto
| extend guid_bytes = extract_all(@"(\w)(\w+)(\w)", Id)
```

ID|guid_bytes
-----|-----
82b8be2d-dfa7-4bd1-8f63-24ad26d31449|`[["8","2b8be2","d"],["d","fa","7"],["4","bd","1"],["8","f6","3"],["2","4ad26d3144","9"]]`

### Extract a subset of capture groups

Shows how to select a subset of capturing groups. The regular expression matches the first letter, last letter, and all the rest. The captureGroups parameter is used to select only the first and the last parts. Let Id=`"82b8be2d-dfa7-4bd1-8f63-24ad26d31449"`:

```kusto
| extend guid_bytes = extract_all(@"(\w)(\w+)(\w)", dynamic([1,3]), Id)
```

ID|guid_bytes
-----|-----
82b8be2d-dfa7-4bd1-8f63-24ad26d31449|`[["8","d"],["d","7"],["4","1"],["8","3"],["2","9"]]`

### Using named capture groups

You can use named capture groups of RE2 in `extract_all`. The **CaptureGroups** uses both capture group indexes and named capture group reference to fetch matching values. Let Id=`"82b8be2d-dfa7-4bd1-8f63-24ad26d31449"`:

```kusto
| extend guid_bytes = extract_all(@"(?&lt;first>\w)(?&lt;middle>\w+)(?&lt;last>\w)", dynamic(['first',2,'last']), Id)
```

ID|guid_bytes
-----|-----
82b8be2d-dfa7-4bd1-8f63-24ad26d31449|`[["8","2b8be2","d"],["d","fa","7"],["4","bd","1"],["8","f6","3"],["2","4ad26d3144","9"]]`

