# Amazon Security Lake Integration


Cribl Stream supports sending data to [Amazon Security Lake](https://docs.aws.amazon.com/security-lake/latest/userguide/what-is-security-lake.html) as Parquet files that adhere to the [Open Cybersecurity Schema Framework](https://docs.aws.amazon.com/security-lake/latest/userguide/open-cybersecurity-schema-framework.html) (OCSF). The diagram below illustrates the all-AWS variant of this architecture.

![Integrating Cribl Stream with Amazon Security Lake](security-lake-integ-4.7.png)
{scale="90%" border="false"}

This guide walks you through an example where Cribl Stream, running in [Cribl.Cloud](deploy-cloud), sends Palo Alto Networks logs to Amazon Security Lake. The workflow starts with a Cribl Stream [Syslog Source](sources-syslog), which ingests the Palo Alto data and routes it to a Cribl Stream [Amazon Security Lake Destination](destinations-security-lake), which then writes Parquet files to Amazon Security Lake. These Parquet files organize the data according to the OCSF schema for the appropriate OCSF Event Class, in this case [Network Activity](https://schema.ocsf.io/classes/network_activity) [4001].

You can work with other data sources besides Palo Alto Networks, and other OCSF Event Classes besides Network Activity [4001]. See [Going Beyond This Example](#going-beyond) below.

If you want to use Amazon Security Lake with a Cribl Stream instance deployed on-prem or in your private cloud, the setup procedures will differ from those documented here. Please contact Cribl via the `#packs` channel of Cribl Community Slack.

## Set Up Amazon Security Lake {#set-up-security-lake}

Complete the Amazon Security Lake Getting Started [instructions](https://docs.aws.amazon.com/security-lake/latest/userguide/getting-started.html), paying particular attention to the following actions: 

1. Enable Amazon Security Lake for your AWS account.
1. Define the **collection objective** and **target objective**.
1. Select the Regions where you want to create S3 buckets to store the data in OCSF format.

Note your Amazon Security Lake S3 bucket name for use later in the setup process. 

## Set Up Cribl Stream {#set-up-stream}

Complete the procedures in this section before doing anything else in Cribl Stream.

First, find and note the Amazon Resource Name (ARN) that enables AWS to locate your Cribl Stream instance.

1. Log in to Cribl.Cloud.
2. In the sidebar, select **Workspace**, and then select **Trust**.
3. Select the copy icon next to the **Worker ARN**, and paste the ARN into a local text editor.
4. Copy the 12-digit number in the middle of the ARN. This is the AWS Account ID you'll need [later](#create-custom-source) in the setup process.

For example: 
* If your ARN is `arn:aws:iam::222233334444:role/main-default`, then ...
* Your AWS Account ID will be `222233334444`.

Next, install the Cribl Pack that Cribl Stream will use to send data to Amazon Security Lake in the required OCSF format. (As the [reference](#data-sources) shows, the Palo Alto Networks data chosen for this example requires the **OCSF Post Processor Pack**.)

1. On the top bar, select **Products**, and then select **Cribl Stream**.
1. Select the name of the Worker Group whose Worker Nodes you want to send data to Amazon Security Lake. (For this example, we'll use the `default` Worker Group.) 
1. Navigate to **Processing** > **Packs** (Stream) or **More** > **Packs** (Edge) and select **Add Pack**. From the drop-down, select **Import from Git**.
1. For the **URL**, enter:
    ```
    https://github.com/asc-me-cribl/cribl_ocsf_postprocessing
    ```
1. For the **New Pack ID**, enter: 
    ```
    Cribl-OCSF_Post_Processor
    ```
1. Select **Import** to close the modal. Cribl Stream will take a little time to finish importing the Pack.

At the bottom of the list of Packs, you should now see your newly imported Pack, listed with the display name of **OCSF Post Processor**.

Finally, you will need the Parquet schema that supports the OCSF Event Class you're working with. In this example, that's OCSF Event Class 4001, so as shown in the reference [below](#data-sources), you'll need the `OCSF 4001 Network Activity` Parquet schema.
* First, download the Parquet schema from [this](https://github.com/criblpacks/cribl-ocsf-parquet-schemas/) GitHub repo.
* Then, upload the schema to the Cribl Stream Parquet schema library as described [here](parquet-schemas#creating-parquet-schemas).

Once this is done, the Parquet schema you need should be available when you configure the **Parquet Settings** for your Cribl Stream Amazon Security Lake Destination.

Please post any questions you have about these procedures to the `#packs` channel of Cribl Community Slack.

## Create an Amazon Security Lake Custom Source {#create-custom-source}

Because Cribl is not a native Amazon service, you'll configure what Amazon calls a **Security Lake custom source**, and the Cribl Stream Amazon Security Lake Destination will write to that.

Before you begin:
1. Have your Cribl.Cloud AWS account ID, which you identified [earlier](#set-up-stream), handy.
1. Ensure that  you have permissions in AWS Lake Formation to create AWS Glue databases and tables. For more information, see [Granting and revoking permissions using the named resource method](https://docs.aws.amazon.com/lake-formation/latest/dg/granting-cat-perms-named-resource.html) in the AWS docs.

Then complete the instructions [here](https://docs.aws.amazon.com/security-lake/latest/userguide/custom-sources.html#custom-sources-best-practices) in the AWS docs. These docs will take you through the **Create custom data source** template show below.

![Creating an Amazon Security Lake custom source](security-lake-integ-02.a8f29a0a2e.png)
{scale="70%" border="true"}

Once the custom source has been created, still in AWS, navigate to **Security Lake** > **Custom sources** and you should see your new custom source in the list.

Now locate the **Provider role ARN**. This is the ARN that Cribl Stream will use to declare its role to AWS.
1. Select the copy icon next to the **Provider role ARN** to add it to your clipboard. Keep the ARN handy for use later in the setup process.
1. Navigate to the **Identity and Access Management (IAM)** screen. 
1. From the left menu, select **Access management** > **Roles**.
1. In the **Roles** search box, paste the part of the ARN that begins `AmazonSecurityLake` and ends with the Region.
1. When the role name appears below, select the link.
1. In the **Trust relationships** tab, view the `ExternalId` element in the JSON object that appears there.

This is the External ID for the trust relationship, which you created when went through the template. You will need this ID later in the setup process.

## Create a Cribl Amazon Security Lake Destination {#create-security-lake-destination}

Back in Cribl Stream, create a new Amazon Security Lake Destination in the Routing UI as described in the Destination [topic](destinations-security-lake). In the **New Destination** modal, configure the settings specified below. For all settings **not** mentioned in the following notes, you can keep the defaults.

#### General Settings

**S3 Bucket name**: The S3 bucket name you noted when [setting Up Amazon Security Lake](#set-up-security-lake).

**Region**: Select the Region where the S3 bucket is located.

##### Optional Settings

**File name prefix expression**: Keep the default (`CriblOut`) if it satisfies your requirements; otherwise, edit to suit your needs.

#### Authentication

**Authentication method** must be set to `Auto` (the default).

#### Assume Role

Cribl strongly recommends using the **Auto** authentication method in conjunction with the **Assume Role** settings as described below. Both Cribl and Amazon discourage the use of other approaches.

When using Assume Role to access resources in a different Region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that Region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node. Setting an invalid Region results in a fallback to the global STS endpoint.

**AssumeRole ARN**: This is the **Provider role ARN** you copied after [creating](#create-custom-source) your custom source in Amazon Security Lake.

**External ID**: Enter the ID that you specified when [creating](#create-custom-source) your Amazon Security Lake custom source.

#### Processing Settings

##### Post-Processing

**Pipeline**: From the drop-down, select the Pack you installed [earlier](#set-up-stream): 
```
PACK Cribl-OCSF_Post_Processor (OCSF Post Processor)
```

**System fields**: Remove any fields specified here.

#### Parquet Settings

**Parquet schema**: From the drop-down select `OCSF 4001 Network Activity`, since OCSF Event Class 4001 describes the events coming from Palo Alto Networks in this example.

At this point, you can select **Save**; the defaults for **Advanced Settings** should work fine for this example. Then, **Commit** and **Deploy**.

> You must create one Cribl Stream Amazon Security Lake Destination for **each unique pairing** of a data source with an OCSF Event Class.
> * In this example, we're ingesting Palo Alto Networks Threat and Traffic data, which requires the OCSF [Network Activity](https://schema.ocsf.io/1.0.0-rc.2/classes/network_activity) [4001] Event Class. That's one pairing.
> * If you also wanted to ingest CrowdStrike Network Activity data, that would be a new data source paired with the same OCSF Event Class –  such as, **that would be a second unique pairing**. You would need to create a **separate** Cribl Stream Amazon Security Lake Destination for that pairing.
> 
> The [reference](#data-sources) below shows all the possible pairings of data sources and OCSF Event Classes.
>
{.box .info}

## Connect a Cribl Source to the S3 Destination

We'll now configure a Cribl Stream Syslog Source to ingest Palo Alto Networks system logs, using the QuickConnect UI. (The [reference](#data-sources) below shows what Cribl Stream Source to use for each supported data source.)

Navigate to QuickConnect as described [here](sources-syslog#configuring-product-to-receive-data-over-syslog). After selecting the Syslog Source tile, select **Select Existing**, then select the preconfigured `in_syslog` Source. When prompted to `Switch in_syslog to send to QuickConnect instead of Routes?`, select `Yes`.

Select **+** and drag the connection line to the S3 Destination you created [above](#create-security-lake-destination).

The connection between the Syslog Source and the S3 Destination should now be enabled.

**Commit** and **Deploy** the changes.

## Send Data to Amazon Security Lake

Return to your AWS Console. You should see Parquet files landing in the S3 bucket. These files should contain the Palo Alto Networks syslog data you sent through Cribl Stream.

#### Troubleshooting and Testing

Good things to double-check:
* Are you sending the events to your Amazon Security Lake in Parquet format? 
* Are your permissions set properly for your IAM role to write to the S3 bucket?

If the answer to either question is "No," your data will not make it into Amazon Security Lake. 

To send sample events from the GitHub repo, using netcat:
1. Download the `oYLbFU.json` [sample file](https://github.com/asc-me-cribl/cribl_ocsf_postprocessing/blob/main/data/samples/oYLbFU.json).
1. Run the following command, replacing `<cribl_org_id>` with your Cribl.Cloud [Organization ID](cloud-portal#configure-organization-details): 

```shell
cat oYLbFU.json | nc default.main.<cribl_org_id>.cribl.cloud 10070
```

> With a Cribl.Cloud Enterprise plan, generalize  the above URL's `default.main` substring to `<groupName>.main` when sending to other Worker Groups.
>
{.box .info}

## Beyond This Example {#going-beyond}

If you want to send data from sources other than Palo Alto Networks, you can adapt the above instructions to use the appropriate Source and Pack (and/or modifications to the OCSF Post Processor Pack) in Cribl Stream. This holds true as long as the OCSF Event Classes you want to work with are among those supported by Cribl Stream. For each unique pairing of a data source with an OCSF Event Class, you'll need to create a separate Amazon Security Lake Destination.

In the next section is a list of the supported data sources, what OCSF Event Classes they handle, and what Packs and Parquet schemas you need to work with them.

## Reference: Supported Data Sources {#data-sources}

| Data Source | OCSF Event Classes Handled | Cribl Source | Cribl Pack Display Name | Parquet Schema(s) |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| Azure Audit Logs | 3002, 3004 | [REST Collector](/stream/collectors-rest/) against Microsoft Graph API  | Azure Audit Logs to OCSF | `OCSF 3002 Authentication` <br /><br /> `OCSF 3004 Entity Management` | 
| Azure NSG Flow Logs | 4001 | [Azure Blob Storage Source](/stream/sources-azure-blob/), run as scheduled jobs, not as a Pull Source | Azure NSG Flow Logs | `OCSF 4001 Network Activity` |
| Cisco ASA | 4001 | [Syslog](sources-syslog) | Cisco ASA | `OCSF 4001 Network Activity` | 
| Cisco FTD | 4001 | [Syslog](sources-syslog) | Cisco FTD | `OCSF 4001 Network Activity` | 
| CrowdStrike Account Change | 3001 | [CrowdStrike FDR](/stream/sources-crowdstrike/) | Crowdstrike FDR Pack | `OCSF 3001 Account Change` | 
| CrowdStrike Authentication | 3002 | [CrowdStrike FDR](/stream/sources-crowdstrike/)  | Crowdstrike FDR Pack | `OCSF 3002 Authentication` | 
| CrowdStrike Network Activity | 4001 | [CrowdStrike FDR](/stream/sources-crowdstrike/)  | Crowdstrike FDR Pack | `OCSF 4001 Network Activity` |
| GCP Audit Logs Account Activity | 3001 | [Google Cloud Pub/Sub](/stream/sources-google_pubsub/) | Google Cloud Audit Logs | `OCSF 3001 Account Change` |
| Palo Alto Networks (PAN) Threat and Traffic | 4001 | [Syslog](sources-syslog) | OCSF Post Processor Pack | `OCSF 4001 Network Activity` |
| SentinelOne | 1001, 2001, 3002, 4001 | [Amazon S3](/stream/sources-s3/) | SentinelOne Cloud Funnel | `OCSF 1001 File System Activity` <br /><br /> `OCSF 2001 Security Finding` <br /><br /> `OCSF 3002 Authentication` <br /><br /> `OCSF 4001 Network Activity` |
| Windows Logon Activity | 3002 | [Splunk TCP](sources-splunk) | Splunk Forwarder Windows Classic Events to OCSF | `OCSF 3002 Authentication` |
| ZScaler Firewall and Weblogs | 4001 | [Syslog](sources-syslog) | OCSF Post Processor Pack | `OCSF 4001 Network Activity` |
