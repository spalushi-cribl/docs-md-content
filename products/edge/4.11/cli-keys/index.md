# keys


Manages encryption keys.

> You must append the `-g <group>` argument to specify a Worker Group/Fleet. As a fallback, append the argument `-g default`. For example: `./cribl keys list -g default`.
>
{.box .info}

**Sub-commands:**

- [`keys add`](#keys-add)
- [`keys list`](#keys-list)

## Sub-commands and Options

### `keys add` {#keys-add}

Add encryption keys.

**Usage:**

```text
./cribl keys add -g default
./cribl keys add -g production_logs
```

**Sample response:**

```text
Adding key succeeded. Key count=1
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-a <algorithm>` | Encryption algorithm. Supported values: aes-256-cbc (default), aes-256-gcm. |
| `-c <keyclass>` | Key class to set for the key. |
| `-k <kms>` | KMS to use. Must be configured; see cribl.yml. |
| `-e <expires>` | Expiration time, in epoch time. |
| `-i` | Use an initialization vector. (IV size configurable for algorithm aes-256-gcm & fixed to 16 for algorithm aes-256-cbc) |
| `-s <ivSize>` | (For algo 'aes-256-gcm' only) Initialization vector size (bytes). Supported values: 12 (default), 13, 14, 15, 16 |
| `-g <group>` | Worker Group ID. |


### `keys list` {#keys-list}

List encryption keys.

**Usage:**

```text
./cribl keys add -g default
./cribl keys add -g production_logs
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-g <group>` | Worker Group ID. |