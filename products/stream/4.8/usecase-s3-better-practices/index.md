# Amazon S3 Better Practices


The "better practices" in this guide all apply to Cribl Stream's Amazon S3-based Sources and Destinations. Many also apply to other object stores, like [Azure Blob Storage](destinations-azure-blob), [MinIO](destinations-minio), and [Google Cloud Storage](destinations-google-cloud-storage).

> View the [AWS S3 with Cribl Best Practices](https://www.youtube.com/watch?v=r8IHD5qX9Qk) video presentation from Cribl Community Office Hours.
>
{.box .info}

## Foundations: Cribl Stream and Object Storage 

In this first section, we'll lay out four basic factual underpinnings:

- [How Cribl writes](#writing-to-object-storage) to object storage.
- [Writing](#writing-to-s3-dest) to a Cribl Amazon S3 Destination.
- [Reading](#reading-from-sqs-source) from Cribl's Amazon SQS Source.
- [Replaying](#replaying) from Amazon S3 buckets, via Cribl's S3 Collector.

Then, this guide will go on to detail 10 "better practices" (and a bonus one!) as we cover four broad topics:

- Writing to object storage, [generically](#generic).
- [Cardinality](#cardinality) and partitioning expressions.
- [Amazon S3–specific](#optimization) optimization.
- Better [partitioning expressions](#expressions).

#### How Cribl Writes to Object Storage {#writing-to-object-storage}

Cribl does not write data **directly** to its object storage Destinations, which include Amazon S3; any solution with an S3 API; and other object storage Destinations, like [Azure Blob Storage](destinations-azure-blob), [MinIO](destinations-minio), and [Google Cloud Storage](destinations-google-cloud-storage).

Data persists locally to a "staging location" on the Workers. The Worker Processes append new data to the staging location based on partitioning expressions and configurations such as file size and max timeout settings. Each Worker Process, governed by its own specific settings, creates and maintains its own set of files.

![Writing to object storage – example](st-usecase-best-practices-ss1.865e49e5fc.png)
{border="true"}

#### Writing to a Cribl Amazon S3 Destination {#writing-to-s3-dest}

When writing to our Amazon S3 [Destination](destinations-s3), or any object storage, consider two main settings:
**File name prefix expression** and **Partitioning expression**.

Cribl allows a maximum of 2000 open files per Worker Process per S3 Destination. On a system with 14 Worker Processes, that translates to a maximum of 28,000 open files per S3 Destination. On a system with 30 CPUs, that translates to 60,000 open files. 

When a specific Worker Process hits the maximum open files, it closes all open files, moves them to the final output location, and creates new ones. If you have multiple object storage Destinations configured in Cribl, ensure that the total maximum open files across all these Destinations is less than your system's configured maximum open files. To figure out this maximum, multiply the number of file-based Destinations by max open file limit for each such Destination and by number of Worker Processes (Destinations x max open file limit x Worker Processes).

The default maximum open file settings on many Linux systems are too low for Cribl Stream when writing to an object storage Destination. You'll need to increase your system max open files and/or process max open files settings, as explained [below](#tuning).

Additionally, to limit the number of files in each directory, **Partitioning expression** should specify dates down to the hour or minute.

#### Reading from Cribl's Amazon SQS Source {#reading-from-sqs-source}

When reading data using Cribl Stream's Amazon SQS [Source](sources-sqs), a few settings can greatly speed up data retrieval. These are:

- **General Settings** > **Queue**, a.k.a. the filename filter. 
- **Advanced Settings** > **Max messages**.
- **Advanced Settings** > **Num receivers**.

Even more importantly, data retrieval speed is also governed by Destination settings and Pipeline efficiency.

#### Replaying from Amazon S3 Buckets {#replaying}

When replaying from S3 via Cribl's [S3 Collector](collectors-s3), optimize your performance by adjusting the following:

- **Optional Settings** > **Path**, a.k.a. the filter. 
- Pipeline efficiency.
- Destination settings.
- Destination health.

The **Path** should specify the time down to the hour, or in some cases, down to the minute.

Also, consider the API limits for your object storage bucket. AWS sets a limit of 3,500 writes/second and 5,500 reads/second per [S3 prefix](https://aws.amazon.com/premiumsupport/knowledge-center/s3-prefix-nested-folders-difference/). 
An S3 prefix is a path on Amazon S3, including bucket name and any subdirectories up to but not including the object name. The S3 prefix always ends in (and includes) the slash before the object name. In its simplest form, the S3 prefix is just `BucketName/`.

AWS API limits apply to all attempted access to these prefixes, cumulatively. You probably won't hit the write limits while putting the "better practices" in this guide into effect. However, populating too many files in any one directory might trigger throttling when reading and performing replays. 

> Some Cribl Destinations have **File prefix expression** and/or **Partitioning expression** settings. Do not confuse these with the AWS prefix.
>
{.box .info}



## Writing to Object Storage, Generically {#generic}

When you deploy a Cribl Amazon S3 Compatible Stores [Destination](destinations-s3), start with a simple global **Partitioning expression** and the default **File name prefix expression**.

This will work as long as cardinality remains below 2,000 during the max timeout period. Also, apply the procedures to increase the system max open file settings, as described later in this guide.

### Better Practice 1: Mind the Partitioning Expression

```shell
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}/${index ? index : 'no_index'}/${host ? host : 'no_host'}/${sourcetype ? sourcetype : 'no_sourcetype'}`
```

If cardinality exceeds 2,000, you can either:
- Remove the less-important fields from the expression; or 
- Create different partitioning expressions for different event types, categories, etc.

On the **Advanced Settings** tab: 

- Match the **Max open files** to cardinality, without exceeding 2,000.
- Toggle **Remove staging dirs** to `Yes`.
- Set the **Storage class** to `Intelligent Tiering`.

Still in  **Advanced Settings**, keep the default settings for:
- **Max file size (MB)**: `32`.
- **Max file open time (sec)**: `300`.
- **Max file idle time (sec)**: `30`.

As you gain deeper understanding of the data, consider the following options:
- Create a set of different S3 Destinations, each with its own **Partitioning expression**. They can all point to the same S3 bucket.
  Be aware that doing so will increase the number of API requests because events will be spread over more files.
- If the numbers of open files becomes too large, discuss alternative architectures (e.g., creating additional Worker Groups) with your Cribl Account team or Support. 

### Better Practice 2: Optimize System Settings for Max Open Files

Consider the workflow for writing to Cribl S3 Destinations:

- Each Cribl Worker Process creates its own set of files in a staging directory based on **Partitioning expression** and **File name expression**. 
- The maximum partitioning expression cardinality allowed is 2,000. This translates to a maximum of 2,000 open files per Worker Process per object storage Destination.
- Each Destination writes files to its staging directory until the number of files exceeds the configured maximum, or the Destination reaches any of the following limits: **Max file size (MB)**, **Max file idle time (sec)**, or **Max file open time (sec)**.

> When writing to an object store, more Worker Nodes with fewer CPUs is better than fewer Worker Nodes with more CPUs (e.g., 5 Workers with 12 vCPUs is better than 2 systems with 32 vCPUs). 
> 
> You should apply this principle to realize two benefits: 
> - To maximize the number of open files.
> - To have more S3 Destinations with more precisely-focused partitioning expressions.
>
{.box .info}

Linux defaults for maximum open files tend to be too low for writing to Cribl S3 Destinations,
but the 65,536 limit recommended by Cribl is a good starting point.
When setting the maximum open files substantially higher than 65,536, you should track system health.

You can further increase the maximum number of open files allowed by your system based on the number of S3 Destinations you are planning to use and the number of Worker Processes on each Node. The table below provides some examples: 

| **# of S3 Destinations in Cribl** | **Max cardinality per Dest.** | **Max open files in Cribl Dest.** | **# Worker Process on each Worker Node** |**Min Linux per-process open file limit** | **Min Linux system plus Cribl user open file and 1k reserved for the system** |
| --- | --- | --- | --- | --- | --- |
|1 |2,000|2,000 |10|3,000|21,000|
|1 |2,000|2,000 |14|3,000|29,000|
|1 |2,000|2,000 |22|3,000|45,000|
|2 |2,000|2,000 |10|5,000|41,000|
|2 |2,000|2,000 |14|5,000|57,000|
|2 |2,000|2,000 |22|5,000|89,000|
|5 |2,000|2,000 |10|11,000|101,000|
|5 |2,000|2,000 |14|11,000|141,000|
|5 |2,000|2,000 |22|11,000|221,000|

### Better Practice 3: Update Max Open File Settings on Hosts

Here are basic instructions that cover many Linux distributions. (Check the documentation for your specific Linux distribution and version for any variations required.)

Edit the following settings. You will be required to reboot.

```shell
# Update `fs.file-max` to a very large value run (as root):
sysctl -w fs.file-max=<open-files-limit> 

# or edit sysctl.conf (as root):

vi /etc/sysctl.conf 
fs.file-max = <open-files-limit>
```

```shell
# vi /etc/security/limits.conf (as Cribl user)
* soft nproc 65536 
* hard nproc 65536 
* soft nofile 65536 
* hard nofile 65536
```

After you reboot, verify the settings:

```shell
# sysctl -p
# cat /proc/sys/fs/file-max
# sysctl fs.file-max
```

## Cardinality and Partitioning Expressions {#cardinality}

As we've seen, partitioning expressions and file expressions interact to determine cardinality. In this section, we'll examine this interaction more closely and learn better practices based on what we find.

#### Where Cardinality Begins

Partitioning expressions specify the directory structure that Cribl adds when writing to an object storage Destination.
 
 For example, the following partitioning expression extracts the `year`, `month`, `day`, `hour`, `minute`, `index`, `host`, `sourcetype`, `server_ip`, `status_code`, and `Method`, and populates them as the directory structure. 

```shell
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')
/${index ? index : 'no_index'}
/${host ? host : 'no_host'}
/${sourcetype ? sourcetype : 'no_sourcetype'}
/${server_ip}
/${status_code}
/${Method}/`
```



A file expression prefix comes into play next. The default file expression is just a random file name. However, an expression like this would create files that also include the hour and minute in each file name. This reflects the time the file was created, not the time of each event.

`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%H%M')}`

> Together, the partitioning expression and the file expression must not produce a cardinality greater than 2,000.
>
{.box .warning}

#### Calculating Cardinality 

Cardinality is the quantity of combinations of unique values, within a time period, for each parameter in the expressions. 

For example, a partitioning expression could specify the following:
- Within an hour.
- 5 unique sourcetype values.
- 10 unique host values.
- 2 unique source values for each host.

For this example, we could calculate the maximum cardinality as `(5*10*2)=100`. 

It's important to also consider the timeframe this is calculated within. That cardinality might be 100 over a 1-minute period, but might be 500 over a 5-minute period. 

If we further add a file name prefix expression, this increases the cardinality. For example, let's add the method and top-level URI as file name prefix parameters. If there are 3 possible methods, and 10 URIs, the cardinality just went up from 100 to `100*3*10=3,000`.

Cardinality has the greatest impact on how you should configure an object storage Destination in Cribl Stream. For every combination of partitioning expressions, file prefix expressions, and Worker Processes, there can be an open file on the Worker Node's system.

A partitioning expression and file prefix expression should produce fewer than 2,000 potential combinations. When a Worker Process exceeds the configured **Max open files**, this generates errors; all open files for that Worker Process are simultaneously closed; and, new files will be created.

The Worker Process might experience some extra load while all files are processed. If this occurs infrequently, that's generally okay. But if it happens several times per hour, you should consider how to improve the combination of partitioning and file expressions.

As a starting point, consider these two strategies:

1. Write to one S3 bucket, incorporating parameters common to each event. 

  For example, here's a partitioning expression that specifies year, month, day, hour, minute, index, sourcetype, source, and host: 

  ```js
  ${C.Time.strftime(_time ? _time:Date.now()/1000,'%Y/%m/%d/%H/%M')/${index}/${host}/${sourcetype}
  ```

2. Dedicate different S3 Destinations to different event types. During replays, this makes searching specific data faster. However, this benefit comes at the expense of needing a larger number of open files. Should you pursue this option, test large numbers of open files on a test system in your environment. Such testing is especially crucial in virtualized environments with shared disk, CPU, and memory resources.

### Better Practice 4: Select Files for Partitioning and File Expressions

The main factors to consider when selecting fields for a partitioning or filename prefix expression are:

- Configure Time-based filtering: All partitioning expressions should include `year`, `month`, `day`, `hour`, and optionally, `minute`. This facilitates filtering during replays. It also limits the files in any one directory, so that replays are not throttled when returning large volumes of files.
- Identify Universal fields: Fields that users will typically search on during replays across all datasets. Examples include `sourcetype`, `host`, `source`, `category`, and `index`.
- Optionally, configure specific fields for different datasets.

  Examples include:

  - Web logs: server IP, method, status code.
  - Firewall logs: source zone, destination zone, accept/deny status.
  - Flow logs: protocol, accept/deny status.

  Be careful when pursuing this option as the number of open files can grow exponentially with too many multiple partition and file name prefix expressions.
 
#### Analyzing Partitioning Expressions

Let's work through some examples of partitioning expressions, with hypothetical possible values for each parameter, to see what cardinalities they produce.

**Example 1**

This one produces reasonable results.

```shell
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}
/${sourcetype}/${host}/${AttackType}`
```

Possible values:
- `sourcetype`: 5.
- `host`: 100.
- `AttackType`: 5.

Potential cardinality: `5 x 100 x 5 = 2,500`.

**Example 2**

Here, too, cardinality is kept commendably low.

```shell
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}/${state}/${interface}/${dest_ip}`
```

Possible values:
- `state`: 2.
- `interface`: 50.
- `dest_ip`: 20.

Potential cardinality: `2 x 100 x 10 = 2,000`.

**Example 3**

This is a bad partitioning expression which produces out-of-control cardinality. Back to the drawing board!

```shell
`${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}/${state}/${interface}/${src_ip}`
```

Possible values:
- `state`: 2.
- `interface`: 100.
- `src_ip`: 65,535.

Potential cardinality: `2 x 100 x 65,535 = 13,107,000`.

For more examples like these, see [Better Partitioning Expressions](#expressions). 

#### Seeing How Cardinality Changes Over Time 

In the examples so far, we've assumed typical expected values for parameters. Another useful exercise is to estimate the maximum possible values of each parameter, and multiply each value to obtain maximum theoretical cardinality.

You should also investigate how cardinality changes depending on when and how long you measure it. To do this, query/search your analytic tool to determine cardinality for different durations over several days.

Run the searches over different duration spans (e.g., 1 minute, 3 minutes, 5 minutes) to determine the best timeout period. For example, your cardinality might be 2,000 over a 2-minute span, but 10,000 over a 5-minute span. In this case, it is most certainly beneficial to set the maximum file timeout to 2 minutes.

Below are examples of Splunk searches over different span periods, with maximum open file time set to 3 minutes. Change the fields to the ones you'd prefer to incorporate into a partition and/or file name prefix expression, and try running them in your environment.

Here's an example using a "traditional" search method:

```shell
earliest=-24h index=* 
|bin _time span=3m 
| stats dc(source) as dc by host, sourcetype _time 
| stats sum(dc) by _time
```

Here's an example using `tstats`:

```shell
| tstats dc(source) as dc where index=* earliest=-25h@h latest=-1h@h by _time, host, sourcetype span=3m
| stats sum(dc) by _time
```

The `tstats` search produces the results shown in the diagram below. 
- Cardinality mostly stays below 1,625. 
- There are some hourly peaks around 2,500. These spikes, which exceed the max allowed cardinality of 2,000, are infrequent.

All of this is generally acceptable.

![cardinality Check Results](st-usecase-best-practices-ss2.1281a8d7a4.png)
{border="true"}

### Better Practice 5: Size a Separate Fast(er) Disk for Staging Location 

The system's ability to handle enough open files is a key factor that drives performance. This requires:
- Setting **Max open files** high enough.
- Ensuring that the staging directory on the Workers can handle simultaneous writing tens of thousands of open files. The metric to use here is Input/Output Operations per Second (IOPS). A good starting point is 2,000 IOPS.

Cribl recommends that you allocate a separate disk for the staging directory on Workers, especially when you have configured multiple object storage Destinations. Follow these general, conservative guidelines:
- Toggle **Advanced Settings** > **Remove staging dirs** to `Yes`. This means that any empty directory that has not been written to for an hour, is deleted.
- Disk size of 500 GB should be adequate, since the Workers need only enough disk capacity for a few minutes' worth of data. 500 GB errs on the side of caution, allowing us to store as much as 4 hours of data to disk. See the [formula and examples](#disk-size-formula) below.
- Because Cribl writes to the staging directory only when the object storage Destinations are healthy, disk size does not usually become an issue. If the Worker encounters or triggers a backpressure situation, it stops writing to the staging directory.
- The staging directories are not intended for high availability, but rather to support creation of large-enough files to write to object storage.

> Replay from a dedicated Worker Group. Do not compete for resources against production, real-time, streaming resources to perform replays!
>
{.box .warning}

#### Determining Staging Directory Disk Size {#disk-size-formula}

You can calculate an optimal size for your staging directory disk using the following formula:

- `Data Volume` (GB/day) you expect to ingest, times `1.25` metadata factor; divided by 
- `1440` (minutes in a day), times
- `Duration` (minutes) you want to store data on disk; divided by 
- `Number of Workers` writing to a staging directory.

The result will be disk size per Worker.

The figure below illustrates the formula.

![](st-usecase-best-practices-ss3.c0bb856c4c.png)
{border="true"}

Here are some examples:

| **Data Volume** | **Number of Workers** | **Duration** | **Disk Size per Worker** | 
| --- | --- | --- | --- |
| 1 TB/day | 3 | 2 | 35 GB |
| 10 TB/day | 5 | 4 | 417 GB | 
| 50 TB/day | 45 | 4 | 232 GB |

## Amazon S3 Optimization {#optimization}

Once you configure your Amazon S3 [permissions](#permissions), verify [access](#verifying-s3-access), and (if applicable) discover your Cribl.Cloud Organization's [AWS ARN](#access-from-cribl-cloud), you can start applying some more better practices.

#### Amazon S3 Permissions {#permissions}

If your Workers are hosted outside AWS, you will need an access key and secret key to access the S3 bucket. If the Workers are EC2 instances or in AWS EKS, it's most ideal to assign an EC2 service role to the instances. The role will need access to the S3 bucket. For policies for reading and writing to/from Amazon S3, see [AWS Cross-Account Data Collection](usecase-aws-x-account).

Here's a policy to grant a role, or user access, with read and write permissions the S3 bucket. Just change the bucket name in this policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::bucketname/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Resource": "arn:aws:s3:::bucketname"
    }
  ]
}
```

If `assumeRole` is required, here's a sample policy to assume the role that has access to the S3 bucket:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111111111111:role/account-a-cribl-stream-assumerole-role"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "cribl-s3cre3t"
        }
      }
    }
  ]
}
```

> Test your policy with [AWS's IAM policy simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) to ensure there are no boundary policies that might conflict with this policy. 
>
{.box .success}

#### Verifying S3 Access {#verifying-s3-access}

To troubleshoot access to your S3 bucket, install the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) on one of the Workers. Log in using your AWS user and Region. Assume the role if necessary. Then, attempt to download a file from the bucket or write a new file to the bucket. 

Example of assuming a role:

```java
aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session
```

Example of validating the role you'll use to run subsequent commands:

```java
aws sts get-caller-identity
```

Example of listing files in a bucket:

```java
aws s3 ls s3://s3-bucket-name
```

Example of sending stdin to a specified bucket:

```java
aws s3 cp - <target> [--options]
```

Example of writing a local file to the bucket:

```java
aws s3 cp localfilename.txt s3://bucket-name
```

Example of reading a file from S3:

```java
aws s3 cp s3://bucket-name localfilename.txt
```
#### Accessing Amazon S3 from Cribl.Cloud {#access-from-cribl-cloud}

Every Cribl.Cloud Organization exposes its own unique AWS ARN. 

From the main Cribl.Cloud portal, click the ⚙️ **Network Settings** link, then select the **Trust** tab to expose the Cribl.Cloud ARN.

![Cribl.Cloud ARN](st-usecase-best-practices-ss4.5c84354d0d.png)
{border="true"}

 Configure your AWS account with access to the S3 bucket, to allow Cribl.Cloud to assume that role, as illustrated above in [Amazon S3 Permissions](#permissions).

 ### Better Practice 6: Tuning Amazon S3 Destination Settings{#tuning}

 Under the Amazon S3 Destination's **General Settings** and **Advanced Settings** tabs are several settings for tuning your Amazon S3 Destination. Below are some recommendations for tuning those settings, in order of importance:

1. **Staging Location**: Change this to a directory on the Workers with increased IOPS, to accommodate thousands or tens of thousands of simultaneous open files. (Cannot be configured on Cribl.Cloud-managed Workers.)
1.  **Remove staging dirs**: This setting must be enabled, so as not to leave millions of empty directories on the system over time.
1.  **Compress**: Always enable compression, to reduce the size of files uploaded and downloaded from S3. This can reduce your S3 bill by 8–15x. Note that compression does add to the processing time – but transfer time will be substantially faster.
1.  **Max open files**: Adjust this to be slightly larger than the data's cardinality, but not greater than 2,000. Also, ensure that the system can support the number of open files for all Worker Processes, and across all configured S3 Destinations.
This value is set per process. 32CPUs*100 - 3200 max open files. Increase this value to avoid "Too many open files" errors. Default open file limit on Linux is 1024/process.
1.  **Max file size (MB)**: If you have a separate S3 Destination for high-volume data sources, such as VPC Flow Logs or firewall logs, consider raising this value in order to create fewer files. Start with 64 MB. Otherwise, the default 32 MB setting is generally acceptable.
1.  **Max file open time (sec)**: First, determine your cardinality over 1 minute, 3 minutes, 5 minutes, and 10 minutes. Adjust this setting to match the lowest-cardinality period. E.g., if cardinality is 10,000 over 5 minutes, but 2,000 over 90 seconds, change this setting to 90 seconds. If cardinality is 2,000 over 5 minutes, then the default 5 minutes (300 seconds) is acceptable.
1.  **Max idle time (sec)**: The default 30 seconds is generally acceptable. In cases with high cardinality, lowering this setting can result in fewer open files when processing bursty data.
1.  **Data format**: If using a Passthru Pipeline, change this setting to `Raw`. Otherwise, leave it at the JSON default to preserve all metadata added to the events.
1.  **Storage class**: For Amazon S3 Destinations, consider the "infrequent access tier" or "intelligent tier" to reduce overall cost if the data will be rarely searched. Examples of rarely-searched data include threat-hunting or compliance-reporting use cases.
"Standard, Infrequent Access" and "Intelligent Tiering" options should be used on low-access use cases such as threat hunting or compliance audit.
1.  **AWS Region**: Leave this field blank for non-AWS Destinations, unless instructed otherwise. For AWS, set this to the nearest geographic location to the Workers.

![Configuring S3 Destination Settings](st-usecase-best-practices-ss5.b7f3329566.png)
{border="true"}

### Better Practice 7: Lower AWS Costs

Lowering AWS costs can be achieved by a few means. First and foremost, ensure that compression is enabled on your Cribl S3 Destination and set to `gzip`. This will reduce both egress costs and S3 storage costs.

To eliminate egress costs from your Workers to S3, map a VPC endpoint on the S3 bucket to the VPC where your Workers reside.

![Mapping a VPC endpoint to an S3 bucket](st-usecase-best-practices-ss8.be265457b7.png)
{border="true"}

Writing to S3's Intelligent storage class, or infrequent access tier, can reduce long-term costs. Intelligent storage automatically routes data to cheaper S3 tiers when it is not searched or touched for certain periods of time. Consult the [AWS Cost Estimator](https://calculator.aws/#/addService/S3) to calculate the differences. Old data can also be rolled off to a much cheaper option such as Glacier Instant Retrieval or Glacier Deep Archive. Below are sample cost comparisons:

![AWS Cost Comparison](st-usecase-best-practices-ss10.2d36f7910a.png)
{border="true"}

> Cribl Stream Collectors and Replay features don't support S3 Glacier or S3 Deep
> Glacier due to long retrieval times for those storage
> classes.
> 
> Collectors and Replay features do support S3 Glacier Instant Retrieval when
> you're using the S3 Intelligent-Tiering storage class.
>
{.box .warning}

### Better Practice 8: Monitor Amazon S3 Buckets

AWS may limit S3 access if cumulative API writes are greater than 3,500 writes/second, or cumulative API reads are greater than 5,500 reads/second for a prefix (a.k.a., directory within S3). While it would be impossible to track every single AWS prefix's API requests, Cloudtrail offers "rate exceeded" events if you are ever over the API request limits.

Here's a Splunk search to report on all Cloudtrail events for S3. Filter or look for rate exceeded within the `errorMessage` field:

`index=aws sourcetype=aws:cloudtrail errorCode=* "resources{}.ARN"=* 
| rename "resources{}.ARN" as ARN 
| stats count by eventName errorCode errorMessage ARN`

![Cloudtrail Logs Search](st-usecase-best-practices-ss11.1351c6008b.png)
{border="true"}

If there are `rate exceeded` messages from writing to Amazon S3, then a new strategy for partition and filename prefix expressions might be warranted. You will want to create fewer files, so consider one or more of the following options:
- Increasing timeouts. 
- Eliminating file name expression prefixes. 
- Updating partitioning expressions.

If there are rate exceeded messages on reading from Amazon S3, then you should likely apply better filtering. This might also warrant a new strategy for how files are written to S3.

For trending API calls, you'll need to focus on a specific prefix. This might be useful for specific AWS prefixes, as there will be hundreds of thousands or millions. API calls can be tracked via AWS Cloudwatch, but first need to be enabled for tracking within the S3 bucket. See AWS' [Creating a Metrics Configuration that Filters by Prefix, Object Tag, or Access Point](https://docs.aws.amazon.com/AmazonS3/latest/userguide/metrics-configurations-filter.html) topic. 

The screenshot below illustrates the locations to enable tracking API calls for a specific prefix:

![Enable Monitoring in S3](st-usecase-best-practices-ss12.d48cd82e37.png)
{border="true"}

Here's a Cloudwatch chart trending all requests, and write requests, for an S3 prefix. This is granular to 1 minute. Because the API limits are per second, some extrapolations are necessary to estimate per-second values.

![Cloudwatch Chart](st-usecase-best-practices-ss13.5db3fd6941.png)
{border="true"}

Consider both the write and read behavior when designing your partitioning expressions and file name prefix expressions.

### Better Practice 9: Monitor Worker Node Performance, and Enable Notifications

Aside from the standard CPU load, disk space, memory usage metrics, consider tracking some additional metrics. If the Worker Nodes are deployed as EC2 instances, track the Cloudwatch metrics for the EC2 instances and associated disks. Cribl Support will ask for these metrics to track Amazon S3 issues:
- `BurstBalance`
- `VolumeReadBytes`
- `VolumeWriteBytes`
- `VolumeReadOps`
- `VolumeWriteOps`
- `VolumeQueueLength`

On the Linux host, the following command allows you to track the number of open files. This would be another useful metric to track over time:

`lsof | grep <common_upperlevel_staging_dir> | wc -l`

You can also configure Notifications in Cribl Stream when the S3 Destination is unhealthy or triggering backpressure. Once the S3 Destination is enabled, and a Notification target is set, you need to:
- Edit Cribl Stream's S3 Destination to configure Notifications.
- Add Notifications for different conditions.

### Better Practice 10: Dedicate a Separate Worker Group for Replays

Execute Replays from a dedicated Worker Group. Streaming real-time data needs dedicated Worker Groups. You do not want CPU or memory conflicts on production systems that are streaming mission-critical, real-time machine data. Kubernetes or AWS spot instances are good options for on-demand Worker Processes that are needed only for infrequent replays.

### Bonus Better Practice: Tune Amazon S3 Source Settings 

For Cribl Stream's SQS-based Amazon S3 Source (as opposed to our S3 Collector), a few settings can speed up processing of new files added to an S3 bucket: 
1. Under **General Settings**, apply a **Filename filter** if you want to pull only a subset of files from the S3 filter. The value is a regular expression (e.g., *suricata*)
1. Under **Advanced Settings**, set **Max messages** as high as 10 to fetch more SQS messages from the queue at once.
1. Under **Advanced Settings**, set the **Num receivers** to vertically scale the number of simultaneous processes pulling SQS messages and downloading S3 files. For S3 buckets with centralized Cloudtrail logs, or VPC flows across multiple accounts, raise the setting to 5 or 10.

## Improving Partitioning Expressions {#expressions}

Creating partitioning expressions is an art. To get a sense of the possibilities and risks, let's consider both positive and negative examples.

### Partitioning Expression 1: The Good

```shell
${C.Time.strftime(_time ? _time : Date.now()/ 1000, '%Y/%m/%d/%H/%M')}
```

| **Key Questions** | **Answers** | 
| --- | --- |
|Potential cardinality every *minute*? |1|
|How many open files if there are 30 Worker Processes? |30 open files|
|How many files in the S3 prefix, if the timeout is the default 60 seconds (assuming no file size limit)?  |30 \* 1 = 30 files|
|What if there are 10 Nodes in the Worker Group? How many files in each S3 prefix?  |30 \* 10 = 300 files|
|What happens when a replay fetches data for this hour? |Under API limit of 5,500 reads/sec|

**Result** 
- A good partitioning expression.

###  Partitioning Expression 2: The Ambiguous

```shell
${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}
/${index ? index : 'no_index'}
/${host ? host : 'no_host'}
/${sourcetype ? sourcetype : 'no_sourcetype'}
```

| **Key Questions** | **Answers** | 
| --- | --- |
|Potential cardinality every 3 minutes? |3,000|
|How many active subdirectories in the staging dir? |3,000|
|What is the excess cardinality?  |3,000 - 2,000 = 1,000|
|What are maximum open files if there are 14 Worker Processes on a Worker ?  |14 \* 2,000 = 28,000 open files|
|What happens when maximum open files of 2,000 is reached for any one Worker Process? What happens if it happens too frequently? |All files closed & written to S3 at once = 2,000 writes. Potential for backpressure if Cribl Stream takes long enough to write the files.|
|What's the maximum number of files in any S3 prefix? |14 \* 10 Worker Process \* (60min/3min) = 2800 files max in any S3 prefix|

Additional assumptions:
- Cardinality of 3,000 every 3 mins.
- File write timeout of 3 minutes.
- Max file size never reached.
- 10 Worker Process in group.

**Result** 
- Proceed with caution.

**Solution** 
- Track the occurrences of `max open files reached` messages
- If the `max open files reached` message occurs too frequently, consider reducing the cardinality. You might achieve this by lowering the timeout period to 2 minutes or 1 minute.

### Partitioning Expression 3: The Bad

```shell
${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d/%H')}
```

| **Key Questions** | **Answers** | 
| --- | --- |
|Potential cardinality every hour? |1|
|How many open files if there are 30 Worker Processes? |30 open files.|
|How many files in the S3 prefix, if the timeout is the default 60 seconds (assuming no file size limit)?  |(30 \* 60) = 1800 files|
|What if there are 10 Nodes in the Worker Group? How many files in each S3 prefix?  |(1,800 \* 10) = 18,000 files|
|What happens when a replay fetches data for this hour? |Could exceeed the API limit of 5,500 reads/sec.|

**Result** 
- A bad partitioning expression.

**Solution** 
- Change the timeout to 1800 seconds to allow files to grow in size.

### Partitioning Expression 4: The Terrible

```shell
${C.Time.strftime(_time ? _time : Date.now() / 1000, '%Y/%m/%d')}
/${index ? index : 'no_index'}
/${host ? host : 'no_host'}
/${sourcetype ? sourcetype : 'no_sourcetype'}
```

**Filename Prefix Expression**:

```shell
${C.Time.strftime(_time ? _time : Date.now() / 1000, '%H%M')}
```

| **Key Questions** | **Answers** | 
| --- | --- |
|What are max open files on any one Worker Process? |Answer: `2,000`, because that is the lower of: `[(500 directories * 5 filename prefix files = 2,500 open files per process]` and `[2,000 open files]`|
|How many active subdirectories in the staging dir? |Around 400|
|What happens when max open files of 2,000 is reached for any one Worker Process?  |All files closed and written to S3 at once (2,000 writes at once). Happens every 4 minutes. There's potential for backpressure if Cribl Stream takes long enough to write the files.|
|What's the max number of files in any S3 prefix?  | 14 processes \* 1440 mins \* 10 Worker Process = 202K files! API issues on read are likely.|

**Additional Assumptions**
- Reduced cardinality of 200 every 2 minutes, and 500 every 5 minutes.
- Introduction of a file prefix partition.
- Max open files within Cribl Destination set to 2,000.
- Max file open time of 5 minutes.
- Assume max file size is never reached.
- 10 Worker Processes in Group.
- 14 Worker Processes/Worker.

**Result** 
- A terrible partitioning expression.

**Solution** 
- Don't use the file prefix expression; consider expanding the partitioning expression instead.
- Reduce timeout to 2 minutes, as cardinality will be lower over a 2-minute span.
