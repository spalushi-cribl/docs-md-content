# Grant Access to AWS


Allow Cribl Search to access to your AWS data, by configuring an IAM role or user. See various trust policy examples.

---
  
Cribl Search uses Amazon Web Services (AWS) IAM to access and search your data in AWS. When creating an Amazon [Dataset Provider](set-up-s3#provider) in Cribl Search, you'll need to provide it with credentials.

Access to AWS services is managed through [AWS Identity and Access Management (IAM)](http://aws.amazon.com/iam/). You can grant access either at the [IAM role](#role) level, or at the [IAM user](#user) level using an access and secret key pair. Cribl recommends role-based access, as described below.

## IAM User {#user}

You can use an IAM user's [access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) (access key IDs and secret access keys) to grant access to your data in AWS. If [creating a new IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html), give it **Programmatic access** so AWS assigns the user access keys.

## IAM Role {#role}

Cribl recommends using IAM role–based access for increased security. An IAM role is similar to a user – it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of passing IAM user credentials, here a user assumes a role and triggers AWS to dynamically generate [temporary security credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html). With these temporary credentials, the user then accesses an AWS resource by its Amazon Resource Name (ARN).

> For details about configuring the IAM role in the AWS console, see [Create a role using custom trust policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-custom.html). For details about using permissions policies to restrict an IAM role's capabilities on AWS, see [Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html).
>
{.box .success}

The process for granting access is:

1. Set up a new AWS IAM role in your account.

  > You need to either build the [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) and policy using your own infrastructure-management solution (such as Terraform or CloudFormation), or else build it via the AWS console. 
  
  > You can create a new role with Cribl's [trust policy](#trust) instead of attaching it – see the next step. 
  
2. Attach the [trust policy](#trust), provided by Cribl, to the role in your account.

> [Modify your IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) to include Cribl Search's trust policy. You can do this with the AWS [console](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-managingrole_edit-trust-policy), [CLI](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-cli.html#roles-managingrole_edit-trust-policy-cli), or [API](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-api.html#roles-managingrole_edit-trust-policy-api).

3. Add the appropriate permissions to your IAM role using an [identity-based policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html). For example, to allow access to an S3 bucket called `cribl-search-data`, you'd use the following sample **permission policy**:

     ```json
     {
   	"Version": "2012-10-17",
   	"Statement": [{
   			"Effect": "Allow",
   			"Action": "s3:GetObject",
   			"Resource": "arn:aws:s3:::cribl-search-data/*"
   		},
   		{
   			"Effect": "Allow",
   			"Action": "s3:ListBucket",
   			"Resource": "arn:aws:s3:::cribl-search-data"
   		}
   	]
     }
     ```

4. Specify the IAM role's ARN on an AWS Dataset Provider.

## Trust Policy {#trust}

Cribl Search builds a trust policy that creates a trust relationship between Cribl Search and a role in your AWS account. This policy controls and restricts access to only those resources to which you grant access. This trust relationship can be revoked at any time, without having to also revoke credentials (and thus, revoke access to the data).

The provided trust policy contains the AWS Role ARN and [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) of Cribl Search's IAM role that you'll need to trust. The policy allows Cribl Search to perform the `sts:AssumeRole` operation toward your external account. 

The external ID is a custom string that you create to enhance authentication and authorization when accessing the S3 resource. The crucial requirement is to ensure that the strings match on both the Cribl Dataset Provider the and AWS trust policy.

> The external ID must meet the length and character requirements that AWS specifies in [IAM name requirements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-quotas.html#reference_iam-quotas-names).
>
{.box .info}

### Example Trust Policy {#trust-example}

Here is a basic trust policy – an anonymized version of the policy you can copy
from your Dataset Provider modal's **Trust Policy** button (when setting up the
[Authentication Method](set-up-s3#auth)). See also the variations below the example.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "<EXTERNAL_ID>"
        }
      }
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:TagSession"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:SetSourceIdentity"
    }
  ]
}
```

### Trust Policy with Multiple Workspaces {#workspaces}

If you have configured [multiple workspaces](/stream/workspaces), replace `<TENANT_ID>:role/search-exec-main` above with `<TENANT_ID>:role/search-exec-<WORKSPACE-NAME>`, as needed.

Alternatively, you can use this wildcard syntax to enable searching all workspaces. But use wildcards with caution, to avoid creating an overly permissive policy:

```json
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
```

### Trust Policy with ABAC Tagging {#trust-vpce}

You can make your AssumeRole sessions more secure by adding Cribl's VPCE (Virtual Private Cloud Endpoint) ID to your trust policy. This enables attribute-based access control (ABAC).

> For an overview of how ABAC works on AWS, with links to further resources, see [Define permissions based on attributes with ABAC authorization](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html).
>
{.box .success}

To set up ABAC on a supported Dataset Provider: After you enable the **Apply ABAC source‑ip tagging** option, copy the adjacent **VPCE ID** to use later in this section. For convenience, we'll start with the default trust policy shown [above](#trust-example):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "sts:ExternalId": "<EXTERNAL_ID>"
        }
      }
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:TagSession"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<TENANT_ID>:role/search-exec-main"
      },
      "Action": "sts:SetSourceIdentity"
    }
  ]
}
```

