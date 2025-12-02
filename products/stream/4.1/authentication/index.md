# Authentication


User authentication in Cribl Stream

---

Cribl Stream supports **local**, **Splunk**, **LDAP**, **SSO/OpenID Connect**, and **SSO/SAML** authentication methods, depending on license type.

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

> Manual Password Replacement is for on-prem deployments only. For Cribl Stream in Cribl.Cloud, Cribl recommends setting up [Fallback Access](cloud-sso#fallback); if you do need to reset your password, you must contact Cribl.
>
{.box .cloud}

To manually add, change, or restore a password, replace the affected user's `passwd` key-value pair with a `password` key, in this format: `"password":"<newPlaintext>"`. Cribl Stream will hash all plaintext password(s), identified by the `password` key, during the next file reload, and will rename the plaintext `password` key. 

Starting with the same `users.json` line above:

`{"username":"user","first":"Goat","last":"McGoat","disabled":"false", "passwd":"Yrt0MOD1w8OzyMYB8WMcEleOtYESMwZw2qIZyTvueOE"}`   

...you'd modify the final key-value pair to something like:

`{"username":"user","first":"Goat","last":"McGoat","disabled":"false", "password":"V3ry53CuR&pW9"}` 

Within at most one minute after you save the file, Cribl Stream will rename the `password` key back to `passwd`, and will hash its value, re-creating something resembling the original example.



### Set Worker/Edge Node Passwords {#worker_pwd}

In a distributed deployment ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)), once a Worker/Edge Node has been set to point to the Leader Node, Cribl Stream will set each Worker/Edge Node's admin password with a randomized password that is different from the admin user's password on the Leader Node. This is by design, as a security precaution. But it might lead to situations where administrators cannot log into a Worker/Edge Node directly, and must rely on accessing them via the Leader.

To explicitly apply a known/new password to your Worker Node, you set and push a new password to the Worker Group. Here's how, in the Leader Node's UI:

1. From the top nav, select **Manage**.
2. Select the desired Worker Group. 
3. From the Worker Group's submenu, select **Group Settings**.
4. Select **Local Users**, then expand the desired user. 
6. Update the **Password** and **Confirm Password** fields and select **Save**.

Every 10 seconds, the Worker/Edge Node will request an update of configuration from the Leader, and any new password settings will be included.

