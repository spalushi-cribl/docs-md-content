# Common Issues and Resolutions


Diagnose and resolve common errors in working with Cribl Search.

---

This page lists errors and issues that you might commonly encounter when working with Cribl Search, and provides recommendations for diagnosing and resolving the problems.

## General Troubleshooting Tree {#general}

If you are getting unexpected results from a search against an object store or data lake, start by following this general troubleshooting path:

1. Are you retrieving objects? (Check this using the [`.show objects`](show-objects) command.)
1. If you've retrieved no objects, check bucket path(s), Search [Permissions](cloud-managing#search-member-roles) to access the Dataset and Dataset Provider, and access permissions on the object store.
1. If you are retrieving objects, search for raw events with no time filtering.
1. If you find no events, check the data format and permissions.
1. If you find events, check your original time filtering and time parsing – is time recognized accurately? Also check [Datatypes](datatypes) that contain [Event Breakers](datatypes#event-breaker-types).

## Amazon Connectivity Issues {#amazon}

This section covers issues specific to Amazon Datasets.

### S3 Returns No Elements {#no-elements-s3}

You might find no elements returned in response to your query.

##### Possible Cause

Check Search [Permissionss](cloud-managing#search-member-roles) to access the Dataset and Dataset Provider. Also check [Datatypes](datatypes) and [Event Breakers](datatypes#event-breaker-types).

##### Recommendations

Check contents ([ListBuckets](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBuckets.html) in S3) to see, and adjust, the Cribl user's or role's  permissions on the bucket.

### KMS Authorization Fails on S3 {#kms-aws}

If you are using a custom KMS (key management system), you might find that Cribl Search cannot authorize on S3.

##### Possible Cause

Cribl Search cannot access your keys.

##### Recommendations

Include `AssumeRole` permissions in the KMS console and in the [trust policy](aws-access#trust), to allow Cribl Search to access the keys.

### KMS Decrypt Errors on S3 {#kms-decrypt}

Retrieving results from objects in an S3 bucket fail, and internal logs show errors of the form:  
`User is not authorized to perform kms:Decrypt on resource`.

##### Possible Cause

Your IAM role is missing permissions required to access S3 objects that are encrypted with a KMS key.

##### Recommendations

Add this sub-element to the applicable IAM role:

```json
 "Action": [
     "kms:Decrypt",
     "kms:Describe*",
     "kms:Get*",
     "kms:List*",
     "kms:GenerateDataKey"
 ],
```

### Existing AssumeRole Fails to Authorize on S3 {#existing-assumerole}

You might find that your existing `AssumeRole` from Cribl Stream/Edge fails to authorize Cribl Search on S3.

##### Possible Cause

Cribl Search requires a different `Principal` element.

##### Recommendations

Add the required `Principal` to your [trust policy](aws-access#trust). (Follow the link to see how to copy this `Principal` out of your Dataset Provider modal.)

### Can't Add Buckets to Amazon Security Lake Dataset {#add-buckets}

You might find that you cannot add new buckets to your Dataset.

##### Possible Cause

Insufficient permissions on Amazon Security Lake.

##### Recommendations

Check whether you have added the `securitylake:ListDataLakes` permission to your `AssumeRole`.

## Performance Fixes {#performance}

This section covers workarounds for slow, inefficient, or failing searches.

### Scheduled Search Jobs Canceled Due to Queue Length Limit

You might find that some of your scheduled search jobs were canceled. because of the default queue length limit.

##### Possible Cause

At **Settings** > **Search**, the [limit](limits#list-of-configurable-system-limits) on **Length of the search job queue** defaults to `500%`. This value represents 5x the maximum number of concurrent queries, which you can think of as 5x CPU capacity.

##### Recommendations

To check the state of the queue, enter `.show queue stats` in the query box.

Try increasing the **Length of the search job queue** to `1500%` (15x maximum concurrent queries), or to a high absolute value like `1000`.


## Feature Workarounds {#features}

This section covers substitutes for features that Cribl Search doesn't provide out-of-the-box.

### Get the Length of an Array Field

Cribl Search currently has no native function to return the length of an array's field, without stringifying the value.

##### Recommendations

1. Strip everything from the array except for commas; apply the `strlen()` function; and then add 1 to the result. 

2. Try a query of the form: `print <arrayName> = dynamic([1,2,3]) | extend len = <arrayName>.length`


## Email Notifications Issues {#troubleshooting-email-notifications}

The section details the troubleshooting steps you can take if an [email Notification](email-notifications) fails to reach its intended recipient.

### Test the Email Notification Target {#test-target}

To prevent email Notifications from failing in production, you can check the target's configuration as follows:

1. Open the target (in **Settings** > **Search** > **Notification Targets**).
2. Select **Test Target**.
3. Add one or more email addresses to the **Test Target** modal. (Include at least one address where you can check incoming messages.)
4. Select **Send Test Email**.
5. If the test succeeds, check the specified inbox(es) to verify that the test message actually arrived. If not, re-examine the target's configuration.

> The preconfigured `system_email` target does not enable displaying this **Test Target** option or its parent modal.
>
{.box .info}

### Check Notification Logs {#check-email-logs}

A failed email Notification leaves a log entry. You can search Cribl internal logs for SMTP errors using a query of this form:

```kusto {runSearch=true}
dataset="cribl_internal_logs" level=error channel="output:*" ioName=smtp
```

Within any returned log events, unwrap the `reason` field to find details about the root cause of the outbound email failure.


