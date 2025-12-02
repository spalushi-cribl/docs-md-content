# Cribl Stream 4.10.1


This maintenance release of Cribl Stream includes bug fixes and experience improvements.

## Important Changes

### Deprecation Notice

Upcoming deprecation for all Elasticsearch API Sources: in the next release, these Sources will ignore extra HTTP headers not starting with `X-` or `x-`.

## New Features

This release provides the following improvements:

### `sensitiveFields` Configuration Property

You can now use the `sensitiveFields` configuration property to [filter sensitive information](securing-onprem#sensitivefields) in the responses to API requests for `/inputs` and `/outputs` endpoints.

## Experience Improvements

- Logs now contain information about the reason for reaping collection job artifacts. This information is contained in the `JobArtifactReaper` logging channel at debug level.

## Sources and Destinations

- Newly created or modified Elasticsearch API Sources will ignore extra HTTP headers that do not start with `X-` or `x-`.
- The Prometheus Remote Write Source has new parsing logic to increase data throughput performance by ~2.5x  and reduce CPU memory usage. To opt in to the new parser, add the following JSON configuration via **Manage as JSON**: `"prometheusParser": "v2"`.
- The Datadog Destination now includes an option to send `counter` metrics as `count`.

## Corrections

This release contains the following bug fixes:

### Operational Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-30329</div>| After a session timeout, logging back into Cribl.Cloud now restores focus to the last-viewed product page. |
| CRIBL-30037 | Fixed an error that prevented users from cloning built-in roles that ship with the product. |
| CRIBL-30302 | Resolved an issue where custom Collectors displayed the folder name instead of the custom name in the list of active Collectors. |

### Source and Destination Fixes

|ID|Description|
|--|-----------|
|<div style="width: 100px">CRIBL-30469</div>| We fixed an issue where the Azure Data Explorer Destination was unable to send data in GCC High Azure environments due to a pattern match error with GovCloud URIs. The URI validation has been updated to allow Azure GCC High URIs, ensuring seamless data transmission to Azure Data Explorer for you in GCC High environments. |
| CRIBL-29845 | The Cribl HTTP Destination now correctly displays TLS status based on its configuration, not the URL scheme.  This resolves an issue where the **TLS** column incorrectly reflected the http/https URL. |
| CRIBL-30511 | Fixed an issue with Parquet autoschema generation where fields containing a mix of JSON and string types were incorrectly identified as STRING. |
