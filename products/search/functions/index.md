# Functions


A comprehensive list of all functions supported in Cribl Search, grouped by category.

---

## Context Functions {#context}

[Context functions](context-functions) return contextual information about your search.


| Name                | Description                                                                             |
| ------------------- | --------------------------------------------------------------------------------------- |
| `createdTime()`     | Time when the search was created, in seconds.                                           |
| `earliestTime()`    | Beginning of the search's time range, in seconds.                                       |
| `jobID()`           | Unique search identifier.                                                               |
| `latestTime()`      | End of the search's time range, in seconds.                                             |
| `query()`           | Query string.                                                                           |
| `user()`            | Username of the user who created the search.                                            |
| `displayUsername()` | Friendly display name (typically first + last name) of the user who created the search. |

## Cribl Functions {#cribl}

Cribl functions can be used together with the [`summarize`](summarize), [`eventstats`](eventstats), and
[`timestats`](timestats) operators to aggregate your data. We refer to those additional functions as **Cribl**
functions, since they're specific to Cribl Search.


| Name                               | Description                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| [`findearliest`](findearliest)     | Returns the earliest value of an expression across the group.                                                |
| [`findearliestif`](findearliestif) | Returns the earliest value of an expression across the group for which a predicate evalutes to `true`.       |
| [`findfirst`](findfirst)           | Returns the first observed value of an expression across the group.                                          |
| [`findfirstif`](findfirstif)       | Returns the first observed value of an expression across the group for which a predicate evalutes to `true`. |
| [`findlast`](findlast)             | Returns the last observed value of an expression across the group.                                           |
| [`findlastif`](findlastif)         | Returns the last observed value of an expression across the group for which a predicate evalutes to `true`.  |
| [`findlatest`](findlatest)         | Returns the latest value of an expression across the group.                                                  |
| [`findlatestif`](findlatestif)     | Returns the latest value of an expression across the group for which a predicate evalutes to `true`.         |
| [`list`](list)                     | Returns the list of values of an expression across the group                                                 |
| [`median`](median)                 | Returns the middle value of an expression across the group.                                                  |
| [`medianif`](medianif)             | Returns the middle value of an expression across the group for which a predicate evalutes to `true`.         |
| [`persecond`](persecond)           | Returns the per-second rate of an expression across the group                                                |
| [`persecondif`](persecondif)       | Returns the per-second rate of an expression across the group for which a predicate evalutes to `true`.      |
| [`rate`](rate)                     | Returns the rate observed value of an expression across the group.                                           |
| [`rateif`](rateif)                 | Returns the rate observed value of an expression across the group for which a predicate evalutes to `true`.  |
| [`sumsq`](sumsq)                   | Returns the sum of squares of an expression across the group.                                                |
| [`sumsqif`](sumsqif)               | Returns the sum of squares of an expression across the group for which a predicate evalutes to `true`.       |
| [`values`](values)                 | Returns all of the distinct values of an expression across the group.                                        |


## Scalar Functions

Scalar functions perform calculations, transformations, or conversions.

### Binary Functions {#binary}


| Name                                       | Description                                                         |
| ------------------------------------------ | ------------------------------------------------------------------- |
| [`binary_and`](binary-and)                 | Returns a result of the bitwise `and` operation between two values. |
| [`binary_not`](binary-not)                 | Returns a bitwise negation of the input value.                      |
| [`binary_or`](binary-or)                   | Returns a result of the bitwise or operation of the two values.     |
| [`binary_shift_left`](binary-shift-left)   | Returns binary shift left operation on a pair of numbers.           |
| [`binary_shift_right`](binary-shift-right) | Returns binary shift right operation on a pair of numbers.          |
| [`binary_xor`](binary-xor)                 | Returns the bitwise `xor` operation of the two values.              |
| [`from_binary_string`](from-binary-string) | Takes a binary string and returns a number.                         |
| [`to_binary_string`](to-binary-string)     | Takes a number and returns a binary string.                         |


### Conditional Functions {#conditional}


