# Cribl Lake 4.13.3


This maintenance release provides corrections for Cribl Stream and Cribl Edge interactions with Amazon Sources: (a) Customer-managed Leaders and Workers now default to the AWS SDK for JavaScript v2. (CRIBL-34784) (b) Corrected Amazon S3 Source endpoints for multi-Region buckets. (CRIBL-34740) (c)  Corrected Amazon S3 and SQS Source initialization with Region mismatches. (CRIBL-34759) Also, in Stream Worker Group Settings and Edge Fleet Settings, it rolls back the default **Number of metrics allowed** count from 100,000 to its earlier 1,000,000 value.

For the latest feature and correction updates to Cribl Lake, see the [4.13.1 release notes](release-v4131).
