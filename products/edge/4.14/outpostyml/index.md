# outpost.yml


`outpost.yml` contains configuration for a [Cribl Outpost](/edge/outpost) instance.

```yaml {title="outpost.yml"}
# settings for the internal distributed-management interface
listener:
  # Host - Host/IP address to bind to
  # [string; required]
  host:
  # Port - Port number to bind to
  # [number; required]
  port:
  # IP Whitelist Regex - Regex to match incoming connections' source IPs
  # [string]
  ipWhitelistRegex:
  # Max Active Connections - Maximum number of active connections to accept
  # [number]
  maxActiveCxn:
  # TLS Settings - TLS configuration for secure connections
  tls:
    # Disabled - Whether TLS is disabled for this connection
    # [boolean]
    disabled:
    # Certificate Path - Path to TLS certificate file
    # [string]
    certPath:
    # Private Key Path - Path to TLS private key file
    # [string]
    privKeyPath:
    # CA Path - Path to certificate authority file
    # [string]
    caPath:
    # Server Name - TLS server name for verification
    # [string]
    servername:
    # Reject Unauthorized - Whether to reject unauthorized certificates
    # [boolean]
    rejectUnauthorized:
    # Request Certificate - Whether to request client certificates
    # [boolean]
    requestCert:
```