| Name                        | Description                                                                                                                                                                                      |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`case`](case)              | Evaluates a list of predicates and returns the first result expression whose predicate is satisfied.                                                                                             |
| [`coalesce`](coalesce)      | Evaluates a list of expressions and returns the first non-null (or non-empty for string) expression.                                                                                             |
| [`iif`](iif) ([`iff`](iff)) | Evaluates the first argument (the predicate), and returns the value of either the second or third arguments, depending on whether the predicate evaluated to `true` (second) or `false` (third). |
| [`max_of`](max-of)          | Returns the maximum value of several evaluated numeric expressions.                                                                                                                              |
| [`min_of`](min-of)          | Returns the minimum value of several evaluated numeric expressions.                                                                                                                              |


### Conversion Functions {#conversion}


| Name                                                                  | Description                                                                              |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| [`bin`](bin)                                                          | Rounds values down to an integer multiple of a given bin size.                           |
| [`bin_auto`](bin-auto)                                                | Rounds values down to a fixed-size bin.                                                  |
| [`floor`](floor)                                                      | Rounds values down to an integer multiple of a given floor size.                         |
| [`gettype`](gettype)                                                  | Returns the type of the input value.                                                     |
| [`tobool`](tobool)                                                    | Converts the input to a [`bool`](bool) value.                                            |
| [`todouble`](todouble) ([`toreal`](toreal), [`todecimal`](todecimal)) | Converts the input to a [`double`](double) ([`real`](real), [`decimal`](decimal)) value. |
| [`toint`](toint) ([`tolong`](tolong))                                 | Converts the input to an [`int`](int) ([`long`](long)) value.                            |
| [`tostring`](tostring)                                                | Converts the input to a [`string`](string) value.                                        |


### DateTime Functions {#datetime}


| Name                                                                   | Description                                                                                                          |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [`ago`](ago)                                                           | Subtracts the given timespan from the current UTC clock time.                                                        |
| [`datetime_add`](datetime-add)                                         | Calculates a new datetime from a specified datepart multiplied by a specified amount, added to a specified datetime. |
| [`datetime_diff`](datetime-diff)                                       | Calculates the number of the specified periods between two datetime values.                                          |
| [`datetime_part`](datetime-part)                                       | Extracts the requested date part as an integer value.                                                                |
| [`dayofmonth`](dayofmonth)                                             | Returns the integer number representing the day number of the given month.                                           |
| [`dayofweek`](dayofweek)                                               | Returns an integer between `0` and `6` representing the day of the week, beginning on Sunday.                                      |
| [`dayofyear`](dayofyear)                                               | Returns the integer number representing the day number of the given year.                                            |
| [`endofday`](endofday)                                                 | Returns the end of the day containing the date, shifted by an offset, if provided.                                   |
| [`endofmonth`](endofmonth)                                             | Returns the end of the month containing the date, shifted by an offset, if provided.                                 |
| [`endofweek`](endofweek)                                               | Returns the end of the week containing the date, shifted by an offset, if provided.                                  |
| [`endofyear`](endofyear)                                               | Returns the end of the year containing the date, shifted by an offset, if provided.                                  |
| [`format_datetime`](format-datetime)                                   | Formats a datetime according to the provided format.                                                                 |
| [`format_timespan`](format-timespan)                                   | Formats a timespan according to the provided format.                                                                 |
| [`getmonth`](getmonth) ([`monthofyear`](monthofyear))                  | Gets the month number (1–12) from a datetime.                                                                        |
| [`getyear`](getyear) (`yearofyear`)                                    | Returns the year part of a datetime.                                                                                 |
| [`hourofday`](hourofday)                                               | Returns the integer number representing the hour number of the given date.                                           |
| [`make_datetime`](make-datetime)                                       | Converts the specified date and time into a datetime value (Unix time in seconds).                                   |
| [`make_timespan`](make-timespan)                                       | Converts the specified time period into a datetime value (Unix time in seconds).                                     |
| [`now`](now)                                                           | Returns the current UTC clock time as a datetime value (Unix time in seconds).                                       |
| [`startofday`](startofday)                                             | Returns the start of the day containing the date, shifted by an offset, if provided.                                 |
| [`startofmonth`](startofmonth)                                         | Returns the start of the month containing the date, shifted by an offset, if provided.                               |
| [`startofweek`](startofweek)                                           | Returns the start of the week containing the date, shifted by an offset, if provided.                                |
| [`startofyear`](startofyear)                                           | Returns the start of the year containing the date, shifted by an offset, if provided.                                |
| [`strftime`](strftime) (`format_time`)                                 | Converts a datetime (date) object to a human-readable string.                                                        |
| [`strptime`](strptime) (`parse_time`)                                  | Converts a string to a datetime.                                                                                     |
| [`todatetime`](todatetime)                                             | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`totimespan`](totimespan)                                             | Converts the input into a number of seconds.                                                                            |
| [`unixtime_microseconds_todatetime`](unixtime-microseconds-todatetime) | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime_milliseconds_todatetime`](unixtime-milliseconds-todatetime) | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime-nanoseconds_todatetime`](unixtime-nanoseconds-todatetime)   | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`unixtime_seconds_todatetime`](unixtime-seconds-todatetime)           | Converts the input into a datetime value (Unix time in seconds).                                                     |
| [`week_of_year`](week-of-year)                                         | Returns an integer representing the week number.                                                                     |


