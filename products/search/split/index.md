# split



The `split` function splits a given string according to a given delimiter and returns a string array with the contained substrings. Optionally, a specific substring can be returned if it exists.

## Syntax

    `split( Source, Delimiter [, RequestedIndex] )`

### Arguments

* **Source**: The source string that will be split according to the given delimiter.
* **Delimiter**: The delimiter that will be used to split the source string.
* **RequestedIndex**: An optional zero-based index `int`. If provided, the returned string array will contain the requested substring if it exists.

## Results

A string array that contains the substrings of the given source string that are delimited by the given delimiter.

## Examples

* `split("aa_bb", "_")` returns as `["aa","bb"]`
* `split("aaa_bbb_ccc", "_", 1)` returns as `["bbb"]`
* `split("", "_")` returns as `[""]`
* `split("a__b", "_")` returns as `["a","","b"]`
* `split("aabbcc", "bb")` returns as `["aa","cc"]`

### Example Query: Split URL Path Segments {#split-url}

```kusto {runSearch=true}
dataset=$vt_dummy
| extend url = '/api/v1/items/42'
| extend segments = split(url, '/'),
         resource = split(url, '/', 3),   // ["items"]
         id = split(url, '/', 4)          // ["42"]
```

### Example Query: Split Comma-Separated Values {#split-csv}

```kusto {runSearch=true}
dataset=$vt_dummy
| extend line = 'alpha,beta,gamma'
| extend parts = split(line, ','),
         second = split(line, ',', 1)    // ["beta"]
```

### Example Query: Isolate Filename Extension {#example-filename}

```kusto {runSearch=true}
dataset=$vt_dummy
| extend filename = 'awesome.log'
| extend basename = split(filename, '.', 0) // ["awesome"]
```

