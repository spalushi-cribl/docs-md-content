# Connect Cribl Search to AWS API


Configure Cribl Search to query an AWS API endpoint.

---

[Amazon Web Services (AWS)](https://aws.amazon.com/) offers scalable and cost-effective cloud computing solutions.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to
[search](search-overview) the [AWS API](https://docs.aws.amazon.com/) supporting the following endpoints:

| Product        | Endpoints                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EC2            | [EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/Welcome.html), [EC2 Volumes](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeVolumes.html), [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_SecurityGroup.html)                                                                                                                                                                                                                                                          |
| Lambda         | [Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/API_Reference.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| IAM            | [Users](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListUsers.html), [Roles](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListRoles.html), [Groups](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListGroups.html), [Policies](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListPolicies.html), [MFA Devices](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListMFADevices.html)                                                                                                              |
| CloudFormation | [StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_ListStackSets.html), [Stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_ListStacks.html), [Exports](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_ListExports.html)                                                                                                                                                                                                                                           |
| DynamoDB       | [Backups](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_ListBackups.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDS            | [Clusters](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBClusters.html), [Cluster Endpoints](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBClusterEndpoints.html), [Instances](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBInstances.html), [Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSecurityGroups.html), [Certificates](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeCertificates.html) |
| CloudTrail     | [Events](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_LookupEvents.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| VPC            | [VPCs](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeVpcs.html), [Subnets](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeSubnets.html), [Network Interfaces](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeNetworkInterfaces.html)                                                                                                                                                                                                                                                   |
| EFS            | [File Systems](https://docs.aws.amazon.com/efs/latest/ug/API_DescribeFileSystems.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## Add an AWS API Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an AWS API
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Set the following configurations in the **New Dataset Provider** modal:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **AWS API**.
1. **Authentication method** provides two options, **Assume Role** and **AWS keys**. See how to
   [grant access to AWS](aws-access) for details on each option.
1. Select **Add Configuration** to specify your AWS account. The configuration depends on the **Authentication method**
   selected and you can use only one method for all configurations. In the **Account Configurations** table:
   - **Assume Role** requires the IAM role's ARN (**AssumeRole ARN**) and has options to define an **External ID** and
     **Duration**.
     - The **External ID** on the Dataset Provider must match the
       [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) defined
       in the IAM Role [Trust Policy](aws-access#trust).
     - **Duration** defines the Assumed Role's session length of time, in seconds. Minimum is `900` (15 minutes),
       default is `3600` (1 hour), and maximum is `43200` (12 hours).
   - **AWS keys** requires the IAM user's account **Name**, **Access key**, and **Secret key**.
1. Select **Save** when finished.

For details on obtaining your AWS credentials, see [Grant Access to AWS](aws-access).

### Permission Requirements for AWS API

Accessing specific AWS endpoints requires the following permissions:

| Endpoint                                                                                                        | Permission                                                                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ec2_instances`<br/>`ec2_volumes`<br/>`ec2_security_groups`                                                     | `ec2:DescribeInstances`<br/>`ec2:DescribeVolumes`<br/>`ec2:DescribeSecurityGroups`<br/><br/>or<br/><br/>`ec2:Describe*`                                                                            |
| `lambda_functions`                                                                                              | `lambda:ListFunctions`                                                                                                                                                                             |
| `iam_users`<br/>`iam_groups`<br/>`iam_policies`<br/>`iam_roles`<br/>`iam_mfa_devices`                           | `iam:ListPolicies`<br/>`iam:ListRoles`<br/>`iam:ListUsers`<br/>`iam:ListGroups`<br/>`iam:ListMFADevices`                                                                                           |
| `cloudformation_stacks`<br/>`cloudformation_stacksets`<br/>`cloudformation_exports`                             | `cloudformation:ListExports`<br/>`cloudformation:ListStacks`<br/>`cloudformation:ListStackSets`                                                                                                    |
| `dynamodb_backups`                                                                                              | `dynamodb:ListBackups`                                                                                                                                                                             |
| `rds_clusters`<br/>`rds_cluster_endpoints`<br/>`rds_instances`<br/>`rds_security_groups`<br/>`rds_certificates` | `rds:DescribeDBInstances`<br/>`rds:DescribeDBClusterEndpoints`<br/>`rds:DescribeDBSecurityGroups`<br/>`rds:DescribeCertificates`<br/>`rds:DescribeDBClusters`<br/><br/>or<br/><br/>`rds:Describe*` |
| `cloudtrail_events`                                                                                             | `cloudtrail:LookupEvents`                                                                                                                                                                          |
| `vpc_vpcs`<br/>`vpc_subnets`<br/>`vpc_network_interfaces`                                                       | `ec2:DescribeNetworkInterfaces`<br/>`ec2:DescribeVpcs`<br/>`ec2:DescribeSubnets`                                                                                                                   |
| `efs_file_systems`                                                                                              | `elasticfilesystem:DescribeFileSystems`                                                                                                                                                            |

## Add an AWS API Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Set the following configurations in the **New Dataset** modal:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of an [AWS Dataset Provider](#provider).
1. Under **Enabled endpoint**, select **Add Endpoints** to select the endpoints for your Dataset. Select an endpoint
   from the drop-down menu. Your options are:
   - `ec2_instances`
   - `ec2_volumes`
   - `ec2_security_groups`
   - `lambda_functions`
   - `iam_users`
   - `iam_roles`
   - `iam_groups`
   - `iam_policies`
   - `iam_mfa_devices`
   - `cloudformation_stacks`
   - `cloudformation_stacksets`
   - `cloudformation_exports`
   - `dynamodb_backups`
   - `rds_exports`
   - `rds_backups`
   - `rds_clusters`
   - `rds_cluster_endpoints`
   - `rds_instances`
   - `rds_security_groups`
   - `rds_certificates`
   - `cloudtrail_events`
   - `vpc_subnets`
   - `vpc_network_interfaces`
   - `efs_file_systems`
1. Under **AWS Regions**, select **Add Regions** to specify the AWS Regions to query for the endpoint(s).
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. Select **Save** when finished.

## Search AWS API

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-aws).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