### Dynamic Functions {#dynamic}

Dynamic scalar functions allow you to manipulate objects by operating on [`dynamic` values](dynamic), including dynamic arrays
and property bags.


| Name                                   | Description                                                                     |
| -------------------------------------- | ------------------------------------------------------------------------------- |
| [`bag_has_key`](bag-has-key)           | Checks whether a property bag contains a given key.                             |
| [`bag_keys`](bag-keys)                 | Lists all root keys of a property bag.                                          |
| [`bag_merge`](bag-merge)               | Merges multiple property bags, discarding duplicate keys.                       |
| [`bag_pack`](bag-pack)                 | Creates a property bag from an alternating list of keys and values.             |
| [`bag_pack_columns`](bag-pack-columns) | Creates a property bag from a list of columns.                                  |
| [`bag_remove_keys`](bag-remove-keys)   | Removes key-value pairs from a property bag.                                    |
| [`bag_set_key`](bag-set-key)           | Adds or overwrites a key-value pair in a property bag.                          |
| [`bag_zip`](bag-zip)                   | Creates a property bag from two dynamic arrays.                                 |
| [`make_bag`](make-bag)                 | Creates a property bag from multiple input bags.                                |
| [`make_bag_if`](make-bag-if)           | Creates a property bag from those input bags that meet the specified condition. |
| [`zip`](zip)                           | Merges dynamic arrays, grouping elements by index.                              |

The following [string functions](string-functions) support `dynamic` types as well:

| Function                                         | Usage with dynamic data types              |
| ------------------------------------------------ | ------------------------------------------ |
| [`` extractjson(`path,object`) ``](extract-json) | Uses path to navigate into object.         |
| [`` parse_json(`source`) ``](parse-json)         | Turns a JSON string into a dynamic object. |
| [`` range(`from,to,step`) ``](range)             | Generates an array of values.              |


### Cryptographic Functions {#cryptographic}


| Name                 | Description                                                      |
| -------------------- | ---------------------------------------------------------------- |
| [`encrypt`](encrypt) | Encrypts data with a key managed by a Cribl Stream Worker Group. |
| [`decrypt`](decrypt) | Decrypts data with a key managed by a Cribl Stream Worker Group. |


### Hash Functions {#hash}


| Name                             | Description                                       |
| -------------------------------- | ------------------------------------------------- |
| [`hash`](hash)                   | Returns a hash value for the input value.         |
| [`hash_combine`](hash-combine)   | Combines hash values of two or more hashes.       |
| [`hash_many`](hash-many)         | Returns a combined hash value of multiple values. |
| [`hash_md5`](hash-md5)           | Returns an MD5 hash value for the input value.    |
| [`hash_sha1`](hash-sha1)         | Returns a SHA1 hash value for the input value.    |
| [`hash_sha256`](hash-sha256)     | Returns a SHA-256 hash value for the input value. |
| [`hash_xxhash64`](hash-xxhash64) | Returns a 64-bit hash value for the input value.  |


