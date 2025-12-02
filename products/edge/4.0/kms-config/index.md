# KMS Configuration


Cribl Edge’s Key Management Service (KMS) maintains the keys that protect sensitive information throughout your deployment. Cribl Edge offers two options for key management:
- **Internal KMS (Stream Internal)** - The default built-in key management system.
- **External KMS Providers** - Enterprise-grade options with HashiCorp Vault and AWS KMS integration (requires [Enterprise license](licensing#license-types)).

The table below summarizes the key differences between the Internal vs. External KMS.

| Feature                       | Internal KMS (Stream Internal)             | External KMS Providers (HashiCorp Vault and AWS KMS) |
|-------------------------------|--------------------------------------------|-----------------------------------------------------|
| **Master Key Storage** | Stores `cribl.secret` directly on the filesystem. | Stores `cribl.secret` in a separate key management system. |
| **Master Key Location** | In a file called `cribl.secret`.         | Completely removed from Cribl Stream’s filesystem.   |
| **License Requirement** | Available by default on all license tiers. | Requires an Enterprise license/plan.                 |
| **Management** | Managed entirely within Cribl Edge.      | Managed by the external provider (Vault or AWS).      |
| **Configuration/Secrets** | Maintained through Cribl’s Settings interface. | Configured through Cribl’s Settings interface.      |
| **Setup Complexity** | Simpler to set up.                         | More complex to set up.                              |
| **Security** | Less separation between the application and its encryption keys. | Enhanced security through separation of encryption keys from the application. |
| **Centralized Key Management** | None.                                      | Enables centralized key management across your organization. |
| **Availability** | Available by default. Unavailable if an external KMS is enabled for the same Leader Node/Fleet. | Unavailable if internal KMS is enabled for the same Leader Node/Fleet. |
| **Supported Providers** | N/A                                        | HashiCorp Vault, AWS KMS.                           |

## How Encryption Works in Cribl Edge

### The Role of `cribl.secret`

At the core of the Cribl Edge encryption system is a master encryption key called `cribl.secret`. This key is used to encrypt sensitive configuration elements such as:

- Authentication credentials in Sources and Destinations (stored in `inputs.yml` and `outputs.yml`)
- API keys and tokens
- Database connection strings (stored in `database-connections.ym`l)
- TLS certificates and associated private keys (stored in the `auth/certs` directory
- Other sensitive attributes throughout Cribl Edge
- Secrets stored in `secrets.yml`

### KMS Provider Interaction​
When using the default  Cribl Edge KMS provider:
- The `cribl.secret` value is stored directly on the filesystem in a file called `cribl.secret`.
-  Cribl Edge uses this key to encrypt sensitive information in various configuration files.

When you configure an external KMS provider (HashiCorp Vault or AWS KMS):
- The `cribl.secret` file is removed from the filesystem.
-  Cribl Edge sets and retrieves the `cribl.secret` value via the chosen external KMS provider.
-  The external KMS provider only handles the master encryption key (`cribl.secret`) itself.
-  All other sensitive information (Source/Destination credentials, TLS certificates, database connection strings, etc.) remains in the  Cribl Edge configuration files, encrypted using the externally-managed key.
-  Some sensitive configuration elements may remain in plaintext in the configuration files, depending on the specific attribute.

###  Manage KMS Across Distributed Environments
In Distributed deployments:
- Each environment (Leader Node, Fleet) can have its own KMS configuration.
- The internal KMS is unavailable for any environment where an external KMS is enabled.
- Other environments without external KMS configured will continue to use the internal KMS.
- Each environment requires its own distinct secret that cannot be shared between environments.

#### External KMS Providers and Fleets
Some guideines to keep in mind when using an external KMS provider in a Distributed deployment: 
- To integrate an external KMS provider into an on-prem Distributed deployment, the Cribl Edge Leader Node must have internet access.
- After initial license installation in Distributed mode, you may need to take the following steps to activate KMS features within Fleets:
  - Open **Settings** > **Global** > **Service Processes** > **Processes**.
  - In the list of processes, locate any process with a Role of `CONFIG_HELPER`.
  - Select the **Restart** button next to that process.
  - Upon restarting, the KMS will be available for use in the corresponding Fleet.
  
### Deployment Scenarios​
The KMS configuration varies depending on your deployment type:
- In an on-prem Single-instance deployment, you configure the KMS at **Settings** > **Global** > **Security** > **KMS** > **Configure**. 
- In a Distributed deployment, you configure the Leader's KMS at the same global location, while additional KMS configs for each Fleet are available at the Fleet's **Group Settings** > **Security** > **KMS** > **Configure** page.

### Available KMS Providers​
The KMS **Configure** drop-down currently provides these options:
- [Stream Internal](#internal): The only option available without an Enterprise license/plan. With this option, the secrets themselves are configured and maintained in Cribl [Secrets](securing-and-monitoring#secrets) section.
- [HashiCorp Vault](#vault): Stores the `cribl.secret` key securely in HashiCorp Vault, completely removing it from Cribl’s local filesystem.
- [AWS KMS](#aws): Stores the `cribl.secret` key directly within AWS Key Management Service, completely removing it from Cribl’s local filesystem.



> ##### External KMS Providers and Fleets {{< id `group-ext-kms` >}}
>
> To integrate an external KMS provider into an on-prem Distributed deployment, Cribl Edge's Leader Node must have internet access.
>
> When you initially install a license in Distributed mode, a known bug prevents immediate use of KMS features within Fleets. Here is the workaround:
>
> 1. Open **Settings** > **Global** > **Service Processes** > **Processes**.
> 1. In the list of processes, locate any process with a Role of `CONFIG_HELPER`.
> 1. Select that process' **Restart** button.
>
> Upon restarting, KMS will be available for use in the corresponding Fleet.
>
{.box .info}

## Internal KMS {#internal}

The **KMS provider** field defaults to `Stream Internal`. With this option, no further configuration here is required (or possible). See [Secrets](securing-and-monitoring#secrets) to configure individual secrets.

## HashiCorp Vault  {#vault}

Setting the **KMS provider** drop-down to `HashiCorp Vault` exposes the following configuration options:

### KMS Settings

**Vault URL**: Enter the Vault server's URL (e.g., http://localhost:8200).

### Authentication

**Auth provider**: The method for authenticating requests to Vault server. Select one of `Token`, `AWS IAM`, or `AWS EC2`. Your selection determines the remaining **Authentication** options displayed.

#### Token-based Authentication

**Token**: Enter the authentication token. This token will be used only to generate child tokens for further authentication actions.

#### AWS IAM Authentication

Use the **Authentication method** buttons to select one of the following AWS methods:

- **Auto**: Uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance. The attached IAM role grants Cribl Edge Edge Nodes access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

- **Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials directly or by reference. This is useful for Edge Nodes not in an AWS VPC, e.g., those running a private cloud. It prompts you to provide an **Access key** and a **Secret key**. 

**Vault AWS IAM Server ID**: Value to use for the `Vault-AWS-IAM-Server-ID` header value. This should match the value configured with IAM authentication on Vault.

**Vault Role**: Authentication role to use in Vault.

##### Assume Role

This section is displayed for all AWS IAM authentication methods.

**Enable for Vault Auth**: Toggle to `Yes` if you want to use your Assume Role credentials to access Vault authentication.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming the role.

#### AWS EC2 Authentication

**Vault Role**: Enter the authentication role to use in Vault.

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

### Service Configuration

**KMS Key ARN**: Enter the Amazon Resource Name (ARN) of the AWS KMS Key to use for encryption. This entry is required.
