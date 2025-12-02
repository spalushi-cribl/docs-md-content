# secrets.yml


`secrets.yml` stores secrets for Cribl Edge.

The secrets are decrypted and encrypted using a `cribl.secret` file.
`cribl.secret` is unique per Fleet and resides in the `$CRIBL_HOME/groups/<group-name>/local/cribl/auth` directory on Leader nodes,
or in `$CRIBL_HOME/local/cribl/auth` for Workers or single-instance deployments.

```yaml title="$CRIBL_HOME/local/cribl/secrets.yml"
<secret-id>:
  secretType: # [string] One of: keypair | text | credentials
  secrets:
    # -------------- if secretType is keypair ---------------
    apiKey: "<hashed-value>"
    secretKey: "<hashed-value>"

    # -------------- if secretType is text ------------------ 
    value: "<hashed-value>"

    # -------------- if secretType is credentials ----------- 
    username: "<hashed-value>"
    password: "<hashed-value>"
```