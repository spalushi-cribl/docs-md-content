# Authentication


User authentication in Cribl Edge

---

Cribl Edge supports **local**, **Splunk**, **LDAP**, **SSO/OpenID Connect**, and **SSO/SAML** authentication methods, depending on license type.

## Local Authentication

To set up local authentication:

1. In the sidebar, select **Settings**, then **Global**.
1. Under **Access Management**, select **Authentication**.
1. Choose **Local** as authentication **Type**.

You can then manage users through the **Settings** > **Global** > **Access Management > Local Users** page.
All changes made to users are persisted in a file located at `$CRIBL_HOME/local/cribl/auth/users.json`. 

> For Windows Edge Nodes, the changes are persisted in a file located at `C:\ProgramData\Cribl\local\cribl\auth\users.json`. 
>
{.box .windows}

This is the line format, and note that both usernames and passwords are case-sensitive:

`{"username":"user","first":"Goat","last":"McGoat","roles":["admin"],"disabled":"false","passwd":"Yrt0MOD1w8OzyMYB8WMcEleOtYESMwZw2qIZyTvueOE"}`

The file is monitored for modifications every 60s, and will be reloaded if changes are detected. 

Adding users through direct modification of the file is also supported, but not recommended.

> If you edit `users.json`, maintain each JSON element as a single line. Otherwise, the file will not reload properly.
>
{.box .warning}

### Manual Password Replacement {#manual-pwd}

> Manual password replacement is available only in on-prem deployments.
> In Cribl.Cloud, when logging into Cribl Edge, you can reset your password by selecting the **Forgot password?** link.
{.box .cloud}

To manually add, change, or restore a password, replace the affected user's `passwd` key-value pair with a `password` key, in this format: `"password":"<newPlaintext>"`. Cribl Edge will hash all plaintext password(s), identified by the `password` key, during the next file reload, and will rename the plaintext `password` key. 

Starting with the same `users.json` line above:

`{"username":"user","first":"Goat","last":"McGoat","roles":["admin"],"disabled":"false","passwd":"Yrt0MOD1w8OzyMYB8WMcEleOtYESMwZw2qIZyTvueOE"}`

...you'd modify the final key-value pair to something like:

`{"username":"user","first":"Goat","last":"McGoat","roles":["admin"],"disabled":"false","password":"V3ry53CuR&pW9"}`

Within at most one minute after you save the file, Cribl Edge will rename the `password` key back to `passwd`, and will hash its value, re-creating something resembling the original example.

### Set Worker/Edge Node Passwords {#worker_pwd}

In a Distributed deployment ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)), once a Worker/Edge Node has been set to point to the Leader Node, Cribl Edge will set each Worker/Edge Node's admin password with a randomized password that is different from the admin user's password on the Leader Node. This is by design, as a security precaution. But it might lead to situations where administrators cannot log into a Worker/Edge Node directly, and must rely on accessing them via the Leader.

To explicitly apply a known/new password to your Edge Node, you set and push a new password to the Fleet. Here's how, in the Leader Node's UI:

1. On the top bar, select **Products**, and then select **Cribl Edge**.
1. Select the desired Fleet. 
1. On the Fleet submenu, select **Fleet Settings**.
1. Select **Local Users**, then expand the desired user. 
1. Update the **Password** and **Confirm Password** fields and select **Save**.

Every 10 seconds, the Worker/Edge Node will request an update of configuration from the Leader, and any new password settings will be included.

