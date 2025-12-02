# encrypt


Encrypts data with a secret key. See the related [decrypt](cli-decrypt) command.

**Usage:**

```text
./cribl encrypt -v my_token -s my-path-name
```

**Sample response:**

```text
LzmJ3uqA6WXtXjaLPwuM++WX84o3fPxBaubA6Q0look=
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-v <value>` | Value to encrypt. |
| `-s <criblSecretPath>` | The cribl.secret encrypt key path. If you do not specify a cribl.secret file with -s options, the default cribl.secret is used. |