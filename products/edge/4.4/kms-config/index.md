# KMS Configuration


Cribl Edge’s Key Management Service (KMS) maintains the keys that Cribl Edge uses to encrypt secrets on Fleets and Edge Nodes. The internal KMS is always available, but integrating an external KMS provider requires an Enterprise or Standard [license](licensing).

> To configure KMS for Cribl Edge in Cribl.Cloud, see the [Launch Guide](cloud-vs-self-hosted#access). 
>
{.box .cloud}

In an on-prem single-instance deployment, you configure the KMS at **Settings** > **Global Settings** > **Security > KMS**. In a distributed deployment, you configure the Leader's KMS at the same global location, while additional KMS configs for each Fleet are available at the Fleet's **Group Settings** > **Security > KMS** page.

The resulting **KMS Provider** drop-down currently provides these options: 

- [Stream Internal](#internal): The **only** option available without an Enterprise license/plan. With this option, the secrets themselves are configured and maintained in Cribl Edge Settings' parallel [Secrets](securing-and-monitoring#secrets) section.
- [HashiCorp Vault](#vault)
- [AWS KMS](#aws)



> ##### External KMS Providers and Fleets {{< id `group-ext-kms` >}}
>
> To integrate an external KMS provider into an on-prem distributed deployment, Cribl Edge's Leader Node must have internet access.
> 
> When you initially install a license in distributed mode, a known bug prevents immediate use of KMS features within Fleets. Here is the workaround: 
> 
> 1. Open **Settings** > **Global Settings** > **Worker Processes**.
> 1. In the list of processes, locate any process with a Role of `CONFIG_HELPER`. 
> 1. Click that process' **Restart** button. 
> 
> Upon restarting, KMS will be available for use in the corresponding Fleet.
>
{.box .info}

## Internal KMS {#internal}

The **KMS provider** field defaults to `Stream Internal`. With this option, no further configuration here is required (or possible). See [Secrets](securing-and-monitoring#secrets) to configure individual secrets.

## HashiCorp Vault {#vault}

Setting the **KMS provider** drop-down to `HashiCorp Vault` exposes the following configuration options:

### KMS Settings

**Vault URL**: Enter the Vault server's URL (e.g., `http://localhost:8200`).

**Namespace**: If you are using HashiCorp Vault Enterprise [namespaces](https://developer.hashicorp.com/vault/docs/enterprise/namespaces), enter the desired namespace. 

### Authentication

**Auth provider**: The method for authenticating requests to HashiCorp Vault server. Select one of `Token`, `AWS IAM`, or `AWS EC2`. Your selection determines the remaining **Authentication** options displayed.

#### Token-based Authentication

**Token**: Enter the authentication token. This token will be used only to generate child tokens for further authentication actions.

#### AWS IAM Authentication

> In HashiCorp Vault, the term "method" can refer to `userpass`, `token`, or `aws`, among others, but the `aws` [method](https://developer.hashicorp.com/vault/docs/auth/aws) supports two authentication types: `iam` and `ec2`. Meanwhile, in Cribl Edge, you'll see "method" used differently, e.g. in the **Authentication method** setting described below.
>
{.box .info}

Use the **Authentication method** buttons to select one of the following:

- **Auto**: Uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance, local credentials, sidecar, or other source. The attached IAM role grants Cribl Edge Edge Nodes access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

- **Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials directly or by reference. This is useful for Edge Nodes not in an AWS VPC, e.g., those running in a private cloud. It prompts you to provide an **Access key** and a **Secret key**.

**Vault AWS IAM Server ID**: Value to use for the `Vault-AWS-IAM-Server-ID` header value. This should match the value configured with IAM authentication on Vault.

**Vault Role**: Authentication role to use in Vault.

**Custom auth path**: If you enabled [authentication in HashiCorp Vault](https://developer.hashicorp.com/vault/docs/commands/auth/enable) with a custom path, enter that path again here.

For example:
* If you enabled authentication with the HashiCorp Vault command `vault auth enable -path /my-auth aws` instead of `vault auth enable aws` you would set a custom path of `my-auth`. Subsequently, when you perform actions using the `vault write` command, you'd specify an auth type with the `auth_type=ec2` or `auth_type=iam` options. 

##### Assume Role

This section is displayed for all AWS IAM authentication methods.

**Enable for Vault Auth**: Toggle to `Yes` if you want to use your Assume Role credentials to access Vault authentication.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming the role.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

#### AWS EC2 Authentication

**Vault Role**: Enter the authentication role to use in Vault.

**Custom auth path**: If you enabled [authentication in HashiCorp Vault](https://developer.hashicorp.com/vault/docs/concepts/auth) with a custom path, enter that path again here.
For example:
* You could have used the HashiCorp Vault command `vault auth enable -path /my-auth aws` to enable authentication with a custom path of `my-auth`. Subsequently, when you perform actions using the `vault write` command, you'd specify an auth type with the `auth_type=ec2` or `auth_type=iam` options.

### Secret Engine

**Mount**: Mount point of the Vault secrets engine to use. (Currently, only the KVv2 engine is supported.) Defaults to `secret`.

**Secret path**: Enter the path on which the Cribl Edge secret should be stored, e.g.: `<somePath>/cribl‑secret`.

> In a distributed deployment, the Leader, and each Fleet, require a distinct secret. This location cannot be shared between them.
>
{.box .warning}

### Advanced

**Enable health check**: Whether to perform a health check before migrating secrets data. Defaults to `Yes`.

**Health check endpoint**: Configurable endpoint to use for validating system health. Defaults to `/v1/sys/health`.

## AWS KMS {#aws}

Setting the **KMS provider** drop-down to `AWS KMS` exposes the following configuration options:

### Authentication

**Authentication method**: Select an AWS authentication method.

- **Auto**: This default option uses the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, or the attached IAM role. Works only when running on AWS.

- **Manual**: You must select this option when not running on AWS.

The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to `env.AWS_ACCESS_KEY_ID`, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to `env.AWS_SECRET_ACCESS_KEY`, or to the metadata endpoint for IAM credentials.

### Assume Role

**Enable for KMS**: Toggle to `Yes` if you want to use Assume Role credentials to access the AWS KMS.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role. This is required only when assuming a role that requires this ID in order to delegate third-party access. For details, see [AWS' documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html).

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Service Configuration

**KMS Key ARN**: Enter the Amazon Resource Name (ARN) of the AWS KMS Key to use for encryption. This entry is required.

When you configure your IAM account/role in AWS, grant access to the following permissions on the KMS key that will be used:

- `kms:Encrypt`
- `kms:Decrypt`

Then use that account for authentication.