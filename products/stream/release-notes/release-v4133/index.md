# Cribl Stream 4.13.3


This maintenance release of Cribl Stream includes bug fixes and important updates.

## Corrections

- **For customer-managed deployments only:** Some customers with complex environments have reported issues with the 4.13.1 release. Customer-managed Leaders and Workers now default to the AWS SDK for JavaScript v2.

- Fixed an issue where Amazon S3 Sources failed to resolve and use the correct regional endpoint for multi-Region buckets.

- Fixed an issue where, with AWS SDK v3, Cribl S3 and SQS Sources failed to initialize if the configured AWS Region did not exactly match the actual Region of the S3 bucket or SQS queue.

- Reverted the Worker Group **Number of metrics allowed** default setting from `100,000` back to `1,000,000`. This change addresses recent customer reports of Worker Node crashes related to metrics processing. This temporary fix provides stability for high-volume environments while we work on a more permanent solution for handling high-volume metrics.

See the [Cribl Stream 4.13.1](https://docs.cribl.io/stream/release-notes/release-v4131/) release notes for the latest feature and correction updates.
