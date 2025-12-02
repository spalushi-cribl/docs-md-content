# CLI Reference


Command line interface basics and reference

---

In addition to starting and stopping the Cribl Stream server, the Cribl Stream command line interface enables you to initiate many configuration and administrative tasks directly from your terminal.

> The command-line interface is only available to on-prem or hybrid customers.
>
{.box .info}

To use the CLI:

1. First check that you have read/write access to the directory where Cribl Stream is installed. Contact your Cribl administrator to request access if needed.

1. Navigate to the directory where Cribl Stream is installed:

   ```text
   cd $CRIBL_HOME/bin
   ```

1. Execute a command. CLI commands have this basic syntax:

   ```text
   ./cribl <command> <sub-command> <options> <arguments>
   ```

   > Not all commands have sub-commands.
   >
   {.box .info}

1. To see the documentation for any command, append the `--help` option. For example:

   ```text
   ./cribl vars --help

   ./cribl vars get --help

   ./cribl vars get -i myArray --help
   ```

## Prevent and Troubleshoot CLI Problems

### Immediate Execution

As indicated in the command reference and sample output, some commands take effect immediately.

Commands that require additional input will echo the sub-commands, options, and arguments they expect.

### Persistent Volumes

If you start Cribl Stream with the `CRIBL_VOLUME_DIR` variable, all subsequent CLI commands should have this variable defined. Otherwise, those commands will apply the Cribl Stream default directories, yielding misleading results.

You can set `CRIBL_VOLUME_DIR` as an [environment variable](environment-variables), or you can explicitly include it in each command, as in this example:

`CRIBL_VOLUME_DIR=<writable-path-name> /opt/cribl/bin/cribl status`

When set, `$CRIBL_VOLUME_DIR` overrides `$CRIBL_HOME`.

Avoid setting `$CRIBL_VOLUME_DIR` to an existing folder because it creates predefined folders in the specified directory. If that directory already contains folders with those names, Cribl Stream will overwrite those directories.

### Failover Mode

In failover mode, set the `CRIBL_CONF_DIR` variable to your failover directory, ensuring your CLI commands access the correct configuration directory. For example, `CRIBL_CONF_DIR=/<path_to_failover_volume> cribl diag create`.

Your failover directory is either defined by the [environment variable](environment-variables) `CRIBL_DIST_LEADER_FAILOVER_VOLUME` (alternatively `CRIBL_DIST_MASTER_FAILOVER_VOLUME`), or defined in the [instance.yml](instanceyml) configuration file.

## Commands Available

To see a list of available commands, enter `./cribl` alone (or the equivalent `./cribl help`). To execute a command, or to see its required parameters, enter `./cribl <command>`.


| Command | Description |
| ------- | ----------- |
| [auth](cli-auth) | Command to log into or out of the product |
| [boot-start](cli-boot-start) | Command to enable or disable product boot-start |
| [cloud-workspace](cli-cloud-workspace) | Command to update a Leader with new config that allows it to connect to the Cribl.Cloud Leader and send it usage metrics |
| [decrypt](cli-decrypt) | Command to decrypt data with a secret key |
| [diag](cli-diag) | Command to manage diagnostic bundles |
| [encrypt](cli-encrypt) | Command to encrypt data with a secret key |
| [git](cli-git) | Command to manage Worker Group or Fleet configuration |
| [help](cli-help) | Displays a list of commands and their help |
| [keys](cli-keys) | Command to manage encryption keys |
| [limits](cli-limits) | Command to control the availability of Cribl features |
| [mode-master](cli-mode-master) | Command to configure an instance as a Leader |
| [mode-single](cli-mode-single) | Command to configure an instance as a Single-instance deployment |
| [mode-edge](cli-mode-edge) | Command to configure Cribl Edge as a Single-instance deployment |
| [mode-worker](cli-mode-worker) | Command to configure Cribl Stream as a Worker instance |
| [mode-managed-edge](cli-mode-managed-edge) | Command to configure Cribl Edge as an Edge Node |
| [nc](cli-nc) | Command to listen a port for traffic and output stats and data |
| [node](cli-node) | Command to execute a JavaScript file |
| [pack](cli-pack) | Command to mange Cribl packs |
| [parquet](cli-parquet) | Command to view a Parquet file, its metadata, or its schema |
| [pipe](cli-pipe) | Command to feed stdin to a Pipeline |
| [reload](cli-reload) | Command to reload the product |
| [restart](cli-restart) | Command to restart the product |
| [scope](cli-scope) | Scope a Linux Command via AppScope |
| [start](cli-start) | Command to start the product |
| [status](cli-status) | Command to display the product status |
| [stop](cli-stop) | Command to stop the product |
| [vars](cli-vars) | Command to manage global variables |
| [version](cli-version) | Command to display product version |
