# instance.yml


Instance configuration is located under `$CRIBL_HOME/local/_system/instance.yml` (`C:\ProgramData\Cribl\local\_system.yml` for Cribl Edge on Windows).

```yaml {title="$CRIBL_HOME/local/_system/instance.yml or C:\ProgramData\Cribl\local\_system.yml for Cribl Edge on Windows"}
distributed:
  mode: # [string] One of: master | worker | single | edge | managed-edge.
  master: # [object]
    disableSNIRouting: # [boolean] Whether SNI routing is enabled. See (Environment Variables)[environment-variables#cribl_dist_leader_url] before changing this setting.
    host: # [string] Instance host address.
    port: # [number] Instance post number.
    tls: # [object]
      disabled: # [boolean]
      certificateName: # [string] TLS certificate name.
      rejectUnauthorized: # [boolean] False if ignoring untrusted certificates.
      requestCert: # [boolean] Whether certificates are required and validated.
      privKeyPath: # [string] Path to private key file. Can reference $ENV_VARS.
      certPath: # [string] Path to certificate file. Can reference $ENV_VARS.
      caPath: # [string] Path to CA certificate file. Can reference $ENV_VARS.
      commonNameRegex: # [any] Allowed common names in peer certificates' subject (regex).
      # Available values: 'TLSv1.3' | 'TLSv1.2' | 'TLSv1.1' | 'TLSv1'
      minVersion: # [string] Minimum TLS version.
      maxVersion: # [string] Maximum TLS version.
      servername: # [string] Server name.
    resiliency: # [string] Preferred failover mode, one of: none | failover.
    ipWhitelistRegex: # [string] IP addresses allowed to send data (regex).
    maxActiveCxn: # [number] Max number of active connections allowed per Worker Process (0 for unlimited).
    authToken: # [string] Token used for communication between Leader and managed nodes. If empty, unauthorized access is permitted.
    compression: # [string] Codec used to compress persisted data. One of: none | gzip.
    connectionTimeout: # [number] Maximum time to wait for a connection to complete successfully.
    writeTimeout: # [number] Time (ms) to wait for a write to complete before assuming connection is dead.
    configBundles: # [object]
      remoteUrl: # [string] Bucket to use for remote bundle storage, in s3://${bucket} format. Supported format expression: ^s3://[a-z0-9.-]{3,63}$
  group: # [string] Group the managed node belongs to.
  envRegex: # [string] Regex used to filter env variable names that can be sent by managed node to Leader over heartbeat.
  tags: # [array of strings] Tags associated with managed node, can be used by Leader for mapping.
```