### INET Functions {#inet}


| Name                                           | Description                                                                                |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------ |
| [`ipv4_compare`](ipv4-compare)                 | Compares two IPv4 strings.                                                                 |
| [`ipv4_is_in_range`](ipv4-is-in-range)         | Checks if IPv4 string address is in IPv4-prefix notation range.                            |
| [`ipv4_is_in_any_range`](ipv4-is-in-any-range) | Checks whether IPv4 string address is in any of the specified IPv4 address ranges.         |
| [`ipv4_is_match`](ipv4-is-match)               | Matches two IPv4 strings.                                                                  |
| [`ipv4_is_private`](ipv4-is-private)           | Checks if IPv4 string address belongs to a set of private network IPs.                     |
| [`ipv4_netmask_suffix`](ipv4-netmask-suffix)   | Returns the value of the IPv4 netmask suffix from IPv4 string address.                     |
| [`ipv6_compare`](ipv6-compare)                 | Compares two IPv6 or IPv4 network address strings.                                         |
| [`ipv6_is_match`](ipv6-is-match)               | Matches two IPv6 or IPv4 network address strings.                                          |
| [`format_bytes`](format-bytes)                 | Converts the input into a string that represents data size.                                |
| [`format_ipv4`](format-ipv4)                   | Parses input with a netmask and returns string representing IPv4 address.                  |
| [`format_ipv4_mask`](format-ipv4-mask)         | Parses input with a netmask and returns string representing IPv4 address as CIDR notation. |


### Mathematical Functions {#mathematical}


| Name                   | Description                                                                                                    |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- |
| [`abs`](abs)           | Calculates the absolute value of the input.                                                                    |
| [`acos`](acos)         | Calculates the angle whose cosine is the specified number.                                                     |
| [`asin`](asin)         | Calculates the angle whose sine is the specified number.                                                       |
| [`atan`](atan)         | Returns the angle whose tangent is the specified number.                                      |
| [`atan2`](atan2)       | Calculates the angle, in radians, between the positive x-axis and the ray from the origin to the point (y, x). |
| [`beta_cdf`](beta-cdf) | Returns the standard cumulative beta distribution function.                                                    |
| [`beta_inv`](beta-inv) | Returns the inverse of the beta cumulative probability beta density function.                                  |
| [`beta_pdf`](beta-pdf) | Returns the probability density beta function.                                                                 |
| [`ceil`](ceil)   | Rounds up a numeric expression's value to the nearest integer.  |
| [`ceiling`](ceiling)   | Rounds up a numeric expression's value to the nearest integer.  |
| [`cos`](cos)           | Returns the cosine function.                                                                                   |
| [`cot`](cot)           | Calculates the trigonometric cotangent of the specified angle, in radians.                                     |
| [`degrees`](degrees)   | Converts angle value in radians into value in degrees.                                                         |
| [`exp`](exp)           | Calculates the base-e exponential function of x.                                                               |
| [`exp2`](exp2)         | Calculates the base-2 exponential function of x.                                                                |
| [`exp10`](exp10)       | Calculates the base-10 exponential function of x.                                                               |
| [`gamma`](gamma)       | Computes gamma function.                                                                                       |
| [`isfinite`](isfinite) | Returns whether input is a finite value.                                                                       |
| [`isinf`](isinf)       | Returns whether input is an infinite value.                                                                    |
| [`isnan`](isnan)       | Returns whether input is Not-a-Number (NaN) value.                                                             |
| [`log`](log)           | Returns the natural logarithm function.                                                                        |
| [`log2`](log2)         | Returns the (base-2) logarithm function.                                                                       |
| [`log10`](log10)       | Returns the common (base-10) logarithm function.                                                               |
| [`loggamma`](loggamma) | Computes log of absolute value of the loggamma function.                                                       |
| [`not`](not)           | Reverses the value of its boolean argument.                                                                    |
| [`pi`](pi)             | Returns the constant value of Pi.                                                                              |
| [`pow`](pow)           | Returns a result of raising to power.                                                                          |
| [`radians`](radians)   | Converts angle value in degrees into value in radians.           |
| [`rand`](rand)         | Returns a random number.                                                                                       |
| [`range`](range)       | Generates a dynamic array, holding a series of equally spaced values.                                          |
| [`round`](round)       | Returns the rounded source to the specified precision.                                                         |
| [`sign`](sign)         | Returns the sign of a numeric expression.                                                                      |
| [`sin`](sin)           | Returns the sine of a numeric expression.                                                                      |
| [`sqrt`](sqrt)         | Returns the square root function.                                                                              |
| [`tan`](tan)           | Returns the tangent function.                                                                                  |


