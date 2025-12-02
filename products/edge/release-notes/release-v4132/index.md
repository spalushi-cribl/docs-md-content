# Cribl Edge 4.13.2


Cribl Edge v4.13.2 corrects three [critical issues](https://docs.cribl.io/results/?query=CRIBL-34737&searchOption=known-issues) found in Cribl Stream v4.13.1 and rolls back the AWS SDK to v2 for on-premises and hybrid deployments:

## Important Changes

For all on-premises and hybrid deployments, we have reverted the AWS SDK to v2. This change addresses the following [issues](https://docs.cribl.io/results/?query=&searchOption=known-issues&product=edge&reported-from=4.13.1) with AWS integrations that occurred following the upgrade to v3 in the 4.13.1 release.

## Corrections

- Uploads from the Amazon S3 Destination to buckets located in `us-east-1` or `eu-west-1` now succeed when a Region is not explicitly specified. This prevents the "Region is missing" and "failed to upload file" errors previously encountered in this scenario. (CRIBL-34732)

- The Amazon S3 Destination now correctly uploads data to S3 buckets with Object Lock enabled. Requests include the required `Content-MD5` or `x-amz-checksum-` header, which prevents the `InvalidRequest` error previously returned by AWS. (CRIBL-34737)

- Fixed an issue where AWS KMS-backed secrets failed to decrypt, causing `bad decrypt` errors. (CRIBL-34785)

See the [Cribl Edge 4.13.1](https://docs.cribl.io/edge/release-notes/release-v4131/) release notes for the latest feature and correction updates.
