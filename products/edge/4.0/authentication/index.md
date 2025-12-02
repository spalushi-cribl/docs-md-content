# Authentication


User authentication in Cribl Edge

---

Cribl Edge supports **local**, **Splunk**, **LDAP**, and **SSO/OpenID Connect** authentication methods, depending on license type.

## Local Authentication

To set up local authentication, navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication** and select **Local**.

You can then manage users through the **Settings** > [**Global Settings** >] **Access Management > Local Users** UI. All changes made to users are persisted in a file located at `$CRIBL_HOME/local/cribl/auth/users.json`. 

> For Windows Edge Nodes, the changes are persisted in a file located at `C:\ProgramData\Cribl\local\cribl\auth\users.json`. 
>
{.box .windows}

This is the line format, and note that both usernames and passwords are case-sensitive:

`{"username":"user","first":"Goat","last":"McGoat","disabled":"false", "passwd":"Yrt0MOD1w8OzyMYB8WMcEleOtYESMwZw2qIZyTvueOE"}`

The file is monitored for modifications every 60s, and will be reloaded if changes are detected. 

Adding users through direct modification of the file is also supported, but not recommended.

> If you edit `users.json`, maintain each JSON element as a single line. Otherwise, the file will not reload properly.
>
{.box .warning}

### Manual Password Replacement {#manual-pwd}

To manually add, change, or restore a password, replace the affected user's `passwd` key-value pair with a `password` key, in this format: `"password":"<newPlaintext>"`. Cribl Edge will hash all plaintext password(s), identified by the `password` key, during the next file reload, and will rename the plaintext `password` key. 

Starting with the same `users.json` line above:

`{"username":"user","first":"Goat","last":"McGoat","disabled":"false", "passwd":"Yrt0MOD1w8OzyMYB8WMcEleOtYESMwZw2qIZyTvueOE"}`   

...you'd modify the final key-value pair to something like:

`{"username":"user","first":"Goat","last":"McGoat","disabled":"false", "password":"V3ry53CuR&pW9"}` 

Within at most one minute after you save the file, Cribl Edge will rename the `password` key back to `passwd`, and will hash its value, re-creating something resembling the original example.

### Set Worker/Edge Node Passwords {#worker_pwd}

In a distributed deployment ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)), once a Worker/Edge Node has been set to point to the Leader Node, Cribl Edge will set each Worker/Edge Node's admin password with a randomized password that is different from the admin user's password on the Leader Node. This is by design, as a security precaution. But it might lead to situations where administrators cannot log into a Worker/Edge Node directly, and must rely on accessing them via the Leader.

To explicitly apply a known/new password to your Edge Node, you set and push a new password to the Fleet. Here's how, in the Leader Node's UI:

1. From the top nav, select **Manage**.
2. Select the desired Fleet. 
3. From the Fleet's submenu, select **Group Settings**.
4. Select **Local Users**, then expand the desired user. 
6. Update the **Password** and **Confirm Password** fields and select **Save**.

Every 10 seconds, the Worker/Edge Node will request an update of configuration from the Leader, and any new password settings will be included.

