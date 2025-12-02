# AWS Cross-Account Data Collection


Cribl uses multiple AWS accounts for different purposes, including our public [Cribl.Cloud](https://cribl.cloud) service (deployment details [here](deploy-cloud)). We built our account strategy from the ground up using the AWS Control Tower framework, as summarized in our [Logging in a Multi-Account AWS Environment](https://cribl.io/blog/logging-in-a-multi-account-aws-environment/) blog post. 

However, some organizations use a legacy account structure that doesn’t consolidate logs to a single account. This creates a challenge in collecting data or logs from peer accounts, as permission boundaries now need to be crossed. Whether you need to collect data from, or write data to, another account, the `AssumeRole` permission allows for cross-account access without the need to generate static IAM keys.

## AssumeRole Example

A usage example is a central logging account that has access to other organization accounts to consume logs, but not to perform other actions. Let’s say we have two AWS accounts, A and B. We want account A to be able to access resources in account B. We can build a policy inside account B that allows permissions to access the target resources. We can then specify, in account B, that we trust account A to be allowed to use this role. The diagram below illustrates how the `AssumeRole` action permits the Trusted Account (A)  access to resources in the Trusting Account (B).

![Using STS and temporary credentials to access an S3 bucket and SQS queue across accounts](aws-x-acct-data-collection.90a0441128.png)

Here's how Cribl Stream would work with `AssumeRole` permissions inside AWS:

1. The Cribl Stream Worker has an EC2 instance role attached.
2. The IAM role in Account A permits the EC2 instance to assume the role in Account B (and Account B trusts Account A).
3. Temporary IAM credentials are returned to the EC2 instance.
4. Cribl Stream uses the temporary IAM credentials to access the resources in Account B.

> Since this guide's original publication, we've updated `logstream` to `cribl‑stream` in example ARNs below, to match our renamed product.
>
{.box .success}

## Configure IAM AssumeRole Permissions

First, in account A, we build a policy that allows only the ability to assume the role inside account B. This policy restricts users from being able to access any resources that they don’t need to see. We can also revoke this trust relationship at any time, without having to worry about an account still having keys and (therefore) access to the data.

In our example, we want to access VPC Flow logs inside an S3 bucket in account B (ID 222222222222) from account A (ID 111111111111). We’ll start building the two policies in account B, and then move to account A.

### Account B Configuration

In account B, we build a policy to enable access to the S3 bucket (`vpc-flow-logs-for-cribl`) with the least privileges required for Cribl Stream. This policy is the one that changes depending on what you need to accomplish – e.g., reading from an S3 bucket, writing to Amazon Kinesis Data Streams, etc.

```json
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Action": "s3:GetObject",
     "Resource": "arn:aws:s3:::vpc-flow-logs-for-cribl/*"
   },
   {
     "Effect": "Allow",
     "Action": "s3:ListBucket",
     "Resource": "arn:aws:s3:::vpc-flow-logs-for-cribl"
   }
 ]
}
```

Next, we need to attach a Trust Relationship to Account B's IAM role to permit the `AssumeRole` action from account A:

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

> If you are creating a Trust Relationship for a [Cribl.Cloud](deploy-cloud) Worker managed by Cribl, you would instead paste the AWS Worker's role ARN in the format shown below. This option applies only to Cribl-managed Workers, not to hybrid Workers on customer-managed Cribl Stream instances:
>
{.box .success}

```json
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Principal": {
       "AWS": "arn:aws:iam::111111111111:role/main-default"
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

### Why an External ID Condition in the Trust Policy?

It is important to [configure an AWS External ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html#external-id-purpose), especially if you have third parties accessing your AWS accounts. The External ID protects from the [confused deputy problem](https://en.wikipedia.org/wiki/Confused_deputy_problem), where a third party obtains access through an intermediary. The External ID is not a password or secret, but it should still be protected from accidental sharing.

<div style="background-color: white;">

![Mitigating the confused deputy vulnerability](aws-x-acct-confused-deputy-mitigation-3.d5c86c76de.png)

</div>

### Account A Configuration

In account A, we next configure a new IAM role with the following policy. 

> If you are creating a Trust Relationship for a Cribl-managed [Cribl.Cloud](deploy-cloud) Worker, skip this section. Cribl has implicitly preconfigured Account A for you.
>
{.box .success}

For our example, we only want the role to be able to use the `AssumeRole` action, but you can add additional statements to meet your needs:

```json
{
 "Version": "2012-10-17",
 "Statement": {
   "Effect": "Allow",
   "Action": "sts:AssumeRole",
   "Resource": "arn:aws:iam::222222222222:role/account-b-cribl-stream-role-to-assume"
 }
}
```

Since we need our EC2 instance to be able to assume this role, we will configure the Trust Relationship as follows:

```json
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Effect": "Allow",
     "Principal": {
       "Service": "ec2.amazonaws.com"
     },
     "Action": "sts:AssumeRole"
   }
 ]
}
```

## Configure Cribl Stream

Now that our `AssumeRole` policies have been built, we can configure Cribl Stream to assume the correct role to access the resources we need. Configure your Source, Collector, or Destination with the appropriate `AssumeRole` ARN and External ID. While this screenshot shows an S3 collector specifically, all AWS sources and destinations support Assume Role functionality.

![Cribl Stream Assume Role configuration](aws-collector-config-assume-role.9d59fc9be9.png)
{border="true"}