### String Functions {#string}


| Name                                                 | Description                                                                                                             |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| [`base64_decode_toarray`](base64-decode-toarray)     | Decodes a base64 string to an array of long values.                                                                     |
| [`base64_decode_tostring`](base64-decode-tostring)   | Decodes a base64 string to a UTF-8 string.                                                                              |
| [`base64_encode_fromarray`](base64-encode-fromarray) | Encodes a base64 string from a bytes array.                                                                             |
| [`base64_encode_tostring`](base64-encode-tostring)   | Encodes a string as base64 string.                                                                                      |
| [`countof`](countof)                                 | Counts occurrences of a substring in a string.                                                                          |
| [`extract`](extract)                                 | Gets a match for an RE2 regular expression from a source string.                                                        |
| [`extract_all`](extract-all)                         | Gets all matches for an RE2 regular expression from a source string.                                                    |
| [`extract_json`](extract-json)                       | Gets a specified element out of a JSON text using a path expression.                                                    |
| [`has_any_index`](has-any-index)                     | Gets a match for an RE2 regular expression from a source string.                                                        |
| [`indexof`](indexof)                                 | Reports the zero-based index of the first occurrence of a specified string within the input string.                     |
| [`isempty`](isempty)                                 | Returns `true` if the argument is an empty string or is null.                                                           |
| [`isnotempty`](isnotempty) (`notempty`)              | Returns `true` if the argument isn’t an empty string, and it isn’t null.                                                |
| [`isnotnull`](isnotnull) (`notnull`)                 | Returns `true` if the argument is not null.                                                                             |
| [`isnull`](isnull)                                   | Indicates whether the argument evaluates to a null value.                                                               |
| [`match_regex`](match_regex)                         | Searches a text string for a specific pattern defined by a regular expression.                                          |
| [`parse_csv`](parse-csv)                             | Splits a given string representing a single record of comma-separated values.                                           |
| [`parse_ipv4`](parse-ipv4)                           | Converts IPv4 string to long (signed 64-bit) number representation in big-endian order.                                 |
| [`parse_ipv4_mask`](parse-ipv4-mask)                 | Converts the input string of IPv4 and netmask to a signed, 64-bit wide, long number representation in big-endian order. |
| [`parse_ipv6`](parse-ipv6)                           | Converts IPv6 or IPv4 string to a canonical IPv6 string representation.                                                 |
| [`parse_ipv6_mask`](parse-ipv6-mask)                 | Converts IPv6/IPv4 string and netmask to a canonical IPv6 string representation.                                        |
| [`parse_json`](parse-json) (`todynamic`)             | Interprets a string as a JSON value and returns the value as dynamic.                                                   |
| [`parse_url`](parse-url)                             | Parses an absolute URL string and returns a dynamic object that contains URL parts.                                     |
| [`parse_urlquery`](parse-urlquery)                   | Returns a dynamic object that contains the Query parameters.                                                            |
| [`parse_version`](parse-version)                     | Converts the input string representation of version to a comparable decimal number.                                     |
| [`replace_regex`](replace-regex)                     | Replaces all RE2 regular expression matches with another string.                                                        |
| [`reverse`](reverse)                                 | Reverses the order of the input string.                                                                                 |
| [`split`](split)                                     | Splits a given string according to a given delimiter.                                                                   |
| [`strcat`](strcat)                                   | Concatenates between 1 and 64 arguments.                                                                                |
| [`strcat_delim`](strcat-delim)                       | Concatenates between 2 and 64 arguments, with a delimiter.                                                              |
| [`strcmp`](strcmp)                                   | Compares two strings.                                                                                                   |
| [`strlen`](strlen)                                   | Returns the length, in characters, of the input string.                                                                 |
| [`strrep`](strrep)                                   | Repeats given string specified number of times.                                                                         |
| [`substring`](substring)                             | Extracts a substring from a source string starting from some index to the end of the string.                            |
| [`tolower`](tolower)                                 | Converts a string to lower case.                                                                                        |
| [`toupper`](toupper)                                 | Converts a string to upper case.                                                                                        |
| [`translate`](translate)                             | Replaces a set of characters with another set of characters in a given string.                                          |
| [`trim`](trim)                                       | Removes all leading and trailing matches of the specified string or regular expression.                                 |
| [`trim_end`](trim-end)                               | Removes trailing match of the specified regular expression.                                                             |
| [`trim_start`](trim-start)                           | Removes leading match of the specified regular expression.                                                              |
| [`url_decode`](url-decode)                           | Converts encoded URL into a to regular URL representation.                                                              |
| [`url_encode`](url-encode)                           | Converts characters of the input URL into a format that can be transmitted over the Internet.                           |


