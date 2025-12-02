# Integrating Cribl Stream with AWS Security Hub


> This document is not intended for public use. Pre-release only. Please keep content and URL confidential.
>
{.box .info}

Integrating Cribl Stream with AWS Security Hub enables security teams to ingest, standardize, and route Security Hub findings in near real time. This integration streamlines security operations by providing a unified, flexible, and extensible data pipeline for AWS security events, leveraging the Open Cybersecurity Schema Framework (OCSF) v1.6 for consistent data structure and interoperability.

Key features include:

* Near real-time ingestion of AWS Security Hub findings via AWS EventBridge and Cribl Stream Webhook.    
* Standardization of findings to OCSF v1.6, including support for AWS-specific extensions.
* Flexible routing to multiple destinations, including Cribl Lake, SIEMs, and data warehouses.
* Support for transforming and enriching third-party security findings.
* Secure, scalable architecture with recommended authentication and IAM permissions.

## Architecture and Data Flow

This integration uses an event-driven architecture to deliver Security Hub findings to Cribl Stream for processing and routing:
1.  AWS Security Hub generates a finding.    
2.  AWS EventBridge captures the finding event.
3.  An EventBridge Rule filters and forwards events to a target.
4.  The target is typically an AWS SNS Topic, which pushes the data to a Cribl Stream HTTP Endpoint (Webhook).\
5.  Cribl Stream receives the data, processes and standardizes it (for example, to OCSF v1.6), and routes it to one or more Destinations.

## Setting Up Event Streaming from AWS Security Hub to Cribl Stream

### 1. Create an Amazon SNS Topic

1. In the AWS Management Console, go to **Simple Notification Service (SNS)**.
2. Create a new topic (for example, `Cribl-SecurityHub-Findings`).

### 2. Configure a Cribl Stream HTTP Endpoint as an SNS Subscription

1. In Cribl Stream, navigate to Sources and enable an HTTP Endpoint (Webhook). Copy the unique URL.
2. In AWS SNS, create a subscription for the topic created above. The Protocol is `HTTPS`. For the URL, paste the Cribl Stream HTTP Endpoint URL. 

### 3. Create an EventBridge Rule

1. In the AWS Management Console, go to **Amazon EventBridge**.
2. Select **Create rule** and provide a descriptive name (for example, `SecurityHub-to-Cribl`).
3. Define the event pattern as follows: 
   - **Event source**: AWS services
   - **AWS service**: Security Hub
   - **Event type**: Security Hub Findings - Custom Action or Security Hub Findings - Imported
   - Example pattern: 
  
  ```

{
  "source": ["aws.securityhub"],
  "detail-type": ["Security Hub Findings - Imported"]
}
  ```

   - Set the target to the SNS Topic created in Step 1.

### 4. Enable the Webhook in Cribl Stream

1. In Cribl Stream, go to **Data > Sources**.
1. Select **Add new** and select **HTTP Endpoint**.
   - **Input ID**: Provide a unique ID (for example, `securityhub_webhook`).
   - **Authentication**: Configure Shared Secret authentication for security. (Note: For advanced customization, consider using an intermediate Lambda or API Gateway.)
   - **Output**: Connect the source to a specific pipeline (for example, `securityhub_pipeline`) for OCSF conversion and routing.

## Processing and Standardizing Data in Cribl Stream

1. Pre-process: Handle the SNS envelope/wrapper if present.
2. Standardize: Convert the Security Hub JSON payload to OCSF v1.6 format, adding AWS context/extensions as needed.
3. Enrich: Optionally add external or internal data (for example, CMDB lookups, CloudTrail events).

## Routing and Destination Management

After processing, Security Hub findings can be routed to multiple destinations:

| Destination Service | Cribl Stream Destination Type | Purpose |
| --- | --- | --- |
| Cribl Lake | Amazon S3 (managed by Cribl) | Long-term, cost-effective storage for historical analysis and compliance. |
| SIEM Platform | Splunk HEC, Kafka, ElasticSearch | Real-time security analysis, alerting, and incident response. |
| Data Warehouse | Amazon S3, Snowflake, Google BigQuery | Business intelligence and operational reporting on security trends. |

This flexible routing ensures Security Hub findings are immediately available for analysis and archival across your security ecosystem.

## Transforming Third-Party Findings

Cribl Stream can also convert third-party security findings into OCSF v1.6, including AWS-specific extensions. This enables a unified approach to security data management, regardless of the original source format.

| Original Source | Original Format | Cribl Stream Action | Output Format |
| --- | --- | --- | --- |
| Third-Party Tool 1 | Proprietary | Convert and Add AWS Extensions | OCSF v1.6 with AWS Extensions |
| Third-Party Tool 2 | Custom JSON | Convert and Add AWS Extensions | OCSF v1.6 with AWS Extensions |

## Identity Access Management (IAM) Permissions Required

To enable robust integration, the IAM role associated with your Cribl deployment should have the following permissions (at minimum):

| Endpoint | Permission(s) |
| --- | --- |
| `ec2_instances`, and so forth | `ec2:DescribeInstances`, `ec2:DescribeVolumes`, `ec2:DescribeSecurityGroups`, and so forth |
| `lambda_functions` | `lambda:ListFunctions` |
| `iam_*` | `iam:ListPolicies`, `iam:ListRoles`, `iam:ListUsers`, `iam:ListGroups`, `iam:ListMFADevices` |
| `cloudformation_*` | `cloudformation:ListExports`, `cloudformation:ListStacks`, `cloudformation:ListStackSets` |
| `dynamodb_backups` | `dynamodb:ListBackups` |
| `rds_*` | `rds:DescribeDBInstances`, `rds:DescribeDBClusterEndpoints`, and so forth |
| `cloudtrail_events` | `cloudtrail:LookupEvents` |
| `vpc_*` | `ec2:DescribeNetworkInterfaces`, `ec2:DescribeVpcs`, `ec2:DescribeSubnets` |
| `efs_file_systems` | `elasticfilesystem:DescribeFileSystems` |

For a full list and details, see the Cribl documentation on AWS API permissions.

## Security Best Practices

- Always use Shared Secret authentication for HTTP Endpoints.
- Limit IAM permissions to the minimum required for your use case.
- Use AWS CloudFormation templates provided by Cribl for automated, secure setup of IAM roles and trust relationships.

## Getting Started

If you are new to AWS Security Hub or Cribl Stream, refer to the following resources:
- Enable Security Hub in AWS: See AWS documentation for setup instructions.
- Configure Cribl Stream: Refer to Cribl documentation for detailed configuration steps.

## Summary
Integrating Cribl Stream and AWS Security Hub delivers a powerful, flexible, and secure solution for ingesting, standardizing, and routing security findings. By leveraging OCSF v1.6 and event-driven AWS services, security teams can achieve real-time visibility and operational efficiency across their cloud environments.
