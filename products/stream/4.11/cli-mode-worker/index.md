# mode-worker


Configures Cribl Stream as a Worker instance.

Running this command resets all the related options. This means that even if you want to change only one option, you also need to provide values for all the other arguments.

When you [generate bootstrap scripts](/stream/deploy-workers#ui) through the UI to add or update Workers, these scripts automatically set the `-S` option according to the [Leader TLS configuration](securing-communications#tls-leader).

For more information, see [Set Up Leader and Worker Nodes](setting-up-leader-and-worker-nodes).

**Usage:**

```text
./cribl mode-worker -H 127.0.0.1 -p 9420
```

> The `-H <host> -p <port>` parameters are required.
>
{.box .info}

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d <deploymentId>` | Deployment ID for reporting telemetry on multiple deployments. |
| `-H <host>` | Leader's hostname or IP address. Required. |
| `-p <port>` | Leader's cluster communications port. Example: `4200`.  Required. |
| `-n <certName>` | Name of the saved certificate to use. Mutually exclusive with `-k` or `-c`. |
| `-k <privKeyPath>` | Path on server to the private key to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-c <certPath>` | Path on server to the certificate to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-u <authToken>` | Authentication token to include as part of the connection header. |
| `-S <tls>` | Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"]. |
| `-e <envRegex>` | Regex to select environment variables to report to Leader. For example: `"/test/"`. |
| `-t <tags>` | Tag values to report to Leader. |
| `-g <group>` | Worker Group to report to Leader. |