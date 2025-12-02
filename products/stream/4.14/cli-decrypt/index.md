# decrypt


Decrypts data with a secret key. See the related [encrypt](cli-encrypt) command.

**Usage:**

```text
./cribl decrypt -v 12345 -s my-path-name
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-v <value>` | Value to decrypt. |
| `-s <criblSecretPath>` | The cribl.secret decrypt key path. If you do not specify a cribl.secret file with -s options, the default cribl.secret is used. |