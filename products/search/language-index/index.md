# Language Reference Index


An alphabetical index of all Cribl Search KQL elements.

---

This page provides a global, alphabetical index of all elements of the Cribl Search KQL language implementation: operators, functions, commands, statements, virtual tables, and supported types. 

Mathematical symbols for equality and inequality operators precede letters. Each building block is linked to its reference page and category.

| Operator, Function, Element | Category |
| ----------------- | ---- |
| [==](equals-cs) (equals, case-sensitive) | [Operator](operators) |
| [=~](equals)  (equals, case-insensitive) | [Operator](operators) |
| [!=](not-equals-cs) (not equals, case-sensitive) | [Operator](operators) |
| [!~](not-equals) (not equals, case-insensitive) | [Operator](operators) |
| [abs](abs) | [Mathematical function](mathematical-functions) |
| [acos](acos) | [Mathematical function](mathematical-functions) |
| [ago](ago) | [DateTime function](datetime-functions) |
| [asin](asin) | [Mathematical function](mathematical-functions) |
| [atan](atan) | [Mathematical function](mathematical-functions) |
| [atan2](atan2) | [Mathematical function](mathematical-functions) |
| [avg](avg) | [Statistical function](statistical-functions) |
| [avgif](avgif) | [Statistical function](statistical-functions) |
| [bag_has_key](bag-has-key) | [Dynamic function](dynamic-functions) |
| [bag_keys](bag-keys) | [Dynamic function](dynamic-functions) |
| [bag_merge](bag-merge) | [Dynamic function](dynamic-functions) |
| [bag_pack](bag-pack) | [Dynamic function](dynamic-functions) |
| [bag_pack_columns](bag-pack-columns) | [Dynamic function](dynamic-functions) |
| [bag_remove_keys](bag-remove-keys) | [Dynamic function](dynamic-functions) |
| [bag_set_key](bag-set-key) | [Dynamic function](dynamic-functions) |
| [bag_zip](bag-zip) | [Dynamic function](dynamic-functions) |
| [base64_decode_toarray](base64-decode-toarray) | [String function](string-functions) |
| [base64_decode_tostring](base64-decode-tostring) | [String function](string-functions) |
| [base64_encode_fromarray](base64-encode-fromarray) | [String function](string-functions) |
| [base64_encode_tostring](base64-encode-tostring) | [String function](string-functions) |
| [beta_cdf](beta-cdf) | [Mathematical function](mathematical-functions) |
| [beta_inv](beta-inv) | [Mathematical function](mathematical-functions) |
| [beta_pdf](beta-pdf) | [Mathematical function](mathematical-functions) |
| [bin](bin) | [Conversion function](conversion-functions) |
| [bin_auto](bin-auto) | [Conversion function](conversion-functions) |
| [binary_and](binary-and) | [Binary function](binary-functions) |
| [binary_not](binary-not) | [Binary function](binary-functions) |
| [binary_or](binary-or) | [Binary function](binary-functions) |
| [binary_shift_left](binary-shift-left) | [Binary function](binary-functions) |
| [binary_shift_right](binary-shift-right) | [Binary function](binary-functions) |
| [binary_xor](binary-xor) | [Binary function](binary-functions) |
| [bool](bool) | [Type](types) |
| [.cancel](cancel) | [Command](commands) |
| [case](case) | [Conditional function](conditional-functions) |
| [ceil](ceil) | [Mathematical function](mathematical-functions) |
| [ceiling](ceiling) | [Mathematical function](mathematical-functions) |
| [.clear options](clear-options) | [Command](commands) |
| [centralize](centralize) | [Operator](operators) |
| [coalesce](coalesce) | [Conditional function](conditional-functions) |
| [contains](contains) | [Operator](operators) |
| [contains_cs](contains-cs) | [Operator](operators) |
| [!contains](not-contains) | [Operator](operators) |
| [!contains_cs](not-contains-cs) | [Operator](operators) |
| [cos](cos) | [Mathematical function](mathematical-functions) |
| [cot](cot) | [Mathematical function](mathematical-functions) |
| [count](operators-count) | [Operator](operators) |
| [count](count) | [Statistical function](statistical-functions) |
| [countif](countif) | [Statistical function](statistical-functions) |
| [countof](countof) | [String function](string-functions) |
| [cribl](cribl) | [Operator](operators) |
| [datetime](datetime) | [Type](types) |
| [datetime_add](datetime-add) | [DateTime function](datetime-functions) |
| [datetime_diff](datetime-diff) | [DateTime function](datetime-functions) |
| [datetime_part](datetime-part) | [DateTime function](datetime-functions) |
| [dayofmonth](dayofmonth) | [DateTime function](datetime-functions) |
| [dayofweek](dayofweek) | [DateTime function](datetime-functions) |
| [dayofyear](dayofyear) | [DateTime function](datetime-functions) |
| [dcount](dcount) | [Statistical function](statistical-functions) |
| [dcountif](dcountif) | [Statistical function](statistical-functions) |
| [decimal](decimal) | [Type](types) |
| [decrypt](decrypt) | [Scalar function](scalar-functions) |
| [dedup](dedup) | [Operator](operators) |
| [degrees](degrees) | [Mathematical function](mathematical-functions) |
| [distinct](distinct) | [Operator](operators) |
| [double](double) | [Type](types) |
| [dynamic](dynamic) | [Type](types) |
| [encrypt](encrypt) | [Scalar function](scalar-functions) |
| [endofday](endofday) | [DateTime function](datetime-functions) |
| [endofmonth](endofmonth) | [DateTime function](datetime-functions) |
| [endofweek](endofweek) | [DateTime function](datetime-functions) |
| [endofyear](endofyear) | [DateTime function](datetime-functions) |
| [endswith](endswith) | [Operator](operators) |
| [!endswith](not-endswith) | [Operator](operators) |
| [endswith_cs](endswith-cs) | [Operator](operators) |
| [!endswith_cs](not-endswith-cs) | [Operator](operators) |
| [eventstats](eventstats) | [Operator](operators) |
| [exp](exp) | [Mathematical function](mathematical-functions) |
| [exp10](exp10) | [Mathematical function](mathematical-functions) |
| [exp2](exp2) | [Mathematical function](mathematical-functions) |
| [export](export) | [Operator](operators) |
| [extend](extend) | [Operator](operators) |
| [externaldata](externaldata) | [Operator](operators) |
| [extract](extract) | [String function](string-functions) |
| [extract](extract-operator) | [Operator](operators) |
| [extract_all](extract-all) | [String function](string-functions) |
| [extract_json](extract-json) | [String function](string-functions) |
| [find](find) | [Operator](operators) |
| [findearliest](findearliest) | [Cribl function](cribl-functions) |
| [findearliestif](findearliestif) | [Cribl function](cribl-functions) |
| [findfirst](findfirst) | [Cribl function](cribl-functions) |
| [findfirstif](findfirstif) | [Cribl function](cribl-functions) |
| [findlast](findlast) | [Cribl function](cribl-functions) |
| [findlastif](findlastif) | [Cribl function](cribl-functions) |
| [findlatest](findlatest) | [Cribl function](cribl-functions) |
| [findlatestif](findlatestif) | [Cribl function](cribl-functions) |
| [floor](floor) | [Conversion function](conversion-functions) |
| [foldkeys](foldkeys) | [Operator](operators) |
| [format_bytes](format-bytes) | [INET function](inet-functions) |
| [format_datetime](format-datetime) | [DateTime function](datetime-functions) |
| [format_ipv4](format-ipv4) | [INET function](inet-functions) |
| [format_ipv4_mask](format-ipv4-mask) | [INET function](inet-functions) |
| [format_timespan](format-timespan) | [DateTime function](datetime-functions) |
| [from_binary_string](from-binary-string) | [Binary function](binary-functions) |
| [gamma](gamma) | [Mathematical function](mathematical-functions) |
| [.generate stats](generate-stats) | [Command](commands) |
| [getmonth](getmonth) | [DateTime function](datetime-functions) |
| [gettype](gettype) | [Conversion function](conversion-functions) |
| [getyear](getyear) | [DateTime function](datetime-functions) |
| [has](has) | [Operator](operators) |
| [!has](not-has) | [Operator](operators) |
| [has_all](has-all) | [Operator](operators) |
| [!has_all](not-has-all) | [Operator](operators) |
| [has_any](has-any) | [Operator](operators) |
| [!has_any](not-has-any) | [Operator](operators) |
| [has_any_index](has-any-index) | [String function](string-functions) |
| [has_cs](has-cs) | [Operator](operators) |
| [!has_cs](httpsQL://docs.cribl.io/search/not-has-cs) | [Operator](operators) |
| [!hasprefix](not-hasprefix) | [Operator](operators) |
| [!hasprefix_cs](not-hasprefix-cs) | [Operator](operators) |
| [!hassuffix](not-hassuffix) | [Operator](operators) |
| [!hassuffix_cs](not-hassuffix-cs) | [Operator](operators) |
| [hash](hash) | [Hash function](hash-functions) |
| [hash_combine](hash-combine) | [Hash function](hash-functions) |
| [hash_many](hash-many) | [Hash function](hash-functions) |
| [hash_md5](hash-md5) | [Hash function](hash-functions) |
| [hash_sha1](hash-sha1) | [Hash function](hash-functions) |
| [hash_sha256](hash-sha256) | [Hash function](hash-functions) |
| [hash_xxhash64](hash-xxhash64) | [Hash function](hash-functions) |
| [hasprefix](hasprefix) | [Operator](operators) |
| [hasprefix_cs](hasprefix-cs) | [Operator](operators) |
| [hassuffix](hassuffix) | [Operator](operators) |
| [hassuffix_cs](hassuffix-cs) | [Operator](operators) |
| [hourofday](hourofday) | [DateTime function](datetime-functions) |
| [iff](iff) | [Conditional function](conditional-functions) |
| [iif](iif) | [Conditional function](conditional-functions) |
| [in](in-cs) | [Operator](operators) |
| [indexof](indexof) | [String function](string-functions) |
| [int](int) | [Type](types) |
| [in~](in) | [Operator](operators) |
| [!in](not-in-cs) | [Operator](operators) |
| [!in~](not-in) | [Operator](operators) |
| [ip-lookup](ip-lookup) | [Operator](operators) |
| [ipv4_compare](ipv4-compare) | [INET function](inet-functions) |
| [ipv4_is_in_any_range](ipv4-is-in-any-range) | [INET function](inet-functions) |
| [ipv4_is_in_range](ipv4-is-in-range) | [INET function](inet-functions) |
| [ipv4_is_match](ipv4-is-match) | [INET function](inet-functions) |
| [ipv4_is_private](ipv4-is-private) | [INET function](inet-functions) |
| [ipv4_netmask_suffix](ipv4-netmask-suffix) | [INET function](inet-functions) |
| [ipv6_compare](ipv6-compare) | [INET function](inet-functions) |
| [ipv6_is_match](ipv6-is-match) | [INET function](inet-functions) |
| [isempty](isempty) | [String function](string-functions) |
| [isfinite](isfinite) | [Mathematical function](mathematical-functions) |
| [isinf](isinf) | [Mathematical function](mathematical-functions) |
| [isnan](isnan) | [Mathematical function](mathematical-functions) |
| [isnotempty](isnotempty) | [String function](string-functions) |
| [isnotnull](isnotnull) | [String function](string-functions) |
| [isnull](isnull) | [String function](string-functions) |
| [join](join) | [Operator](operators) |
| [let](let) | [Statement](statements) |
| [limit](limit) | [Operator](operators) |
| [list](list) | [Cribl function](cribl-functions) |
| [log](log) | [Mathematical function](mathematical-functions) |
| [log10](log10) | [Mathematical function](mathematical-functions) |
| [log2](log2) | [Mathematical function](mathematical-functions) |
| [loggamma](loggamma) | [Mathematical function](mathematical-functions) |
| [long](long) | [Type](types) |
| [lookup](lookup) | [Operator](operators) |
| [make_bag](make-bag) | [Dynamic function](dynamic-functions) |
| [make_bag_if](make-bag-if) | [Dynamic function](dynamic-functions) |
| [make_datetime](make-datetime) | [DateTime function](datetime-functions) |
| [make_timespan](make-timespan) | [DateTime function](datetime-functions) |
| [match_regex](match_regex) | [String function](string-functions) |
| [matches regex](matches-regex) | [Operator](operators) |
| [max](max) | [Statistical function](statistical-functions) |
| [max_of](max-of) | [Conditional function](conditional-functions) |
| [maxif](maxif) | [Statistical function](statistical-functions) |
| [median](median) | [Cribl function](cribl-functions) |
| [medianif](medianif) | [Cribl function](cribl-functions) |
| [min](min) | [Statistical function](statistical-functions) |
| [min_of](min-of) | [Conditional function](conditional-functions) |
| [minif](minif) | [Statistical function](statistical-functions) |
| [monthofyear](monthofyear) | [DateTime function](datetime-functions) |
| [mv-expand](mv-expand) | [Operator](operators) |
| [mv-pull](mv-pull) | [Operator](operators) |
| [next](next) | [Window function](window-functions) |
| [not](not) | [Mathematical function](mathematical-functions) |
| [now](now) | [DateTime function](datetime-functions) |
| [null](null) | [Type](types) |
| [order](order) | [Operator](operators) |
| [parse_csv](parse-csv) | [String function](string-functions) |
| [parse_ipv4](parse-ipv4) | [String function](string-functions) |
| [parse_ipv4_mask](parse-ipv4-mask) | [String function](string-functions) |
| [parse_ipv6](parse-ipv6) | [String function](string-functions) |
| [parse_ipv6_mask](parse-ipv6-mask) | [String function](string-functions) |
| [parse_json](parse-json) | [String function](string-functions) |
| [parse_url](parse-url) | [String function](string-functions) |
| [parse_urlquery](parse-urlquery) | [String function](string-functions) |
| [parse_version](parse-version) | [String function](string-functions) |
| [percentile](percentile) | [Statistical function](statistical-functions) |
| [persecond](persecond) | [Cribl function](cribl-functions) |
| [persecondif](persecondif) | [Cribl function](cribl-functions) |
| [pi](pi) | [Mathematical function](mathematical-functions) |
| [pivot](pivot) | [Operator](operators) |
| [pow](pow) | [Mathematical function](mathematical-functions) |
| [prev](prev) | [Window function](window-functions) |
| [print](print) | [Operator](operators) |
| [project](project) | [Operator](operators) |
| [project-away](project-away) | [Operator](operators) |
| [project-rename](project-rename) | [Operator](operators) |
| [radians](radians) | [Mathematical function](mathematical-functions) |
| [rand](rand) | [Mathematical function](mathematical-functions) |
| [range](range-operator) | [Operator](operators) |
| [range](range) | [Mathematical function](mathematical-functions) |
| [rate](rate) | [Cribl function](cribl-functions) |
| [rateif](rateif) | [Cribl function](cribl-functions) |
| [real](real) | [Type](types) |
| [render](render) | [Operator](operators) |
| [replace_regex](replace-regex) | [String function](string-functions) |
| [reverse](reverse) | [String function](string-functions) |
| [round](round) | [Mathematical function](mathematical-functions) |
| [row_cumsum](row_cumsum) | [Window function](window-functions) |
| [row_number](row_number) | [Window function](window-functions) |
| [row_rank_dense](row_rank_dense) | [Window function](window-functions) |
| [row_rank_min](row_rank_min) | [Window function](window-functions) |
| [row_window_session](row_window_session) | [Window function](window-functions) |
| [search](search) | [Operator](operators) |
| [send](send) | [Operator](operators) |
| [set](set) | [Statement](statements) |
| [.show objects](show-objects) | [Command](commands) |
| [.show options](show-options) | [Command](commands) |
| [.show queries](show-queries) | [Command](commands) |
| [sign](sign) | [Mathematical function](mathematical-functions) |
| [sin](sin) | [Mathematical function](mathematical-functions) |
| [sort](sort) | [Operator](operators) |
| [split](split) | [String function](string-functions) |
| [sqrt](sqrt) | [Mathematical function](mathematical-functions) |
| [startofday](startofday) | [DateTime function](datetime-functions) |
| [startofmonth](startofmonth) | [DateTime function](datetime-functions) |
| [startofweek](startofweek) | [DateTime function](datetime-functions) |
| [startofyear](startofyear) | [DateTime function](datetime-functions) |
| [startswith](startswith) | [Operator](operators) |
| [!startswith](not-startswith) | [Operator](operators) |
| [startswith_cs](startswith-cs) | [Operator](operators) |
| [!startswith_cs](not-startswith-cs) | [Operator](operators) |
| [stdev](stdev) | [Statistical function](statistical-functions) |
| [stdevif](stdevif) | [Statistical function](statistical-functions) |
| [stdevp](stdevp) | [Statistical function](statistical-functions) |
| [strcat](strcat) | [String function](string-functions) |
| [strcat_delim](strcat-delim) | [String function](string-functions) |
| [strcmp](strcmp) | [String function](string-functions) |
| [strftime](strftime) | [DateTime function](datetime-functions) |
| [string](string) | [Type](types) |
| [strlen](strlen) | [String function](string-functions) |
| [strptime](strptime) | [DateTime function](datetime-functions) |
| [strrep](strrep) | [String function](string-functions) |
| [substring](substring) | [String function](string-functions) |
| [sum](sum) | [Statistical function](statistical-functions) |
| [sumif](sumif) | [Statistical function](statistical-functions) |
| [summarize](summarize) | [Operator](operators) |
| [sumsq](sumsq) | [Cribl function](cribl-functions) |
| [sumsqif](sumsqif) | [Cribl function](cribl-functions) |
| [take](take) | [Operator](operators) |
| [tan](tan) | [Mathematical function](mathematical-functions) |
| [timespan](timespan) | [Type](types) |
| [timestats](timestats) | [Operator](operators) |
| [to_binary_string](to-binary-string) | [Binary function](binary-functions) |
| [tobool](tobool) | [Conversion function](conversion-functions) |
| [todatetime](todatetime) | [DateTime function](datetime-functions) |
| [todecimal](todecimal) | [Conversion function](conversion-functions) |
| [todouble](todouble) | [Conversion function](conversion-functions) |
| [toint](toint) | [Conversion function](conversion-functions) |
| [tolong](tolong) | [Conversion function](conversion-functions) |
| [tolower](tolower) | [String function](string-functions) |
| [top](top) | [Operator](operators) |
| [top-hitters](top-hitters) | [Operator](operators) |
| [toreal](toreal) | [Conversion function](conversion-functions) |
| [tostring](tostring) | [Conversion function](conversion-functions) |
| [totimespan](totimespan) | [DateTime function](datetime-functions) |
| [toupper](toupper) | [String function](string-functions) |
| [translate](translate) | [String function](string-functions) |
| [trim](trim) | [String function](string-functions) |
| [trim_end](trim-end) | [String function](string-functions) |
| [trim_start](trim-start) | [String function](string-functions) |
| [union](union) | [Operator](operators) |
| [unixtime_microseconds_todatetime](unixtime-microseconds-todatetime) | [DateTime function](datetime-functions) |
| [unixtime_milliseconds_todatetime](unixtime-milliseconds-todatetime) | [DateTime function](datetime-functions) |
| [unixtime_nanoseconds_todatetime](unixtime-nanoseconds-todatetime) | [DateTime function](datetime-functions) |
| [unixtime_seconds_todatetime](unixtime-seconds-todatetime) | [DateTime function](datetime-functions) |
| [url_decode](url-decode) | [String function](string-functions) |
| [url_encode](url-encode) | [String function](string-functions) |
| [values](values) | [Cribl function](cribl-functions) |
| [variance](variance) | [Statistical function](statistical-functions) |
| [varianceif](varianceif) | [Statistical function](statistical-functions) |
| [variancep](variancep) | [Statistical function](statistical-functions) |
| [$vt_dataset_providers](vt_dataset_providers) | [Virtual table](virtual-tables) |
| [$vt_datasets](vt_datasets) | [Virtual table](virtual-tables) |
| [$vt_dummy](vt_dummy) | [Virtual table](virtual-tables) |
| [$vt_jobs](vt_jobs) | [Virtual table](virtual-tables) |
| [$vt_list](vt_list) | [Virtual table](virtual-tables) |
| [$vt_lookups](vt_lookups) | [Virtual table](virtual-tables) |
| [$vt_results](vt_results) | [Virtual table](virtual-tables) |
| [week_of_year](week-of-year) | [DateTime function](datetime-functions) |
| [where](where) | [Operator](operators) |
| [zip](zip) | [Dynamic function](dynamic-functions) |
