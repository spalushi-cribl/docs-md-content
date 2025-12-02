# mode-managed-edge


Configures instance as a Cribl Edge Node.

Running this command resets all the related options. This means that even if you want to change only one option, you also need to provide values for all the other arguments.

When you [generate bootstrap scripts](/edge/managing-edge-nodes#bootstrap) via the UI to add or update Workers, these scripts automatically set the `-S` option according to the [Leader TLS configuration](securing-communications#tls-leader).

**Usage:**

```text
./cribl mode-managed-edge 127.0.0.1 -p 9420 -u myToken && ./cribl restart
```

> The `-H <host> -p <port> -u <authToken>` parameters  are required for successful connection to the Leader.
>
{.box .info}

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d <deploymentId>` | Deployment ID for reporting telemetry on multiple deployments. |
| `-H <host>` | Leader's hostname or IP address. Required. |
| `-p <port>` | Leader's cluster communications port Example: `4200`. Required. |
| `-n <certName>` | Name of the saved certificate to use. Mutually exclusive with `-k` or `-c`. |
| `-k <privKeyPath>` | Path on server to the private key to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-c <certPath>` | Path on server to the certificate to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-u <authToken>` | Authentication token to include as part of the connection header. Required. |
| `-S <tls>` | Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"]. |
| `-e <envRegex>` | Regex to select environment variables to report to Leader. Example: `"/test/"`. |
| `-t <tags>` | Tag values to report to Leader. |
| `-g <group>` | Fleet to report to Leader. |

In Cribl.Cloud, get the token for your Workspace from the Worker Node modal (**Add/Update Worker Node**) from the **Auth token** field.

In an on-prem deployment, get the token from **Settings** > **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Auth token**.