#### Expand the Trust Policy {#trust-vpce-condition}

First, expand the `"Action": "sts:TagSession"` element by adding the sub-element shown below. Paste in the `"${aws:SourceIp}"` variable as-is. This enables passing Cribl Search's `source‑ip` value, making your sessions more secure by enforcing a condition that the `source‑ip` tag must match the IP address requesting to assume the role:

```json
   "Action": "sts:TagSession",
   "Condition": {
       "StringEquals": {
           "aws:RequestTag/source-ip": "${aws:SourceIp}"
       }
   }
```

#### Expand the Permission Policy {#permission-vpce-condition}

For the remaining steps, we'll start with this anonymized default policy that
you can copy from your Dataset Provider modal's **Permission Policy** button
(when setting up the [Authentication Method](set-up-s3#auth)).

```json
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "s3:GetObject",
        "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>/*"]
      },
      {
        "Effect": "Allow",
        "Action": "s3:ListBucket",
        "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"]
      }
    ]
  }
```

Expand the `s3:ListBucket` element by adding the sub-element shown below. Replace `vpce-<someVcpeId>` with the VPCE ID that you copied from your Dataset Provider's config modal:

```json
      {
       "Effect": "Allow",
       "Action": "s3:ListBucket",
       "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"],
       "Condition": {
           "StringEquals": {
               "aws:SourceVpce": "vpce-<someVcpeId>"
           }
       }
      }
``` 

Finally, copy the whole expanded `s3:ListBucket` element that you just modified. In the duplicated element shown below, paste in the `"${aws:SourceIp}"` variable as-is.

```json
      {
       "Effect": "Allow",
       "Action": "s3:ListBucket",
       "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"],
       "Condition": {
           "StringEquals": {
               "aws:SourceVpce": "vpce-<someVcpeId>"
           }
       }
      },
      {
       "Effect": "Allow",
       "Action": "s3:ListBucket",
    "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"],
       "Condition": {
           "StringEquals": {
               "aws:PrincipalTag/source-ip": "${aws:SourceIp}"
           }
       }
      }
```

#### Complete Permission Policy {#permission-vpce-complete}

To wrap up, you'll end up with this expanded permission policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
      {
        "Effect": "Allow",
        "Action": "s3:GetObject",
        "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>/*"]
      },
      {
       "Effect": "Allow",
       "Action": "s3:ListBucket",
       "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"],
       "Condition": {
           "StringEquals": {
               "aws:SourceVpce": "vpce-<someVcpeId>"
           }
       }
      },
      {
       "Effect": "Allow",
       "Action": "s3:ListBucket",
    "Resource": ["arn:aws:s3:::<MY_S3_BUCKET>"],
       "Condition": {
           "StringEquals": {
               "aws:PrincipalTag/source-ip": "${aws:SourceIp}"
           }
       }
      }
  ]
}
```
