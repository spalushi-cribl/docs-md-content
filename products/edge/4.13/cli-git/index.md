# git


Manages Fleet configuration.

**Sub-commands:**

- [`git commit`](#git-commit)
- [`git commit-deploy`](#git-commit-deploy)
- [`git deploy`](#git-deploy)
- [`git list-groups`](#git-list-groups)

## Sub-commands and Options

### `git commit` {#git-commit}

Commit Worker Group or Fleet configuration changes.

**Usage:**

```text
./cribl git commit -g production_logs -m "Add syslog pipeline"
./cribl git commit -g webserver_data -m "Remove Parser function from webhook pipeline"
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-g <group>` | Group ID. |
| `-m <message>` | Commit message. |

### `git commit-deploy` {#git-commit-deploy}

Commit and also deploy Worker Group or Fleet configuration changes.

**Usage:**

```text
./cribl git commit-deploy -g production_logs -m "Add syslog pipeline"
./cribl git commit-deploy -g webserver_data -m "Remove Parser function from webhook pipeline"
```

| Option | Definition |
| ------ | ---------- |
| `-g <group>` | Group ID. |
| `-m <message>` | Commit message. |

### `git deploy` {#git-deploy}

Deploy Worker Group or Fleet configuration changes.

**Usage:**

```text
./cribl git commit-deploy -g production_logs -v 7c04de1
```

**Sample response:**

```text
Successfully committed version 7c04de1
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-g <group>` | Group ID. |
| `-v <version>` | Deploy version. |


### `git list-groups` {#git-list-groups}

List Worker Groups or Fleets.