## Statistical Functions


| Name                       | Description                                                                                                   |
| -------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [`avg`](avg)               | Calculates the average across the group.                                                                      |
| [`avgif`](avgif)           | Calculates the average across the group where a predicate evaluates to `true`.                                |
| [`count`](count)           | Counts events per summarization group.                                                                        |
| [`countif`](countif)       | Counts events based on a predicate.                                             |
| [`dcount`](dcount)         | Calculates an estimate of the number of distinct values.                                                      |
| [`dcountif`](dcountif)     | Calculates an estimate of the number of distinct values for those rows where a predicate evaluates to `true`. |
| [`max`](max)               | Finds the maximum value across the group.                                                                     |
| [`maxif`](maxif)           | Finds the maximum value for which a predicate evaluates to `true`.                                            |
| [`min`](min)               | Finds the minimum value across the group.                                                                     |
| [`minif`](minif)           | Finds the minimum value which a predicate evaluates to `true`.                                                |
| [`percentile`](percentile) | Returns an estimate for the specified nearest-rank percentile of the population defined.                      |
| [`stdev`](stdev)           | Calculates the standard deviation of an expression across the group.                                          |
| [`stdevif`](stdevif)       | Calculates the standard deviation of an expression which a predicate evaluates to `true`.                     |
| [`stdevp`](stdevp)         | Calculates the standard deviation of an expression across the group, considering the group as a population.   |
| [`sum`](sum)               | Calculates the sum of an expression across the group.                                                         |
| [`sumif`](sumif)           | Calculates the sum of an expression for which a predicate evaluates to `true`.                                |
| [`variance`](variance)     | Calculates the variance of an expression.                                                                     |
| [`varianceif`](varianceif) | Calculates the variance of an expression for which a predicate evaluates to `true`.                           |
| [`variancep`](variancep)   | Calculates the variance of an expression across the group, considering the group as a population.             |


## Window Functions


| Name                                       | Description                                                                                     |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------- |
| [`next`](next)                             | Returns the value of a specific field in a subsequent row.                                      |
| [`prev`](prev)                             | Returns the value of a specific field in a previous row.                                        |
| [`row_cumsum`](row_cumsum)                 | Calculates the cumulative sum for a specified field across all previous rows.                   |
| [`row_number`](row_number)                 | Assigns a unique row number to each row within the results.                                     |
| [`row_rank_dense`](row_rank_dense)         | Assigns a unique numerical position (rank) to each row within the results                       |
| [`row_rank_min`](row_rank_min)             | Assigns a minimal numerical position (rank) to each row within the results                      |
| [`row_window_session`](row_window_session) | Identifies the value at the beginning of each session for a specified field within the results. |

