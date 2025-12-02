# mode-master


Configures instance as a Leader.

For more information, see:

- [Leader High Availability/Failover](deploy-add-second-leader)
- [Set Up Leader and Worker Nodes](setting-up-leader-and-worker-nodes)

**Usage:**

```text
./cribl mode-master -r failover -v /tmp/shared
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d <deploymentId>` | Deployment ID for reporting telemetry on multiple deployments. |
| `-H <host>` | Hostname/IP. Defaults to `0.0.0.0`. |
| `-p <port>` | Port. Defaults to `4200`. |
| `-f <uiAccess>` | Controls whether the distributed port responds to requests on behalf of the API process. The default port is `4200`. When disabled, `4200` only responds to Worker/Edge Node queries for config bundles and distributed upgrades. Accepts: ["true", "false"]. Defaults to `true` for backward compatibility. |
| `-n <certName>` | Name of the saved certificate to use. Mutually exclusive with -k or -c. |
| `-k <privKeyPath>` | Path on server to the private key to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-c <certPath>` | Path on server to the certificate to use. Accepts PEM format. Can reference `$ENV_VARS`. |
| `-u <authToken>` | Authentication token to include as part of the connection header. |
| `-S <tls>` | Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"]. |
| `-i <ipWhitelistRegex>` | Regex matching IP addresses that are allowed to establish a connection. |
| `-r <resiliency>` | Resiliency mode for the Leader. Accepts: ["none", "failover"]. |
| `-v <failoverVolume>` | For resiliency=failover, the volume to use as shared storage. |