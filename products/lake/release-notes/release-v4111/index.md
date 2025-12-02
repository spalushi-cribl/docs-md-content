# Cribl Lake 4.11.1


Cribl Lake 4.11.1, a maintenance release, includes an enhancement and bug fixes.

## Important Changes

### New Cribl.Cloud Domain Rollback

In our previous release, we announced a plan to begin migrating Cribl.Cloud Organizations to a new unified domain (`https://app.cribl.cloud/`). After evaluating customer feedback and internal considerations, we’ve decided **not to move forward with this domain migration**.

Your Cribl.Cloud environment will continue to operate under the existing URLs you’ve always used — there is no change to login or portal access. The previously communicated domain (`app.cribl.cloud`) will not be adopted, and no redirects will be implemented.

This rollback requires **no action on your part**. All existing functionality and URLs remain unchanged.

## New Features

This release includes the following enhancements.

### Maximum Retention Period Increased to 10 Years

Cribl Lake now supports retention periods of up to 10 years. 

## Corrections

This release includes the following fixes.

|ID|Description|
|--|-----------|
| SEARCH-9247 | Prevents Cribl Search coordinator time from being billed on Lakehouse-satisfied searches. |
| LAKE-729 | Lakehouses can now accept incoming events up to 1 MB in size, increased from 50 KB. |
