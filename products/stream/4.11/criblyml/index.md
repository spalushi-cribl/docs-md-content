# cribl.yml


`cribl.yml` contains settings for configuring API and other system properties.

```yaml {title="$CRIBL_HOME/default/cribl/cribl.yml"}
api: # [object]
  host: # [string] Address to bind to. Defaults to 0.0.0.0.
  port: # [number] Port to listen to. Defaults to 9000.
  sensitiveFields: # [array of string] Names of fields whose values should be returned as empty ('')
  # in responses for API requests to /inputs and /outputs endpoints. Use caution: this setting affects  
  # workflows and applications that rely on the responses for /inputs and /outputs endpoints. When you 
  # change or update a Source or Destination, you must also re-enter the correct value for any 
  # attribute that is listed in the sensitiveFields array.
  retryCount: # [number] Number of times to retry binding to API port. Defaults to 120. 
  retrySleepSecs: # [number] Period, in seconds, between consecutive port binding retries. Defaults to 5.
  baseUrl: # [string] URL base path from which to serve all assets (useful when behind a proxy).
  disabled: # [boolean] Flag to enable/disable local UI access. Defaults to false.
  workerRemoteAccess: # [boolean] Enable teleporting to Worker Nodes'. Defaults to false.
  revokeOnRoleChange: # [boolean] Log users out when their roles change. Defaults to true.
  authTokenTTL: # [number] How long (in seconds) authentication tokens remain valid. Default is 1 hr.; minimum is 1 sec.'
  idleSessionTTL: # [number] How long (in seconds) Cribl Stream will observe no user interaction before invalidating user's session tokens. Default is 1 hr.; minimum is 60 sec.
  headers: # [object] Custom HTTP headers to be sent with every response.
  loginRateLimit: # [string] Rate limit, expressed as maximum number of requests per interval (second, minute, hour, day). E.g., 3/second, 5/minute.
  ssoRateLimit: # [string] Rate limit for SSO and SLO callback endpoints. Expressed as maximum number of requests per second, minute, hour, or day, e.g., 3/second, 5/minute. When limit is reached, the Cribl Stream sends 429 Too Many Requests HTTP responses.
  apiCache: # [object] Toggle off to disable caching of browser's frequent API requests. (Can slow the UI's response time.)
  ssl: # [object] SSL Settings
    disabled: # [boolean] SSL is enabled by default.
    privKeyPath: # [string] Path to private key.
    certPath: # [string] Path to certificate.

auth: # [object] Authentication Settings
  type: # [string] Type - Select one of the supported authentication providers.

  # -------------- if type is ldap ---------------

  secure: # [boolean] Secure - Enable to use a secure ldap connection (ldaps://), disable for unsecure (ldap://) connection.
  ldapServers: # [array of strings] LDAP servers - List of LDAP servers, each entry should contain host:port (e.g., localhost:389)
  bindDN: # [string] Bind DN - Distinguished name of entity to authenticate with LDAP server, e.g., 'cn=admin,dc=example,dc=org'
  bindCredentials: # [string] Password - Distinguished Name password used to authenticate with LDAP server
  searchBase: # [string] User search base - Starting point to search LDAP for users, e.g., 'dc=example,dc=org'
  usernameField: # [string] Username field - LDAP user search field, e.g. cn or uid
  searchFilter: # [string] User search filter - LDAP search filter to apply when finding user, e.g. (&(group=admin)(!(department=123*)))
  groupSearchBase: # [string] Group search base - Starting point to search LDAP for groups, e.g., 'dc=example,dc=org'
  groupMemberField: # [string] Group member field - LDAP group search field, e.g. member
  groupSearchFilter: # [string] Group search filter - LDAP search filter to apply when finding group, e.g. (&(cn=cribl*)(objectclass=group))
  groupField: # [string] Group name field - LDAP group field, e.g. cn
  connectTimeout: # [number] Connection timeout (ms)
  rejectUnauthorized: # [boolean] Reject unauthorized - Valid for secure LDAP connections, set true to reject unauthorized server certificates.
  fallback: # [boolean] Fallback on fatal error - Attempt local authentication if LDAP authentication is down or mis-configured. Defaults to false.
  groups: # [object] 
    default: # [string] Default role - Default role assigned to groups not explicitly mapped to a role.
    mapping: # [object] Mapping - Group(s)-to-role(s) mappings

  # --------------------------------------------------------


  # -------------- if type is saas ---------------

  issuer: # [string] Issuer - Issuer from which to accept and validate JWT tokens.
  tenantId: # [string] Organization ID - The organization ID within which this instance is running.
  loginUrl: # [string] Login URL - The URL to redirect unauthenticated users to.

  # --------------------------------------------------------


  # -------------- if type is splunk ---------------

  host: # [string] Host - Hostname or address of Splunk instance that provides authentication. Defaults to localhost.
  port: # [number] Port - Port of Splunk instance that provides authentication. Defaults to 8089.
  ssl: # [boolean] SSL - Whether SSL is enabled on Splunk instance that provides authentication. Defaults to Yes.
  fallback: # [boolean] Fallback on fatal error - Attempt local authentication if Splunk is unsuccessful. Defaults to false.
  groups: # [object] 
    default: # [string] Default role - Default role assigned to groups not explicitly mapped to a role.
    mapping: # [object] Mapping - Group(s)-to-role(s) mappings

  # --------------------------------------------------------


  # -------------- if type is openid ---------------

  name: # [string] Provider name - The name of the identity provider service. Manual entries are also allowed.
  audience: # [string] Audience - The Audience from provider configuration. This will be the base URL of your Cribl server, i.e.: https://yourDomain.com:9000
  client_id: # [string] Client ID - The client_id from provider configuration.
  client_secret: # [string] Client secret - The client_secret provider configuration.
  scope: # [string] Scope - Space-separated list of authentication scopes. Default: openid profile email.
  auth_url: # [string] Authentication URL - The full path to the provider's authentication endpoint. Be sure to configure the callback URL at provider as <yourDomainUrl>/api/v1/auth/authorization-code/callback, e.g. https://yourDomain.com:9000/api/v1/auth/authorization-code/callback
  token_url: # [string] Token URL - The full path to the provider's access token URL.
  userinfo_url: # [string] User Info URL - The full path to the provider's user info url. If not provided, Stream will attempt to gather user info from the ID token returned from the Token URL.
  logout_url: # [string] Logout URL - The full path to the provider's logout URL. Leave blank if the provider does not support logout or token revocation.
  userIdExpr: # [string] User identifier - Expression used to derive userId from the id_token returned by the openId provider.
  rejectUnauthorized: # [boolean] Validate certs - Validate certificates; set to false to allow insecure self-signed certificates.
  filter_type: # [string] Filter type - Optional method for limiting access per user.
  groupField: # [string] Group name field - Field on the id_token that contains the user groups.
  fallback: # [boolean] Allow local auth - Allows locally configured users to log in to Stream.
  groups: # [object] 
    default: # [string] Default role - Default role assigned to groups not explicitly mapped to a role.
    mapping: # [object] Mapping - Group(s)-to-role(s) mappings

  # --------------------------------------------------------

system: # [object] 
  upgrade: # [string] 
  restart: # [string] 
  installType: # [string] 
  
workers: # [object] 
  count: # [number] 
  memory: # [number] 
tls: # [object] Default TLS Settings
  minVersion: # [string] Minimum TLS version - Minimum TLS version. Defaults to TLSv1.
  maxVersion: # [string] Maximum TLS version - Maximum TLS version. Defaults to TLSv1.3.
  defaultCipherList: # [string] Default cipher list - Default suite of enabled and disabled TLS ciphers. Defaults to:
      ECDHE-RSA-AES128-GCM-SHA256:
      ECDHE-ECDSA-AES128-GCM-SHA256:
      ECDHE-RSA-AES256-GCM-SHA384:
      ECDHE-ECDSA-AES256-GCM-SHA384:
      DHE-RSA-AES128-GCM-SHA256:
      ECDHE-RSA-AES128-SHA256:
      DHE-RSA-AES128-SHA256:
      ECDHE-RSA-AES256-SHA384:
      DHE-RSA-AES256-SHA384:
      ECDHE-RSA-AES256-SHA256:
      DHE-RSA-AES256-SHA256:
      HIGH:
      !aNULL:
      !eNULL:
      !EXPORT:
      !DES:
      !RC4:
      !MD5:
      !PSK:
      !SRP:
      !CAMELLIA
  defaultEcdhCurve: # [string] ECDH curve - The curve name, or a colon-separated list of curve NIDs or names, to use for ECDH key agreement. For example: 'Pâ521:Pâ384:Pâ256'. Defaults to 'auto'.
  rejectUnauthorized: # [boolean] Validate server certs - Whether to validate server certificates globally. Set to false to allow self-signed certs.
proxy: # [object] 
  useEnvVars: # [boolean] 
sni:
  disableSNIRouting: # [boolean] Manage Server Name Indication (SNI) routing. Set to true to disable for specific Groups/Fleets.
git: # [object] 
  branch: # [string] Branch - The branch to track in your Stream deployment's git repository.
  gitOps: # [string] GitOps workflow - The GitOps workflow for managing Stream's config.
  commitDeploySingleAction: # [boolean] Collapse actions - When enabled, Commit & Deploy will be collapsed into a single action. If you've configured a remote, Commit & Git Push will also be collapsed. Your DefaultÂ CommitÂ Message below will be used for all commits.
  defaultCommitMessage: # [string] Default commit message - Enter a default message to use for all commits.
  remote: # [string] Remote URL - Enter remote git repo's URL.
  authType: # [string] Authentication type - Git authentication type.

  # -------------- if authType is ssh ---------------

  sshKey: # [string] SSH private key - Enter SSH private key (without passphrase) to use for authentication on remote git repo.
  strictHostKeyChecking: # [boolean] SSH strict host key checking - Validate key against known hosts, to prevent spoofing or impersonation attacks. For details, see "VerifyingÂ Host Keys" [here](https://linux.die.net/man/1/ssh).

  # --------------------------------------------------------


  # -------------- if authType is basic ---------------

  user: # [string] User - Username for authentication.
  password: # [string] Password - Password for authentication. (With GitHub, use a personal access token.)

  # --------------------------------------------------------

  autoAction: # [string] Scheduled global actions - Global git actions to run automatically on a schedule.

  # -------------- if autoAction is commit ---------------

  autoActionSchedule: # [string] Schedule - Cron schedule to run selected git action on.
  autoActionMessage: # [string] Commit message - Default scheduled commit message.

  # --------------------------------------------------------


  # -------------- if autoAction is push ---------------

  autoActionSchedule: # [string] Schedule - Cron schedule to run selected git action on.

  # --------------------------------------------------------

  timeout: # [number] Git timeout - Max time (milliseconds) to wait for git processes before killing them, 0 to wait indefinitely


bundler: # [object] - Contains bundler configuration
    bundleGitIgnoredPatterns: # [string] [array of strings] - List of file patterns which must be bundled (overrides .gitignore).
```

{{< id `example` >}} Example `cribl.yml`: 

```yaml {title="$CRIBL_HOME/default/cribl/cribl.yml"}
api:
  host: 0.0.0.0
  port: 9000
  sensitiveFields:
    - awsApiKey
    - awsSecret
    - token
    - password
  retryCount: 120
  retrySleepSecs: 5
  baseUrl: ""
  # Flag to enable/disable UI. Default: false
  disabled : false
  loginRateLimit: 2/second
  ssl:
    disabled: false
    privKeyPath: /path/to/myKey.pem
    certPath: /path/to/myCert.pem  
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
workers: 
  count: -2
  minimum: 2
  memory: 2048
proxy:
  useEnvVars: true
# If there is a custom gitignore rule that excludes cribl.secret from bundle
bundler:
  bundleGitIgnoredPatterns: '**/cribl.secret'
```




