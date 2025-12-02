# vars


Manages Cribl Stream global variables.

See [Global Variables](global-variables-library) for more information.

**Sub-commands:**

- [`vars add`](#vars-add)
- [`vars get`](#vars-get)
- [`vars remove`](#vars-remove)
- [`vars update`](#vars-update)

## Sub-commands and Options

### `vars add` {#vars-add}

Add a global variable.

**Usage:**

```text
./cribl vars add -i exampleVariable -v 42 -t number -d "Sample number variable" -c cribl,sample
```

**Sample response:**

```text
[
  {
    "type": "number",
    "lib": "cribl",
    "description": "Sample number variable",
    "value": "42",
    "tags": "cribl,sample",
    "id": "exampleVariable"
  }
]
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-i <id>` | Global variable ID. |
| `-t <type>` | Type. |
| `-v <value>` | Value. |
| `-a <args>` | Arguments. |
| `-d <description>` | Description. |
| `-c <tags>` | Custom Tags (comma separated list). |
| `-g <group>` | Group ID. |

### `vars get` {#vars-get}

List global variables.

**Usage:**

```text
./cribl vars get
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-i <id>` | Global variable ID. |
| `-g <group>` | Group ID. |

### `vars remove` {#vars-remove}

Remove global variable.

**Usage:**

```text
./cribl vars remove -i exampleVariable
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-i <id>` | Global variable ID. |
| `-g <group>` | Group ID. |

### `vars update` {#vars-update}

Update global variable.

**Usage:**

```text
./cribl vars update -i exampleVariable -v 2001
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-i <id>` | Global variable ID. |
| `-t <type>` | Type. |
| `v <value>` | Value. |
| `-a <args>` | Arguments. |
| `-d <description>` | Description. |
| `-c <tags>` | Custom Tags (comma separated list). |
| `-g <group>` | Group ID. |