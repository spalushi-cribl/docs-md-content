# String Functions


A list of all string functions supported by Cribl Search.

---


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

