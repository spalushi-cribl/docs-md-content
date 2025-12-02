# leader.yml


`leader.yml` contains configuration for a secondary Leader
and is located under `$CRIBL_HOME/local/cribl` in the volume of that Leader.

This configuration file is created when you [have configured a Leader for failover](deploy-add-second-leader#leader-settings)
and the new Leader becomes active in case of failover.



```yaml {title="leader.yml"}
# Host/IP address to bind to - The host address for the instance
# [string; default: 0.0.0.0; required]
host:
# Port number to bind to - The port number for the instance
# [number; default: 4200; required]
port:
# Protocol - Communication protocol
# [string]
protocol:
# Auth Token - Authentication token for secure communication
# [string]
authToken:
# IP allowlist regex - Regex matching IP addresses that are allowed to establish a connection
# [string; default: /.*/]
ipWhitelistRegex:
# Proxy settings - SOCKS proxy configuration
proxy:
# Compression - Data compression method
# [string]
compression:
# Connection timeout - Timeout for establishing connections
# [number]
connectionTimeout:
# Write timeout - Timeout for write operations
# [number]
writeTimeout:
# Max buffer bytes - Maximum buffer size in bytes
# [number]
maxBufferBytes:
# Forward to Leader API - Controls whether distributed traffic gets proxied to Leader's API server
# [boolean; default: true]
forwardToLeaderApi:
# TLS configuration - TLS/SSL settings
tls:
  # Disabled - Whether TLS is disabled
  # [boolean]
  disabled:
  # Certificate Name - Name of the certificate to use
  # [string]
  certificateName:
  # Private Key Path - Path to private key file
  # [string]
  privKeyPath:
  # Passphrase - Passphrase for private key
  # [string]
  passphrase:
  # Certificate Path - Path to certificate file
  # [string]
  certPath:
  # CA Path - Path to certificate authority file
  # [string]
  caPath:
  # Request Certificate - Whether to request client certificates
  # [boolean]
  requestCert:
  # Reject Unauthorized - Whether to reject unauthorized certificates
  # [boolean]
  rejectUnauthorized:
  # Common Name Regex - Regex for validating certificate common name
  commonNameRegex:
  # Min Version - Minimum TLS version
  minVersion:
  # Max Version - Maximum TLS version
  maxVersion:
  # Server Name - TLS server name for verification
  # [string]
  servername:
# Helper processes socket dir - Directory to hold sockets for inter-process communication (IPC) between
# Leader and processes like Config Helpers and services. Defaults to your system's temp directory.
# [string]
configHelperSocketDir:
# Active connection limit - Maximum number of active connections allowed from Worker Nodes. Use 0 for
# unlimited.
# [number; min: 0; default: 0]
maxActiveCxn:
# Resiliency - Enable or disable failover
# [string; default: none]
configBundles:
# Disable SNI Routing - Whether to disable SNI-based routing
# [boolean]
disableSNIRouting:
```
