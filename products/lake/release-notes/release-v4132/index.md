# Cribl Lake 4.13.2


This release corrects three [critical issues](https://docs.cribl.io/results/?query=CRIBL-34737&searchOption=known-issues) found in Cribl Stream and Edge v4.13.1: (a) Amazon S3 Destinations that do not specify a Region failed to write data to S3 buckets located in certain Regions. (CRIBL-34732) (b) Amazon S3 Destinations failed to write to S3 buckets that have Object Lock enabled. (CRIBL-34737) (c) Stream and Edge failed to decrypt AWS KMS-backed secrets, throwing `bad decrypt` errors. (CRIBL-34785)

See the [Cribl Lake 4.13.1](https://docs.cribl.io/lake/release-notes/release-v4131/) release notes for the latest feature and correction updates.