> On Cribl.Cloud, Group Settings for hybrid Workers include the above **Local Users** options, but Groups for Cribl-managed Workers do not. For alternatives, see the [Cribl.Cloud Launch Guide](deploy-cloud#access) and [Cribl.Cloud SSO Setup](cloud-sso) docs.
>
{.box .cloud}

### Authentication Controls

Use the following authentication-specific controls at **Settings** > [**Global Settings** >] > **General Settings** > **API Server Settings** > **Advanced** to customize authentication behavior:

[Snippet not found: content/shared/4.0/snippets/_api-server-settings-authentication.md]

###  Token Renewal and Session Timeout  {#token}

Here is how Cribl Edge sets tokens' valid lifetime by applying the **Auth-token TTL** field's value:

- When a user logs in, Cribl Edge returns a token whose expiration time is set to {login time + **Auth‑token TTL** value}.

- If the user is idle (no UI activity) for the configured token lifetime, they are logged out.

- As long as the user is interacting with Cribl Edge's UI in their browser, Cribl Edge continually renews the token, resetting the idle-session time limit back by the **Auth‑token TTL** value.

### The cribl.secret File

When Cribl Edge first starts, it creates a `$CRIBL_HOME/local/cribl/auth/cribl.secret` file. This file contains a key that is used to generate auth tokens for users, encrypt their passwords, and encrypt encryption keys.

Default local credentials are: `admin/admin`

> Back up and secure access to this file by applying strict permissions – e.g., `600`.
>
{.box .danger}

## External Authentication

Below are configuration details for the following external authentication providers:

- [Splunk Authentication](#splunk)
- [LDAP Authentication](#ldap)
- [SSO/OpenID Connect Authentication](#sso)

> All of these external auth methods require either an Enterprise or a Standard license. They're not supported with a Free license.
> 
> Cribl Edge Roles and [role mapping](#role-mapping) are supported **only** with an Enterprise license. With a Standard license, all your external users will be imported to Cribl Edge in the `admin` role.
> 
> While configuring any external auth method, make sure you don't get locked out of Cribl Edge! Enable the **Fallback on fatal error** or **Allow local auth** toggle until you're certain that external auth is working as intended. If you do get locked out, refer back to [Manual Password Replacement](#manual-pwd) for the remedy.
>
{.box .warning}

###  Splunk Authentication  {#splunk}

Splunk authentication is very helpful when deploying Cribl Edge in the same environment as Splunk. This option requires the user to have Splunk `admin` role permissions. To set up Splunk authentication: 

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type** and select **Splunk**. This exposes the following controls.

- **Host**: Splunk hostname (typically a search head).

- **Port**: Splunk management port (defaults to `8089`).

- **SSL**: Whether SSL is enabled on Splunk instance that provides authentication. Defaults to `Yes`.

- **Fallback on fatal error**: Attempt local authentication if Splunk authentication is unsuccessful. Defaults to `No`. If toggled to `Yes`, Cribl Edge will attempt local auth only **after** a failed Splunk auth. Selecting `Yes` also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on Splunk. This similarly defaults to `No`.

> To prevent lockout, Cribl strongly recommends enabling **Fallback on fatal error** until you're certain that external auth is working as intended. If you do get locked out, see [Manual Password Replacement](#manual-pwd).
> 
> The Splunk search head does not need to be locally installed on the Cribl Edge instance. See also [Role Mapping](#role-mapping) below.
>
{.box .success}

###  LDAP Authentication  {#ldap}

You can set up LDAP authentication as follows: 

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type**, and select **LDAP**. This exposes the following controls.

- **Secure**: Enable to use a secure LDAP connections (`ldaps://`). Disable for an insecure (`ldap://`) connection.

- **LDAP servers**: List of LDAP servers. Each entry should contain `host:port` (e.g., `localhost:389`).

- **Bind DN**: Distinguished name of entity to authenticate with LDAP server. E.g., `'cn=admin,dc=example,dc=org'`.

- **Password**: Distinguished Name password used to authenticate with LDAP server.

- **User search base**: Starting point to search LDAP for users, e.g., `'dc=example,dc=org'`.

- **Username field**: LDAP user search field, e.g., `cn` or `(cn (or uid)`. For Microsoft Active Directory, use `sAMAccountName` here.

- **User search filter**: LDAP search filter to apply when finding user. Optional. Example:    
`(&(group=admin)(!(department=123*)))`

- **Group search base**,  
**Group search filter**,  
**Group member field**,  
**Group name field**: These settings are used only for LDAP authentication with role-based access control. See [Role‑Based LDAP Authentication](#ldap-plus-rbac), below.

- **Connection timeout (ms)**: Defaults to `5000`.

- **Reject unauthorized**: Valid for secure LDAP connections. Set to `Yes` to reject unauthorized server certificates.

- **Fallback on fatal error**: Attempt local authentication if LDAP authentication is down or misconfigured. Defaults to `No`. If toggled to `Yes`, local auth will be attempted only **after** a failed LDAP auth. Selecting `Yes` also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on the LDAP provider. Defaults to `No`.

  > To prevent lockout, Cribl strongly recommends enabling **Fallback on fatal error** until you're certain that external auth is working as intended. If you do get locked out, see [Manual Password Replacement](#manual-pwd).
  {.box .warning}

#### Role-Based LDAP Authentication {#ldap-plus-rbac}

When configuring LDAP authentication with [role‑based](roles) access control (RBAC), you **must** use the following settings to import user groups. (The UI does not enforce filling these fields. When using LDAP without roles, ignore them.)

- **Group search base**: Starting point to search LDAP for groups, e.g., `dc=example,dc=org`.

- **Group member field**: LDAP group search field, e.g., `member`.

- **Group search filter**: LDAP search filter to apply when finding group, e.g.,  
`(&(cn=cribl*)(objectclass=group))`.

- **Group name field**: Attribute used in objects' DNs that represents the group name, e.g., `cn`. Cribl Edge does not directly read this attribute from group objects; rather, it must be present in your groups' DN values. Match the attribute name's original case (upper, lower, or mixed) when you specify it in this field. In particular, Microsoft Active Directory requires all-uppercase group names (e.g., `CN`).

> See also [Role Mapping](#role-mapping) below.
>
{.box .info}

### SSO/OpenID Connect Authentication  {#sso}

Cribl Edge supports SSO/OpenID user authentication (login/password) and authorization (user's group membership, which you can map to Cribl [Roles](#role-mapping)). Using OpenID will change the default `Log in` button on the login page to a button labeled `Log in with <provider>` which redirects to the specified provider. Set this up as follows: 

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type** and select **OpenID Connect**. This exposes the following controls.

- **Provider name**: The name of the identity provider service. You can select **Google** or **Okta**, both supported natively. Manual entries are also allowed. 

- **Audience**: The Audience from provider configuration. This will be the base URL, e.g.: `https://leader.yourDomain.com:9000` for a distributed environment.

  > For distributed environments with a [second Leader](deploy-add-second-leader) configured, modify the **Audience** field to point to the load balancer instead of the Leader Node.
  {.box .info}

- **Client ID**: The `client_id` from provider configuration.

- **Client secret**: The `client_secret` from provider configuration.

- **Scope**: Space-separated list of authentication scopes. The default list is: `openid profile email`. If you populate the **User info URL** field, you must add `groups` to this list.

- **Authentication URL**: The full path to the provider's authentication endpoint. Be sure to configure the callback URL at the provider as `<masterServerFQDN>:9000/api/v1/auth/authorization-code/callback`, e.g.: `https://leader.yourDomain.com:9000/api/v1/auth/authorization-code/callback`.

- **Token URL**: The full path to the provider's access token URL.

- **User info URL**: The full path to the provider's user info URL. Optional; if not provided, Cribl Edge will attempt to gather user info from the ID token returned from the **Token URL**.

- **Logout URL**: The full path to the provider's logout URL. Leave blank if the provider does not support logout or token revocation.

- **User identifier**: JavaScript expression used to derive `userId` from the `id_token` returned by the OpenID provider.

- **Validate certs**: Whether to validate certificates. Defaults to `Yes`. Toggle to `No` to allow insecure self‑signed certificates.

- **Filter type**: Select either **Email allowlist** or **User info filter**. This selection displays one of the following fields:
  - **Email allowlist**: Wildcard list of emails/email patterns that are allowed access.
  - **User info filter**: JavaScript expression to filter against user profile attributes. E.g.: `name.startsWith("someUser") && email.endsWith("domain.com")`

- **Group name field**: Field in the **User info URL** response (if configured); otherwise, `id_token` that contains the user groups. Defaults to `groups`. 

- **Allow local auth**: Toggle to `Yes` to also users to log in using Cribl Edge's local authentication. This enables an extra button called `Log in with local user` on the Cribl Edge login page. (This option ensures fallback access for local users if SSO/OpenID authentication fails.)

  > To prevent lockout, Cribl strongly recommends enabling **Allow local auth** until you're certain that external auth is working as intended. If you do get locked out, see [Manual Password Replacement](#manual-pwd).
  {.box .warning}

- **Email allowlist**: Wildcard list of emails/email patterns that are allowed access.

Note the following details when filling in the form – for example, when using Okta:

- `<Issuer URI>` is the account at the identity provider.

- `Audience` is the URL of the host that will be connecting to the Issuer (e.g., `https://leader.yourDomain.com:9000` for a distributed environment). The issuer (Okta, in this example) will redirect back to this site upon authentication success or failure.

- `User info URL` is required, because Okta doesn't encode groups in `id_token`. Azure AD and Google also rely on this field.

> See also [Role Mapping](#role-mapping) below.
> 
> The only OAuth 2.0 flow that Cribl Edge supports is the [Authorization Code Grant](https://developer.okta.com/blog/2018/04/10/oauth-authorization-code-grant-type) flow.
> 
> In version 3.0 and higher, Cribl Edge's former "master" application components are renamed "leader." Above, while some legacy terminology remains within URLs, this document will reflect that.
>
{.box .info}

### Role Mapping {#role-mapping}

This section is displayed only on **distributed** deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with an Enterprise License. For details on mapping your external identity provider's configured groups to corresponding Cribl Edge user access Roles, see [External Groups and Roles](roles#groups). The controls here are:

- **Default role**: Default Cribl Edge Role to assign to all groups not explicitly mapped to a Role.

- **Mapping**: On each mapping row, enter an external group name on the left, and select the corresponding Cribl Edge Role on the right drop-down list. Click **+ Add Mapping** to add more rows.

  > The external group name on the left may be case-sensitive.
  {.box .info}
