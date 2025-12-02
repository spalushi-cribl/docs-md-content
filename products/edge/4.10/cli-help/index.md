# help


Displays a list of commands with a description (help) for each. Defaults to a selection of generally useful commands.

**Usage:**

```text
./cribl help [-a]
```

**Sample response:**

```text
Software version: 4.4.2
Usage: [sub-command] [options] [args]

Commands:
help              - Display help
mode-edge         - Configure instance in Edge mode
mode-managed-edge - Configure instance in Managed-Edge mode
mode-master       - Configure instance in Leader mode
mode-single       - Configure instance in Single-Instance mode
mode-worker       - Configure instance in Worker mode
reload            - Reload instance
restart           - Restart instance
start             - Start instance
status            - Display status
stop              - Stop instance
version           - Print version
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-a` | Display the list of all commands, except for `scope`. |