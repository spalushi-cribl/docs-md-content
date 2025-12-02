# Create and Manage Secrets in Cribl Stream


The Cribl Stream secrets store enables you to centrally manage secrets that Cribl Stream instances use to authenticate on integrated services. Use the **Manage Secrets** page to create and update authorization tokens, username/password combinations, and API‑key/secret‑key combinations for reuse across the application. These defined secrets can then be programmatically accessed and used within your Cribl Stream configurations using the [C.Secret Function](expressions-other#secret).

## Access Secrets {#get-secret}

- In a single-instance deployment, select **Settings** > **Security > Secrets**.
- In a distributed deployment with one Worker Group, select **Configure > Settings > Security > Secrets**.
- In a distributed deployment with multiple Worker Groups, secrets are managed on each Worker Group. Select **Groups >** `<group-name>` **Settings > Security > Secrets**.

On the **Manage Secrets** page, you can configure existing secrets, and/or click **New Secret** to define new secrets.

## Add New Secret {#add-secret}

The **New Secret** modal provides the following controls:

- **Secret name**: Enter an arbitrary, unique name for this secret.

- **Secret type**: Some options for this field expose additional controls, as described [below](#ssshhh).

- **Description**: Optionally, enter a description summarizing the purpose of the secret.

- **Tags**: Optionally, enter one or multiple tags related to this secret.

###  Secret Type  {#ssshhh}

This drop-down offers the following types:

- **Text**: This default type exposes a **Value** field where you directly enter the secret.

- **API key and secret key**: Exposes **API key** and **Secret key** fields, used to retrieve the secret from a secure endpoint. This is the only secret type that Cribl Stream supports for its AWS-based Sources, Collectors, and Destinations, and its Google Cloud Storage Destination.

- **Username with password**: Exposes **Username** and **Password** fields, which you fill to retrieve the secret using Basic Authentication.

## Encrypt Secrets Using the Command Line

You also have the option to create and [encrypt](cli-reference#encrypt) sensitive values using the same mechanism Cribl Stream employs internally for all sensitive fields. This ensures consistency and leverages the built-in encryption capabilities within Cribl Stream.

This approach allows you to store all your secrets securely in a password store, keeping them out of your Git repository, and adheres to strict organizational security standards that prohibit storing secrets in version control.

> Modifying credentials directly through the Cribl Stream UI won't be possible, even in development environments. This approach prioritizes security by keeping secrets out of the UI altogether.
>
{.box .warning}

For details, see [Encrypt](cli-reference#encrypt) in the CLI Reference doc.