> On Cribl.Cloud, Group Settings for hybrid Workers include the above **Local Users** options, but Groups for Cribl-managed Workers do not. For alternatives, see the [Cribl.Cloud Launch Guide](deploy-cloud#access) and [Cribl.Cloud SSO Setup](cloud-sso) docs.
>
{.box .cloud}

### Authentication Controls

Use the following authentication-specific controls at **Settings** > [**Global Settings** >] > **General Settings** > **API Server Settings** > **Advanced** to customize authentication behavior:

[Snippet not found: content/shared/4.1/snippets/_api-server-settings-authentication.md]

###  Token Renewal and Session Timeout  {#token}

Here is how Cribl Stream sets tokens' valid lifetime by applying the **Auth-token TTL** field's value:

- When a user logs in, Cribl Stream returns a token whose expiration time is set to {login time + **Auth‑token TTL** value}.

- If the user is idle (no UI activity) for the configured token lifetime, they are logged out.

- As long as the user is interacting with Cribl Stream's UI in their browser, Cribl Stream continually renews the token, resetting the idle-session time limit back by the **Auth‑token TTL** value.

### The cribl.secret File

When Cribl Stream first starts, it creates a `$CRIBL_HOME/local/cribl/auth/cribl.secret` file. This file contains a key that is used to generate auth tokens for users, encrypt their passwords, and encrypt encryption keys.

Default local credentials are: `admin/admin`

> Back up and secure access to this file by applying strict permissions – e.g., `600`.
>
{.box .danger}

## External Authentication

> While configuring any external auth method, make sure you don't get locked out of Cribl Stream! Enable the **Fallback on fatal error** or **Allow login as Local User** toggle until you're certain that external auth is working as intended. If you do get locked out, refer back to [Manual Password Replacement](#manual-pwd) for the remedy.
> 
> All external auth methods require either an Enterprise or a Standard license. They're not supported with a Free license.
> 
> Cribl Stream Roles and [role mapping](#role-mapping) are supported **only** with an Enterprise license. With a Standard license, all your external users will be imported to Cribl Stream in the `admin` Role.
>
{.box .warning}

This topic covers the following external authentication providers:

- [Splunk Authentication](#splunk)
- [LDAP Authentication](#ldap)

####  Authentication for Single Sign-On {#sso-auth}

Each authentication method that Cribl Stream supports for on-prem SSO has its own separate topic:

- [SSO/OpenID Connect Authentication](usecase-sso-okta)
- [SSO/SAML Authentication](usecase-sso-saml)

For deployments in Cribl.Cloud, see [Cribl.Cloud SSO Setup](cloud-sso).

###  Splunk Authentication  {#splunk}

Splunk authentication is very helpful when deploying Cribl Stream in the same environment as Splunk. This option requires the user to have Splunk `admin` role permissions. To set up Splunk authentication: 

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type** and select **Splunk**. This exposes the following controls.

- **Host**: Splunk hostname (typically a search head).

- **Port**: Splunk management port (defaults to `8089`).

- **SSL**: Whether SSL is enabled on Splunk instance that provides authentication. Defaults to `Yes`.

- **Fallback on fatal error**: Attempt local authentication if Splunk authentication is unsuccessful. Defaults to `No`. If toggled to `Yes`, Cribl Stream will attempt local auth only **after** a failed Splunk auth. Selecting `Yes` also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on Splunk. This similarly defaults to `No`.

> The Splunk search head does not need to be locally installed on the Cribl Stream instance.
>
{.box .success}

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

###  LDAP Authentication  {#ldap}

You can set up LDAP authentication as follows: 

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type**, and select **LDAP**. This exposes the following controls.

- **Secure**: Enable to use a secure LDAP connections (`ldaps://`). Disable for an insecure (`ldap://`) connection.

- **LDAP servers**: List of LDAP servers. Each entry should contain `host:port` (e.g., `localhost:389`).

- **Bind DN**: Distinguished Name of entity to authenticate with LDAP server. E.g., `'cn=admin,dc=example,dc=org'`.

- **Password**: Distinguished Name password used to authenticate with LDAP server.

- **User search base**: Starting point to search LDAP for users, e.g., `'dc=example,dc=org'`.

- **Username field**: LDAP user search field, e.g., `cn` or `(cn (or uid)`. For Microsoft Active Directory, use `sAMAccountName` here.

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

- **Reject unauthorized**: Valid for secure LDAP connections. Set to `Yes` to reject unauthorized server certificates.

- **Fallback on fatal error**: Attempt local authentication if LDAP authentication is down or misconfigured. Defaults to `No`. If toggled to `Yes`, local auth will be attempted only **after** a failed LDAP auth. Selecting `Yes` also exposes this additional option:
  - **Fallback on bad login**: Attempt local authentication if the supplied user/password fails to log in on the LDAP provider. Defaults to `No`.

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

#### Role-Based LDAP Authentication {#ldap-plus-rbac}

When configuring LDAP authentication with [role‑based](roles) access control (RBAC), you **must** use the following settings to import user groups. (The UI does not enforce filling these fields. When using LDAP without roles, ignore them.)

- **Group search base**: Starting point to search LDAP for groups, e.g., `dc=example,dc=org`.

- **Group member field**: LDAP group search field, e.g., `member`.

- **Group membership attribute**: Attribute name of LDAP user object. Determines group member attribute's value, which defines group's allowed users. This field accepts `dn`, `cn`, `uid`, or `uidNumber`. If none of these are specified, falls back to `dn`. 

- **Group search filter**: LDAP search filter to apply when retrieving groups for authorization and mapping to Roles., e.g., `(&(cn=cribl*)(objectclass=group))`.

- **Group name field**: Attribute used in objects' DNs that represents the group name, e.g., `cn`. Cribl Stream does not directly read this attribute from group objects; rather, it must be present in your groups' DN values. Match the attribute name's original case (upper, lower, or mixed) when you specify it in this field. In particular, Microsoft Active Directory requires all-uppercase group names (e.g., `CN`).

For distributed deployments with an Enterprise License, the next (and final) step is to configure [Role Mapping](#role-mapping) settings.

### Role Mapping Settings {#role-mapping}

This section is displayed only on **distributed** deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with an Enterprise License. For details on mapping your external identity provider's configured groups to corresponding Cribl Stream user access Roles, see [External Groups and Roles](roles#groups). The controls here are:

- **Default Role**: Default Cribl Stream Role to assign to all groups not explicitly mapped to a Role.

- **Mapping**: On each mapping row, enter an external group name (case-sensitive) on the left, and select the corresponding Cribl Stream Role in the right drop-down list. Click **Add Mapping** to add more rows.