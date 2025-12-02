# cribl.yml


`cribl.yml` contains settings for configuring API and other system properties.

```yaml {title="cribl.yml"}
# API configuration - API endpoint settings
# [required]
api:
  # Host - Hostname or address to bind API server to. Defaults to 0.0.0.0. Using $CRIBL_API_HOST
  # overrides this setting.
  # [string; required]
  host:
  # Port - API port to listen to. Defaults to 9000. Using $CRIBL_API_PORT overrides this setting.
  # [number; required]
  port:
  # Protocol - Protocol that API server speaks, defaults to http1.1
  # [string; default: http1.1]
  protocol:
  # Retry count - Number of times to retry binding to API port
  # [number; default: 120]
  retryCount:
  # Retry period - Period, in seconds, between consecutive port binding retries
  # [number; default: 5]
  retrySleepSecs:
  # URL base path - URL base path from which to serve all assets (useful when behind a proxy)
  # [string]
  baseUrl:
  # Local UI access - Enable to allow direct browser access to the Cribl nodes' UI
  # [boolean; default: false]
  disabled:
  # Listen on port - Expose the API service to the network on the configured port.
  # [boolean; default: true]
  listenOnPort:
  # Worker remote access - Enable remote access to Worker nodes
  # [boolean]
  workerRemoteAccess:
  # Revoke on role change - Revoke tokens when user role changes
  # [boolean]
  revokeOnRoleChange:
  # Auth token TTL - Authentication token time to live
  # [string]
  authTokenTTL:
  # Idle session TTL - Session idle timeout
  # [string]
  idleSessionTTL:
  # Login rate limit - Rate limit for login attempts
  # [string]
  loginRateLimit:
  # SSO rate limit - Rate limit for SSO attempts
  # [string]
  ssoRateLimit:
  # Headers - Custom headers to add to API responses
  headers:
  # API cache - Enable API response caching
  # [boolean]
  apiCache:
  # Scripts - Enable script execution in API
  # [boolean]
  scripts:
  # Sensitive fields - List of fields to treat as sensitive
  sensitiveFields:
  # SSL - SSL configuration for API
  # [required]
  ssl:
    # Disabled - Whether SSL is disabled
    # [boolean; default: true]
    disabled:
    # Certificate name - The name of the predefined certificate
    # [string]
    certificateName:
    # Private key path - Path on server in which to find the private key to use. PEM format. Can
    # reference $ENV_VARS.
    # [string]
    privKeyPath:
    # Passphrase - Passphrase to use to decrypt private key
    # [string]
    passphrase:
    # Certificate path - Path on server in which to find certificates to use. PEM format. Can
    # reference $ENV_VARS.
    # [string]
    certPath:
    # CA certificate path - Path on server where to find CA certificates to use. PEM format. Can
    # reference $ENV_VARS.
    # [string]
    caPath:
# Support configuration - Support settings
support:
  # Feature flag overrides - Override feature flags
  featureFlagOverrides:
    # Flag ID - The feature flag identifier
    # [string; required]
    flagId:
    # Disabled - Whether the flag is disabled
    # [boolean; required]
    disabled:
# Authentication settings - Authentication configuration
auth:
  # Type - Select from this list of supported authentication providers
  # One of: local | splunk | ldap | openid | saas | saml
  # [string; required]
  type:
  # LDAP servers - LDAP server configuration (ldap type only)
  ldapServers:
  # Bind DN - Distinguished name for LDAP binding (ldap type only)
  # [string]
  bindDN:
  # Bind credentials - Password for LDAP binding (ldap type only)
  # [string]
  bindCredentials:
  # Username field - LDAP field for username (ldap type only)
  # [string]
  usernameField:
  # Search base - LDAP search base (ldap type only)
  # [string]
  searchBase:
  # Groups field - Field containing group information (ldap type only)
  # [string]
  groupsField:
  # Fallback - Allow login as Local User
  # [boolean]
  fallback:
# System settings
system:
  # Upgrade mode - How system upgrades are handled
  # One of: api | auto | false
  # [string]
  upgrade:
  # Restart mode - How system restarts are handled
  # One of: api | false
  # [string]
  restart:
  # Install Type - Installation type of the system
  # One of: splunk-app | standalone
  # [string]
  installType:
# Worker settings - Configuration for worker processes
workers:
  # Worker Count - Number of worker processes to spawn, if less than 1 the value is added to CPU
  # count
  # [number; default: 1]
  count:
  # Worker Memory - Memory allocation per worker process in MB
  # [number; min: 1024; default: 2048]
  memory:
# TLS settings - Global TLS configuration
tls:
  # Minimum TLS version - Minimum TLS version. Defaults to TLS 1.2.
  # One of: TLSv1 | TLSv1.1 | TLSv1.2 | TLSv1.3
  # [string]
  minVersion:
  # Maximum TLS version - Maximum TLS version. Defaults to TLS 1.3.
  # One of: TLSv1 | TLSv1.1 | TLSv1.2 | TLSv1.3
  # [string]
  maxVersion:
  # Default cipher list - Default suite of enabled and disabled TLS ciphers
  # [string]
  defaultCipherList:
  # ECDH curve - The curve name, or a colon-separated list of curve NIDs or names, to use for ECDH key
  # agreement. For example: 'P‑521:P‑384:P‑256'. Defaults to 'auto'.
  # [string; default: auto]
  defaultEcdhCurve:
  # Validate server certs - Validate server certificates globally. Disable to allow self-signed
  # certificates.
  # [boolean; default: true]
  rejectUnauthorized:
# Proxy settings - Proxy configuration
proxy:
  # Use Environment Variables - Whether to use environment variables for proxy configuration
  # [boolean; default: true]
  useEnvVars:
# Git settings - Git repository configuration
git:
  # Branch - The branch to track in your Stream deployment's git repository
  # [string; default: master]
  branch:
  # GitOps workflow - The GitOps workflow for managing Cribl configuration
  # One of: none | push
  # [string; default: none]
  gitOps:
  # Collapse actions - Collapse Commit & Deploy into a single action. If you've configured a remote,
  # Commit & Git Push will also be collapsed. Your default commit message below will be used for all
  # commits.
  # [boolean]
  commitDeploySingleAction:
  # Default commit message - Enter a default message to use for all commits
  # [string]
  defaultCommitMessage:
  # Remote URL - Git remote repository URL
  # [string]
  remote:
  # Git authentication type - Type of authentication for git operations
  # One of: none | ssh | basic
  # [string; default: ssh]
  authType:
  # Scheduled global actions - Global git actions to run automatically on a schedule
  # One of: none | commit | push | commitPush
  # [string; default: none]
  autoAction:
  # Git timeout - Maximum time (in milliseconds) to wait for git processes before ending them. Enter
  # 0 to wait indefinitely.
  # [number; default: 60000]
  timeout:
  # Auto git commit messages - Enable automatic generation of git commit messages
  # [boolean]
  copilotAutoGitCommitMessages:
# FIPS Mode - Enable FIPS compliance mode
# [boolean]
fips:
```

{{< id `example` >}} Example `cribl.yml`: 

```yaml {title="$CRIBL_HOME/default/cribl/cribl.yml"}
api:
  host: 0.0.0.0
  port: 9000
  disabled: false
  loginRateLimit: 2/second
  ssoRateLimit: 2/second
auth:
  type: local
kms.local:
  type: local
crypto:
  keyPath: $CRIBL_HOME/local/cribl/auth/keys.json
system:
  upgrade: api
  restart: api
  installType: standalone
  intercom: true
upgradeSettings:
  disableAutomaticUpgrade: true
  enableLegacyEdgeUpgrade: false
workers: 
  count: -2
  minimum: 2
  memory: 2048
proxy:
  useEnvVars: true
shutdown:
  drainTimeout: 10
# If there is a custom gitignore rule that excludes cribl.secret from bundle
bundler:
  bundleGitIgnoredPatterns: '**/cribl.secret'
```
