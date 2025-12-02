# cloud-workspace


Updates an on-prem Cribl Edge Leader with a new config in the `instance.yml` file called `cloudWorkspace`.
With this config, the local Cribl Edge Leader will connect to the Cribl.Cloud Leader and send it usage metrics.

**Usage:**

```text
./cribl cloud-workspace -H <host> -p <port> # etc: flags
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d <deploymentId>` | Deployment ID used to report telemetry on multiple deployments. |
| `-H <host>` | Hostname/IP. Defaults to `0.0.0.0`. |
| `-p <port>` | Port. Defaults to `4200`. |
| `-n <certName>` | Name of the saved certificate to use. Mutually exclusive with `-k` or `-c`. |
| `-k <privKeyPath>` | Path on server to the private key to use. PEM format. Can reference `$ENV_VARS`. |
| `-c <certPath>` | Path on server to the certificate to use. PEM format. Can reference `$ENV_VARS`. |
| `-u <authToken>` | Authentication token to include as part of the connection header. |
| `-S <tls>` | Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"]. |
| `-i <ipWhitelistRegex>` | Regex matching IP addresses that are allowed to establish a connection. |
| `-r <resiliency>` | Resiliency mode on the Leader. Accepts: ["none", "failover"]. |
| `-v <failoverVolume>` | For resiliency=failover, the volume to use as shared storage. |