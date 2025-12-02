# auth


Log into or out of Cribl Edge.

**Sub-commands:**

- [`auth login`](#auth-login)
- [`auth logout`](#auth-logout)
- [`auth mf`](#auth-mf)

## Sub-commands and Options

### `auth login` {#auth-login}

Login with a username and password or use a file with credentials.

**Usage:**

```text
./cribl auth login -H http://localhost:9000 -u example-username -p example-password
./cribl auth login -H http://localhost:9000 -f example-credentials-file.txt
```

See additional [`auth login` Examples](#auth-login-examples).

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-H <host>` | Host URL. Example: `http://localhost:9000`. |
| `-u <username>` | Username. |
| `-p <password>` | Password. |
| `-f <file>` | File with credentials. |

Arguments of the `login` subcommand correspond to the following environment variables:

| **Argument** | **Variable** |
| --- | --- |
| `<username>` | `CRIBL_USERNAME` |
| `<password>` | `CRIBL_PASSWORD` |
| `<host>` | `CRIBL_HOST` |

#### `auth login` Examples {#auth-login-examples}

Launch interactive login:

`$CRIBL_HOME/bin/cribl auth login`

Append credentials as command arguments:

`$CRIBL_HOME/bin/cribl auth login -h <url> -u <username> -p <password>`

> All `-h` and `host` arguments are optional, provided that the `cribl.yml` file's `api:` section lists the API host and port.
>
{.box .info}

Provide credentials in environment variables:

`CRIBL_HOST=<url> CRIBL_USERNAME=<username> CRIBL_PASSWORD=<password> $CRIBL_HOME/bin/cribl auth login`

Provide credentials in a file:

`$CRIBL_HOME/bin/cribl auth login -f <path/to/file>`

Corresponding file contents:

```text
host=<url>
username=<username>
password=<password>
```

### `auth logout` {#auth-logout}

Logout from the current session.

**Usage:**

```text
./cribl auth logout
```

### `auth mf` {#auth-mf}

Control multi-factor authentication (PIV).

**Usage:**

```text
./cribl auth mf -e true
```

See additional [`auth mf` Examples](#auth-mf-examples).

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-e <enable>` | Disable or enable multi-factor authentication using PIV. Accepts: ["true", "false"]. |
| `-u <usernameRegex>` | Username regex pattern ( defaults to '/CN=(?:\w+\.)+(\d+)/' ). |
| `-f <usernameField>` | Field from which username is to be extracted ( defaults to 'subject' ). Accepts: [subject,san] |
| `-o <accessControlAllowOrigin>` | Access control allow origin header. Example: "https://127.0.0.1:9000" or "https://fqdn" |
| `-a <apiServerUrl>` |  API server internal FQDN or IP:PORT combo. Make sure this is also part of server TLS certificate SAN->IP or SAN->DNS (such as "https://[IP]:[PORT]" or "https://internal-fqdn"). |

#### `auth mf` Examples {#auth-mf-examples}

Check the status of multi-factor authentication using Personal Identity Verification (PIV):

`$CRIBL_HOME/bin/cribl auth mf`

The response includes the status (disabled or enabled) and the current configuration, along with notifications for unconfigured values and issues like failure to restart or enable TLS. For example:

```
PIV multi-factor authentication is disabled.
Username field not defined. Please make sure to configure it.
Username regex pattern: /(?:UPN:)([^,]+)(?:,|$)/
Access control allow origin header: https://127.0.0.1:9000
Api server url: https://127.0.0.1:9000
You will need to restart instance before your changes take full effect.
Warning: TLS is not enabled. PIV will not function until TLS is successfully configured and enabled.
```

Enable multi-factor authentication login using PIV:

`$CRIBL_HOME/bin/cribl auth mf -e true`

The response confirms that PIV authentication is enabled and lists the current configuration. For example:

```
PIV multi-factor authentication is enabled.
Username field: san
Username regex pattern: /(?:UPN:)([^,]+)(?:,|$)/
Access control allow origin header: https://127.0.0.1:9000
Api server url: https://127.0.0.1:9000
```

Disable multi-factor authentication login using PIV:

`$CRIBL_HOME/bin/cribl auth mf -e false`

The response confirms that PIV authentication is disabled and lists the current configuration. For example:

```
PIV multi-factor authentication is disabled.
Username field: san
Username regex pattern: /(?:UPN:)([^,]+)(?:,|$)/
Access control allow origin header: https://127.0.0.1:9000
Api server url: https://127.0.0.1:9000
```

For a detailed example that uses the mf sub-command to configure and enable PIV authentication, see [Personal Identity Verification (PIV) Authentication](authentication#piv).
