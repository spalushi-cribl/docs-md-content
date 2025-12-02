# Cribl Stream 4.5.1


## New Features​

This release provides the following improvements:

### Sources and Destinations

[Google Pub/Sub Source](sources-google_pubsub) can now subscribe to topics in a project on a different service account. Similarly, [Google Pub/Sub Destination](destinations-google_pubsub) can publish to topics in a different service.

[Amazon S3 Destination](destinations-s3) offers an additional storage class: Glacier Instant Retrieval. Other storage classes are renamed for clarity and to match the names used by S3: “Glacier” is now “Glacier Flexible Retrieval”, while “Deep Archive” changed to “Glacier Deep Archive”.

[Health Check Collector](collectors-health-check) now returns an elapsedMS attribute in `__collectStats`.

[Windows Event Forwarder Source](sources-wef) now contains Field settings. Additionally, you can configure fields per subscription.

[Splunk HEC](sources-splunk-hec), [HTTP/S](sources-https), and [Elasticsearch API](sources-elastic) Sources now log time-based metrics about incoming requests and ingested events.

In the [Raw HTTP/S Source](sources-raw-http) you can now add request headers to events, in the `__headers` field.

### Other New Features

New `C.Misc` UUID functions (`C.Misc.uuidv4`, `C.Misc.uuidv5`, `C.Misc.validateUUID`, `C.Misc.getUUIDVersion`) let you generate a UUID for each event and validate it.

## Corrections

This release includes the following fixes:

- Fixed a bug in the Raw HTTP Source where the `event.headers` field only replaced the first hyphen in a header name with an underscore instead of the intended behavior of replacing all hyphens with underscores. CRIBL-23106

- Special characters `<` `>` `"` `` ` `` `\r` `\n` `\t` `{` `}` `|` `\` `^` `'` in authToken when managed Workers are being deployed using the `CRIBL_DIST_MASTER_URL` (for example, in containers) will now correctly throw an error. Such characters can’t be used in the token. CRIBL-17047

- Corrected a memory leak on Workers. CRIBL-22804

### Source and Destination Fixes

- Cribl TCP, Cribl HTTP, and TCP JSON Sources and Destinations now respond with more meaningful messages in case of connection problems. CRIBL-16537

- Syslog Source now correctly respects the Default timezone setting. CRIBL-23031

- Cribl TCP, TCP JSON, and Appscope Sources now log stats once per minute to the `metrics.log` file. CRIBL-21742

- Fixed an issue with the Google Pub/Sub Source which caused it to run out of memory. CRIBL-20424

- Kafka Destination now outputs a timestamp field based on `__kafkaTime`. If `__kafkaTime` is not present, the timestamp defaults to `Date.now()`. CRIBL-22966
