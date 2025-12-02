# mode-outpost


Configures instance as a Cribl Outpost.

For more information, see:

- [Cribl Outpost](outpost)
- [Set up Outpost](set-up-outpost)

**Usage:**

```shell
./cribl mode-outpost \
  -H leader.example.com \
  -p 4200 \
  -u myToken \
  -S 1 \
  -O 'tls://0.0.0.0:4200?tls.certPath=/opt/certs/outpost.crt&tls.privKeyPath=/opt/certs/outpost.key'
```

> The `-H <host> -p <port> -u <authToken> -O <outpostUrl>` parameters are required for successful connection to the Leader.
>
{.box .info}

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d <deploymentId>` | Deployment ID for reporting telemetry on multiple deployments. |
| `-H <host>` | Leader's hostname or IP address. |
| `-p <port>` | Leader's cluster communications port. |
| `-n <certName>` | Name of the saved certificate to use. Mutually exclusive with -k or -c. |
| `-k <privKeyPath>` | Path on server to the private key to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-c <certPath>` | Path on server to the certificate to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-u <authToken>` | Authentication token to include as part of the connection header. Required. |
| `-S <tls>` | Sets, or disables, secure communication between Leader and the Outpost via TLS. Accepts: ["true", "false"]. |
| `-e <envRegex>` | Regex to select environment variables to report to Leader. Example: `"/test/"`. |
| `-t <tags>` | Tag values to report to Leader. |
| `-O <outpostUrl>` | URL to configure the Outpost listener. Takes the same format as [`CRIBL_DIST_LEADER_URL`](environment-variables#cribl_dist_leader_url). Example: `'tls://0.0.0.0:4200?tls.certPath=/opt/certs/outpost.crt&tls.privKeyPath=/opt/certs/outpost.key'`. |

In Cribl.Cloud, get the token for your Workspace from the Edge Node modal (**Add/Update Edge Node**) from the **Auth token** field.

In an on-prem deployment, get the token from **Settings** > **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Auth token**.
