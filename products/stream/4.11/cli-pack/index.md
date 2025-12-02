# pack


Manages [Cribl Packs](https://docs.cribl.io/docs/packs).

**Sub-commands:**

- [`pack export`](#pack-export)
- [`pack install`](#pack-install)
- [`pack list`](#pack-list)
- [`pack uninstall`](#pack-uninstall)
- [`pack upgrade`](#pack-upgrade)

## Sub-commands and Options

### `pack export` {#pack-export}

Exports Cribl packs.

**Usage:**

```text
./cribl pack export -m merge
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-m <mode>` | Mode to export. Accepts: merge_safe, merge, default_only. |
| `-o <filename>` | Where to export the Pack on disk. |
| `-n <name>` | Name to override the installed Pack's name on export. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack install` {#pack-install}

Install a Cribl Pack.

**Usage:**

```text
./cribl pack export -n HelloPacks
```

**Sample response:**

```text
id          version  spec  displayName    author       description                          source
------------------------------------------------------------------------------------------------------------------------------
HelloPacks  1.0.0    ----  Hello, Packs!  Cribl, Inc.  A sample pack with a simple example  file:/opt/cribl/default/HelloPacks
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run install in debug. |
| `-f` | Force install. |
| `-c` | Disallow installation of Packs with custom functions. |
| `-n <name>` | Name of the Pack to install, defaults to source. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack list` {#pack-list}

Lists Cribl Packs.

**Usage:**

```text
./cribl pack list
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-v` | Display all Pack information in verbose mode. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack uninstall` {#pack-uninstall}

Uninstall a Cribl Pack.

**Usage:**

```text
./cribl pack uninstall
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run uninstall in debug. |
| `-g <group>` | The Worker Group/Fleet to execute within. |

### `pack upgrade` {#pack-upgrade}

Upgrade a Cribl Pack.

**Usage:**

```text
./cribl pack upgrade -s https://packs.cribl.io/packs/cribl-sysdig-events
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-d` | Run upgrade in debug. |
| `-s <source>` | Provide the Pack source. |
| `-m <minor>` | Only upgrade to minor version. |
| `-g <group>` | The Worker Group/Fleet to execute within. |