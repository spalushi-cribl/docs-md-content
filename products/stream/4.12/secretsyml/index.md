# secrets.yml


`secrets.yml` stores secrets for Cribl Stream.

The secrets are decrypted and encrypted using a `cribl.secret` file.
`cribl.secret` is unique per Worker Group and resides in the `$CRIBL_HOME/groups/<groupName>/local/cribl/auth` directory on Leader nodes,
or in `$CRIBL_HOME/local/cribl/auth` for Workers or single-instance deployments.

> Cribl automatically decrypts plaintext secrets from secrets.yml on startup and stores them in memory, keeping your data secure.
>
{.box .info}

```yaml title="$CRIBL_HOME/local/cribl/secrets.yml"
<secret-id>:
  secretType: # [string] One of: keypair | text | credentials
  secrets:
    # -------------- if secretType is keypair ---------------
    apiKey: "<encrypted-value>"
    secretKey: "<encrypted-value>"

    # -------------- if secretType is text ------------------ 
    value: "<encrypted-value>"

    # -------------- if secretType is credentials ----------- 
    username: "<encrypted-value>"
    password: "<encrypted-value>"
```