> On Cribl.Cloud, Group Settings for customer-managed hybrid Workers include the above **Local Users** options, but Groups for Cribl-managed Workers in Cribl.Cloud do not. For alternatives, see the [Cribl.Cloud Launch Guide](cloud-vs-self-hosted#access) and [Cribl.Cloud SSO Setup](cloud-sso) docs.
>
{.box .cloud}

### Authentication Controls {#auth-controls}

Use the following authentication-specific controls at **Settings** > **Global** > **General Settings** > **API Server Settings** > **Advanced** to customize authentication behavior:

[Snippet not found: content/shared/4.12/snippets/_api-server-settings-authentication.md]

###  Token Renewal and Session Timeout  {#token}

Here is how Cribl Edge sets tokens' valid lifetime by applying the **Auth-token TTL** field's value:

- When a user logs in, Cribl Edge returns a token whose expiration time is set to {login time + **Auth‑token TTL** value}.

- If the user is idle (no UI activity) for the configured token lifetime, they are logged out.

- As long as the user is interacting with Cribl Edge's UI in their browser, Cribl Edge continually renews the token, resetting the idle-session time limit back by the **Auth‑token TTL** value.

### The cribl.secret File

When Cribl Edge first starts, it creates a `$CRIBL_HOME/local/cribl/auth/cribl.secret` file. This file contains a key that is used to generate auth tokens for users, encrypt their passwords, and encrypt encryption keys.

Default local credentials are: `admin/admin`

> Back up and secure access to this file by applying strict permissions – for example, `600`.
>
{.box .danger}

## External Authentication

> While configuring any external auth method, make sure you don't get locked out of Cribl Edge! Enable the **Fallback on fatal error** or **Allow login as Local User** toggle until you're certain that external auth is working as intended. If you do get locked out, refer back to [Manual Password Replacement](#manual-pwd) for the remedy.
> 
> All external auth methods require certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
> 
> Cribl Edge Roles and [role mapping](#role-mapping) are supported **only** with an Enterprise license. With a Standard license, all your external users will be imported to Cribl Edge in the `admin` Role.
>
{.box .warning}

This topic covers the following external authentication providers:

- [Splunk Authentication](#splunk)
- [LDAP Authentication](#ldap)

####  Authentication for Single Sign-On {#sso-auth}

Each authentication method that Cribl Edge supports for on-prem SSO has its own separate topic:

- [SSO/OpenID Connect Authentication](usecase-sso-okta)
- [SSO/SAML Authentication](usecase-sso-saml)

> For deployments in Cribl.Cloud, see [Cribl.Cloud SSO Setup](cloud-sso).
>
{.box .cloud}

###  Splunk Authentication  {#splunk}

Splunk authentication is very helpful when deploying Cribl Edge in the same environment as Splunk. This option requires the user to have Splunk `admin` role permissions. To set up Splunk authentication: 

Navigate to **Settings** > **Global** > **Access Management** > **Authentication** > **Type** and select **Splunk**. This exposes the following controls.

- **Host**: Splunk hostname (typically a search head).

- **Port**: Splunk management port (defaults to `8089`).

- **SSL**: Whether SSL is enabled on Splunk instance that provides authentication. Defaults to toggled on.

- **Validate certificates**: When enabled, Cribl Edge will reject any certificate that the Certificate Authority (CA) for your system does not authorize. Although this control is off by default, Cribl strongly recommends that you enable it, except when you must allow untrusted (for example, self-signed) certificates.

- **Fallback on fatal error**: Toggle on to attempt local authentication if Splunk authentication is unsuccessful. Default is toggled off. If toggled on, Cribl Edge will attempt local auth only **after** a failed Splunk auth. Toggling on also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on Splunk. Default is toggled off.

> The Splunk search head does not need to be locally installed on the Cribl Edge instance.
>
{.box .success}

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

###  LDAP Authentication  {#ldap}

You can set up LDAP authentication as follows: 

Navigate to **Settings** > **Global** > **Access Management** > **Authentication** > **Type**, and select **LDAP**. This exposes the following controls.

- **Secure**: Enable to use a secure LDAP connections (`ldaps://`). Disable for an insecure (`ldap://`) connection.

- **LDAP servers**: List of LDAP servers. Each entry should contain `host:port` – for example, `localhost:389`.

- **Bind DN**: Distinguished Name of entity to authenticate with LDAP server – for example, `'cn=admin,dc=example,dc=org'`.

- **Password**: Distinguished Name password used to authenticate with LDAP server.

- **User search base**: Starting point to search LDAP for users – for example, `'dc=example,dc=org'`.

- **Username field**: LDAP user search field, such as `cn` or `(cn (or uid)`. For Microsoft Active Directory, use `sAMAccountName` here.

- **User search filter**: LDAP search filter to apply when authenticating users. Optional, but recommended: If empty, all users in your domain will be able to log in with the default Cribl Role. This example would enable only users in the `StreamAdmins` group to log in – they would then be mapped to any Roles specified in [Role Mapping Settings](#role-mapping):  
`(&(objectClass=user)(memberof=CN=StreamAdmins,OU=Groups,DC=cribl,DC=io))`  
This simpler filter example would enable anyone in a group called ‟admin,” who does **not** have a department attribute that starts with “123,” to log in:  
`(&(group=admin)(!(department=123*)))`

- **Group search base**,  
 **Group member field**,  
 **Group membership attribute**,  
 **Group search filter**,   
**Group name field**: These settings are used only for LDAP authentication with role-based access control. See [Role‑Based LDAP Authentication](#ldap-plus-rbac), below.

- **Connection timeout (ms)**: Defaults to `5000`.

- **Reject unauthorized**: Valid for secure LDAP connections. Toggle on to reject unauthorized server certificates.

- **Fallback on fatal error**: Attempt local authentication if LDAP authentication is down or misconfigured. Default is toggled off. If toggled on, local auth will be attempted only **after** a failed LDAP auth. Toggling on also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on the LDAP provider. Default is toggled off.

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

#### Role-Based LDAP Authentication {#ldap-plus-rbac}

When configuring LDAP authentication with [role‑based](roles) access control (RBAC), you **must** use the following settings to import user groups. (The UI does not enforce filling these fields. When using LDAP without roles, ignore them.)

- **Group search base**: Starting point to search LDAP for groups – for example, `dc=example,dc=org`.

- **Group member field**: LDAP group search field – for example, `member`.

- **Group membership attribute**: Attribute name of LDAP user object. Determines group member attribute's value, which defines group's allowed users. This field accepts `dn`, `cn`, `uid`, or `uidNumber`. If none of these are specified, falls back to `dn`. 

- **Group search filter**: LDAP search filter to apply when retrieving groups for authorization and mapping to Roles – for example, `(&(cn=cribl*)(objectclass=group))`.

- **Group name field**: Attribute used in objects' DNs that represents the group name – for example, `cn`. Cribl Edge does not directly read this attribute from group objects; rather, it must be present in your groups' DN values. Match the attribute name's original case (upper, lower, or mixed) when you specify it in this field. In particular, Microsoft Active Directory requires all-uppercase group names – for example, `CN`.

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

###  Personal Identity Verification (PIV) Authentication  {#piv}

Cribl supports Personal Identity Verification (PIV) authentication with an [Enterprise license](licensing) for an on-prem deployment.

To use PIV authentication, you must [configure and enable TLS](securing-import-certs#upload-certificate). Define your chain of trust for all Certificate Authorities (CAs) in the CA certificate path field in the TLS settings. The PEM-encoded certificate file can include multiple certificates.

PIV authentication is compatible only with [LDAP](#ldap). Users must exist in LDAP to successfully log in using client certificates. If you enable the **Fallback on fatal error** and **Fallback on bad login** options in the LDAP authentication settings, local username and password authentication is also available.

> Cribl does not perform Online Certificate Status Protocol (OCSP) revocation checks for PIV authentication. To revoke access for PIV-authenticated users, you must remove their entries from LDAP.
>
{.box .info}

Follow these steps to configure authentication with PIV credentials:

1. Configure [LDAP authentication](#ldap). To fall back to local login with username and password if PIV authentication fails, in the LDAP authentication settings, enable the **Fallback on fatal error** and **Fallback on bad login** options.

1. Open port 9002 to allow the browser to access the identity server. If you are using a load balancer, open port 9002 on it too.

1. Make sure that the API server's Fully Qualified Domain Name (FQDN) is defined in the `san.ip` or `san.dns` field in the API server certificate.

1. Use the Cribl Stream command line interface (CLI) to configure and enable PIV with the [auth mf](cli-auth) sub-command. For example:

    ```
    $CRIBL_HOME/bin/cribl auth mf -e true -u '/(?:UPN:)([^,]+)(?:,|$)/' -f san -o 'https://127.0.0.1:9000'
    ```

    In this example command:
    * `e true` enables PIV authentication.
    * `u '/(?:UPN:)([^,]+)(?:,|$)/'` is the regular expression (regex) pattern to use to extract the username from the PIV client certificate. Update the pattern according to your implementation. Cribl applies the pattern to the certificate field that you define with the `-f` option.
    * `-f san` is the certificate field from which to extract the username for authentication. Specify either `san` or `subject`, depending on your implementation.
    * `-o 'https://127.0.0.1:9000'` is the URL for the Cribl deployment. Update with the correct URL for your deployment.<br><br>

    You can also include an `-a` option to specify an API server URL (for example, `-a 'https://internal-fqdn'`). If you do not include an `-a` option in your command, the API server URL defaults to the value you provide for the `-o` option.

1. Restart the Leader Node so that the change takes effect:

    ```
    $CRIBL_HOME/bin/cribl restart
    ```

1. Confirm that PIV authentication is configured and enabled:

    ```
    $CRIBL_HOME/bin/cribl auth mf
    ```

    The response indicates the PIV authentication status, along with the current configured values. Using the configured values set in the example command above, the response would be:
    
    ```
    PIV multi-factor authentication is enabled.
    Username field: san
    Username regex pattern: /(?:UPN:)([^,]+)(?:,|$)/
    Access control allow origin header: https://127.0.0.1:9000
    Api server url: https://127.0.0.1:9000
    ```

    The response includes notifications for unconfigured values or issues like failure to restart or enable TLS. For example:
    
    ```
    PIV multi-factor authentication is enabled.
    Username field not defined. Please make sure to configure it.
    Username regex pattern: /(?:UPN:)([^,]+)(?:,|$)/
    Access control allow origin header: https://127.0.0.1:9000
    Api server url: https://127.0.0.1:9000
    You will need to restart instance before your changes take full effect.
    Warning: TLS is not enabled. PIV will not function until TLS is successfully configured and enabled.
    ```

After you configure and enable PIV and restart the Leader Node, users can select **PIV/CAC** on the Cribl login screen to log in without entering a username or password. Users are prompted to enter their PIV credentials, and Cribl validates the certificates, fetches the user from LDAP, and completes login.

To disable PIV authentication:

1. Set the `-e` option to `false` using the [auth mf](cli-auth) sub-command:

    ```
    $CRIBL_HOME/bin/cribl auth mf -e false
    ```

1. Restart the Leader Node so that the change takes effect:

    ```
    $CRIBL_HOME/bin/cribl restart
    ```
    
### Role Mapping Settings {#role-mapping}

This section is displayed only on Distributed deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with an Enterprise License. For details on mapping your external identity provider's configured groups to corresponding Cribl Edge user access Roles, see [External Groups and Roles](roles#groups). The controls here are:

- **Default Role**: Default Cribl Edge Role to assign to all groups not explicitly mapped to a Role.

- **Mapping**: On each mapping row, enter an external group name (case-sensitive) on the left, and select the corresponding Cribl Edge Role in the right drop-down list. Click **Add Mapping** to add more rows.