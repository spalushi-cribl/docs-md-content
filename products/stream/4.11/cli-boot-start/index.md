# boot-start


Enables or disables Cribl Stream boot-start.

**Sub-commands:**

- [`boot-start disable`](#boot-start-disable)
- [`boot-start enable`](#boot-start-enable)

## Sub-commands and Options

### `boot-start disable` {#boot-start-disable}

Disable boot-start.

**Usage:**

```
./cribl boot-start disable -m initd
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-m <manager>` | Init manager. Accepts: `systemd` or `initd`. |
| `-c <configDir>` | Config directory for the init manager. |

### `boot-start enable` {#boot-start-enable}

Enable boot-start.

**Usage:**
```
./cribl boot-start enable -m initd
```

**Sample response:**

```text
Enabling Cribl to be managed by initd...
boot-start enable command needs root privileges...
Enabled Cribl to be managed by initd as user=root.
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-m <manager>` | Init manager. Accepts: `systemd` or `initd`. |
| `-u <user>` | User to run the instance as. |
| `-c <configDir>` | Config directory for the init manager. |
