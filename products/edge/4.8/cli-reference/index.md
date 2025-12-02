# CLI Reference


Command line interface basics

---

In addition to starting and stopping the Cribl Edge server, the Cribl Edge command line interface enables you to initiate many configuration and administrative tasks directly from your terminal.

## Command Syntax

To execute CLI commands, the basic syntax is:

```text
cd $CRIBL_HOME/bin
./cribl <command> <sub-command> <options> <arguments>
```

Not all commands have sub-commands.

To see help for any command, append the `--help` option, for example:

```text
./cribl vars --help

./cribl vars get --help

./cribl vars get -i myArray --help
```

The `scope` command is an exception: it has no `--help` option, but it has [its own CLI Reference](https://appscope.dev/docs/cli-reference) in the AppScope documentation. 

## Avoiding Surprises

### Immediate Execution

As indicated in the sample output below, some commands take effect immediately.

Commands that require further input will echo the sub-commands, options, and arguments they expect.

### Persistent Volumes

If you start Cribl Edge with the `CRIBL_VOLUME_DIR` variable, all subsequent CLI commands should have this variable defined. Otherwise, those commands will apply the Cribl Edge default directories, yielding misleading results.

You can set `CRIBL_VOLUME_DIR` as an [environment variable](environment-variables), or you can explicitly include it in each command, as in this example: 

`CRIBL_VOLUME_DIR=<writable-path-name> /opt/cribl/bin/cribl status`

When set, `$CRIBL_VOLUME_DIR` overrides `$CRIBL_HOME`.

Avoid setting `$CRIBL_VOLUME_DIR` to an existing folder because it creates predefined folders in the specified directory. If that directory already contains folders with those names, they will be overwritten.

### Failover Mode

In failover mode, set the `CRIBL_CONF_DIR` variable to your failover directory, ensuring your CLI commands access the correct configuration directory. For example, `CRIBL_CONF_DIR=/<path_to_failover_volume> cribl diag create`.

Your failover directory is either defined by the [environment variable](environment-variables) `CRIBL_DIST_LEADER_FAILOVER_VOLUME` (alternatively `CRIBL_DIST_MASTER_FAILOVER_VOLUME`), or defined in the [instance.yml](instanceyml) configuration file.

## Commands Available

To see a list of available commands, enter `./cribl` alone (or the equivalent `./cribl help`). To execute a command, or to see its required parameters, enter `./cribl <command>`.

### `help`

Displays a list of commands with a description (help) for each. Defaults to a selection of generally useful commands.

#### Usage

`./cribl help [-a]`

#### Options

```text
-a              - Display the list of all commands, except for `scope`.
```

#### Sample Response

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

auth              - Authentication
boot-start        - Enable/Disable boot-start
diag              - Manage diagnostics bundles
encrypt           - Encrypt with cribl secret
git               - Manage Worker Groups config
keys              - Manage encryption keys
nc                - Listen on a port for traffic and output stats and data
node              - Execute a JavaScript file
pack              - Manage Cribl Packs
parquet           - Manage Parquet files and schemas
pipe              - Feed stdin to a pipeline
vars              - Manage global variables
```

> Starting in version 3.0, former "master" application components have been renamed "Leader." As long as some legacy terminology remains within CLI commands/​options, configuration keys/values, and environment variables, this document will reflect the options available.
>
{.box .info}

### `limits`

Controls the availablity of Cribl features.

#### Usage

`./cribl limits <options> <args>`

#### Options

```text
[-s <scripts>] - Enable custom scripts API
```

### `mode-master`

Configures instance as a Leader.

#### Usage

`./cribl mode-master <options> <args>`

#### Options

```text
[-d <deploymentId>]     - Deployment ID for reporting telemetry on multiple deployments
[-H <host>]             - Hostname/IP (defaults to 0.0.0.0).
[-p <port>]             - Port (defaults to 4200).
[-n <certName>]         - Name of the saved certificate to use. Mutually exclusive with -k or -c.
[-k <privKeyPath>]      - Path on server to the private key to use. PEM format. Can reference $ENV_VARS.
[-c <certPath>]         - Path on server to the certificate to use. PEM format. Can reference $ENV_VARS.
[-u <authToken>]        - Authentication token to include as part of the connection header.
[-S <tls>]              - Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"].
[-i <ipWhitelistRegex>] - Regex matching IP addresses that are allowed to establish a connection.
[-r <resiliency>]       - Resiliency mode for the Leader. Accepts: ["none", "failover"].
[-v <failoverVolume>]   - For resiliency=failover, the volume to use as shared storage.
```

### `mode-single`

Configures instance as a Single-instance deployment.

#### Usage

`./cribl mode-single [--help]`

#### Options

```text
[-d <deploymentId>] - Deployment ID for reporting telemetry on multiple deployments
```

### `mode-edge`

Configures Cribl Edge as a Single-instance deployment.

#### Usage

`./cribl mode-edge [--help]`

#### Options

```text
[-d <deploymentId>] - Deployment ID for reporting telemetry on multiple deployments
[-H <host>]         - Hostname/IP (defaults to 127.0.0.1).
[-p <port>]         - Port (defaults to 9420).
[-s <socket>]       - Location of the AppScope socket.
```

### `mode-worker` {#mode-worker}

Configures Cribl Edge as a Worker instance.

#### Usage

`./cribl mode-worker -H <host> -p <port>  <options> <args>`

The `-H <host> -p <port>` parameters are required.

#### Options

```text
[-d <deploymentId>] - Deployment ID for reporting telemetry on multiple deployments
 -H <host>          - Leader's hostname or IP address.
 -p <port>          - Leader's cluster communications port (e.g.: 4200).
[-n <certName>]     - Name of the saved certificate to use. Mutually exclusive with -k or -c.
[-k <privKeyPath>]  - Path on server to the private key to use. PEM format. Can reference $ENV_VARS.
[-c <certPath>]     - Path on server to the certificate to use. PEM format. Can reference $ENV_VARS.
[-u <authToken>]    - Authentication token to include as part of the connection header.
[-S <tls>]          - Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"].
[-e <envRegex>]     - Regex to select environment variables to report to Leader. E.g.: "/test/".
[-t <tags>]         - Tag values to report to Leader.
[-g <group>]        - Worker Group to report to Leader.
```

When you [generate bootstrap scripts](/stream/deploy-workers#ui) via the UI to add or update Workers, these scripts automatically set the `-S` option according to the [Leader TLS configuration](securing-communications#tls-leader).

### `mode-managed-edge`

Configures Cribl Edge as an Edge Node.

#### Usage

`./cribl mode-managed-edge -H <host> -p <port>  <options> <args> && ./cribl restart`

The `-H <host> -p <port>` parameters are required.

#### Options

```text
[-d <deploymentId>] - Deployment ID for reporting telemetry on multiple deployments
 -H <host>          - Leader's hostname or IP address.
 -p <port>          - Leader's cluster communications port (e.g.: 4200).
[-n <certName>]     - Name of the saved certificate to use. Mutually exclusive with -k or -c.
[-k <privKeyPath>]  - Path on server to the private key to use. PEM format. Can reference $ENV_VARS.
[-c <certPath>]     - Path on server to the certificate to use. PEM format. Can reference $ENV_VARS.
[-u <authToken>]    - Authentication token to include as part of the connection header.
[-S <tls>]          - Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"].
[-e <envRegex>]     - Regex to select environment variables to report to Leader. E.g.: "/test/".
[-t <tags>]         - Tag values to report to Leader.
[-g <group>]        - Fleet to report to Leader.
```

When you [generate bootstrap scripts](/edge/managing-edge-nodes#bootstrap) via the UI to add or update Workers, these scripts automatically set the `-S` option according to the [Leader TLS configuration](securing-communications#tls-leader).

### `pack`

Manages [Cribl Packs](https://docs.cribl.io/docs/packs).

#### Usage

`./cribl pack <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
export            - Export Cribl Packs, args:
   -m <mode>      - Mode to export. Accepts: merge_safe, merge, default_only.
  [-o <filename>] - Where to export the Pack on disk.
  [-n <name>]     - Name to override the installed Pack's name on export.
  [-g <group>]    - The Worker Group/Fleet to execute within.
install           - Install a Cribl Pack, args:
  [-d]            - Run install in debug.
  [-f]            - Force install.
  [-c]            - Disallow installation of Packs with custom functions.
  [-n <name>]     - Name of the Pack to install, defaults to source.
  [-g <group>]    - The Worker Group/Fleet to execute within
list              - List Cribl Packs, args:
  [-v]            - Display all Pack info
  [-g <group>]    - The Worker Group/Fleet to execute within
uninstall         - Uninstall a Cribl Pack, args:
  [-d]            - Run uninstall in debug
  [-g <group>]    - The Worker Group/Fleet to execute within
upgrade           - Upgrade a Cribl Pack, args:
  [-d]            - Run upgrade in debug
  [-s <source>]   - Provide the Pack source
  [-m <minor>]    - Only upgrade to minor version
  [-g <group>]    - The Worker Group/Fleet to execute within
```

#### Sample Response

```text
id          version  spec  displayName    author       description                          source                            
------------------------------------------------------------------------------------------------------------------------------
HelloPacks  1.0.0    ----  Hello, Packs!  Cribl, Inc.  A sample pack with a simple example  file:/opt/cribl/default/HelloPacks
```

### `parquet`

For any Parquet file: view (`cat`) the file, its metadata (`details`), or its `schema`. This command is available only on Linux.

#### Usage

`./cribl parquet <sub-command> -f <file>`

#### Sub-commands and Options

```text
cat             - Cat parquet events to console, args:
   -f <file>    - Path to parquet file
  [-m <max>]    - Maximum events to print
details         - Print all parquet details (schema and statistics), args:
   -f <file>    - Path to parquet file
schema          - Print parquet schema, args:
   -f <file>    - Path to parquet file
```

#### Examples

View a Parquet file:

`./cribl parquet cat -f /some_parquet_file`

```shell
{"__bytes":274,"Unnamed: 0":0,"name":"apples","quantity":10,"price":2.6,"date":"2018-04-02 20:29:00","day":"2017-11-26","finger":"b'FNORD'","inter":"b'*\\x00\\x00\\x00\\x17\\x00\\x00\\x00\\t\\x03\\x00\\x00'","stock":"[{'quantity': array([10]), 'warehouse': 'A'}\n {'quantity': array([20]), 'warehouse': 'B'}]","colour":"['green' 'red']","meta_json":null}
...
```

View a Parquet file's metadata, including encoding and compression:

`./cribl parquet details -f /some_parquet_file`

```shell
{
  "schema": {
    "Unnamed: 0": {
      "optional": true,
      "type": "INT64"
    },
    ...
  },
  "path": "../../../src/sluice/js/util/__tests__/data/parquet/single/small_fruits.parquet",
  "num_rows": 40000,
  "num_cols": 11,
  "num_real_cols": 11,
  "num_row_groups": 1,
  "created_by": "parquet-cpp-arrow version 11.0.0",
  "schema_root_name": "schema",
  "metadata": {
    "pandas": "{\"index_columns\": [{\"kind\": \"range\", \"name\": null, \"start\": 0, \"stop\": 40000, \"step\": 1}],
    ...
  },
  "version": "PARQUET_2_6",
  "metadataSize": 6072,
  ...
    }
  ]
}
```

View a Parquet file's schema (not including encoding or compression), expressed as JSON like in the [Parquet Schema editor](parquet-schemas#the-parquet-schema-editor):

`./cribl parquet schema -f /some_parquet_file`

```shell
{
  "Unnamed: 0": {
    "optional": true,
    "type": "INT64"
  },
  "name": {
    "optional": true,
    "type": "STRING"
  },
  "quantity": {
    "optional": true,
    "type": "DOUBLE"
  },
  "price": {
    "optional": true,
    "type": "DOUBLE"
...
  }
}
```

### `reload`

Reloads Cribl Edge. Executes immediately.

#### Usage

`./cribl reload [--help]`

#### Sample Response

```text
Reload request submitted to Cribl
```

### `restart` 

Restarts Cribl Edge. Executes immediately.

> Executing this command cancels any running [collection jobs](/stream/collectors).
>
{.box .warning}

#### Usage

`./cribl restart [--help]`

#### Sample Response

```text
Stopping Cribl, process 17
..............
Cribl is not running
Starting...
....
Cribl started
```

### `start`

Starts Cribl Edge. Executes immediately. Upon first run, echoes the Cribl Edge default login credentials.

#### Usage

`./cribl start <options> <args>`

#### Options

```text
[-d <dir>]  - Configuration directory
[-r <role>] - Process role
```

#### Sample Response

```text
Starting...
...
Cribl started
```

### `status`

Displays status of Cribl Edge, including the API Server address, instance's mode (Leader or Worker), process ID, and GUID (fictitious example below). Executes immediately.

#### Usage

`./cribl status [--help]`

#### Sample Response

```text
Address: http://172.17.0.3:9000
Mode: master
Status: Up
Software Version: 3.1.0-f765e418
Config Version: 347079c
Master: 0.0.0.0:4200
PID: 4100
GUID: e706052a-ace9-4511-a7c7-b58a414a07d3
```

### `stop`

Stops Cribl Edge. Executes immediately.

> Executing this command cancels any running [collection jobs](/stream/collectors).
>
{.box .warning}

#### Usage

`./cribl stop [--help]`

#### Sample Response

```text
Stopping Cribl, process 3951
............
Cribl is not running
```

### `version` 

Displays Cribl Edge version. Executes immediately.

#### Usage

`./cribl version [--help]`

#### Sample Response

```text
Software Version: 4.7.3-6f48361f
```

### `auth`

Log into or out of Cribl Edge.

#### Usage

`./cribl auth <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
login             - Log in, args:
  [-H <host>]     - Host URL (e.g. http://localhost:9000)
  [-u <username>] - Username
  [-p <password>] - Password
  [-f <file>]     - File with credentials
logout            - Log out
```

Arguments of the `login` subcommand correspond to the following environment variables:

| **Argument** | **Variable** |
| --- | --- |
| `<username>` | `CRIBL_USERNAME` |
| `<password>` | `CRIBL_PASSWORD` |
| `<host>` | `CRIBL_HOST` |

#### Login Examples

Launch interactive login:

`$CRIBL_HOME/bin/cribl auth login`

Append credentials as command arguments:

`$CRIBL_HOME/bin/cribl auth login -h <url> -u <username> -p <password>`

> All `-h` and `host` arguments are optional, provided that the `cribl.yml` file's `api:` section lists the API host and port.
>
{.box .info}

Provide credentials in environment variables:

`CRIBL_HOST=<url> CRIBL_USERNAME=<username> CRIBL_PASSWORD=<password> $CRIBL_HOME/bin/cribl auth login`

Provide credentials in a file:

`$CRIBL_HOME/bin/cribl auth login -f <path/to/file>`

Corresponding file contents: 

```text
host=<url>
username=<username>
password=<password>
```

### `boot-start` 

Enables or disables Cribl Edge boot-start.

#### Usage

`./cribl boot-start <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
disable             - Disable boot-start, args:
  [-m <manager>]    - Init manager (systemd|initd)
  [-c <configDir>]  - Config directory for the init manager
enable              - Enable boot-start, args:
  [-m <manager>]    - Init manager (systemd|initd)
  [-u <user>]       - User to run the instance as
  [-c <configDir>]  - Config directory for the init manager
```

#### Sample Response

```text
Enabling Cribl to be managed by initd...
boot-start enable command needs root privileges...
Enabled Cribl to be managed by initd as user=root.
```

### `cloud-workspace`

Updates an on-prem Cribl Edge Leader with a new config in the `instance.yml` file called `cloudWorkspace`.
With this config, the local Cribl Edge Leader will connect to the Cribl.Cloud Leader and send it usage metrics.

#### Usage

`./cribl cloud-workspace -H <host> -p <port> # etc: flags`

#### Options

```text
[-d <deploymentId>]     - Deployment ID used to report telemetry on multiple deployments.
[-H <host>]             - Hostname/IP (defaults to 0.0.0.0).
[-p <port>]             - Port (defaults to 4200).
[-n <certName>]         - Name of the saved certificate to use. Mutually exclusive with -k or -c.
[-k <privKeyPath>]      - Path on server to the private key to use. PEM format. Can reference $ENV_VARS.
[-c <certPath>]         - Path on server to the certificate to use. PEM format. Can reference $ENV_VARS.
[-u <authToken>]        - Authentication token to include as part of the connection header.
[-S <tls>]              - Sets, or disables, secure communication between Leader and Worker/Edge Nodes via TLS. Accepts: ["true", "false"].
[-i <ipWhitelistRegex>] - Regex matching IP addresses that are allowed to establish a connection.
[-r <resiliency>]       - Resiliency mode on the Leader. Accepts: ["none", "failover"].
[-v <failoverVolume>]   - For resiliency=failover, the volume to use as shared storage.
```

### `diag` {#diag}

Manages diagnostic bundles. For failover mode, see how to [avoid surprises](#failover-mode).

#### Usage

`./cribl diag <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
create                  - Creates diagnostic bundle for this instance, args:
  [-d]                  - Run create in debug mode
  [-j]                  - Do not append '.txt' to js files
  [-t <maxIncludeJobs>] - Latest number of jobs to include in bundle
  [-M]                  - Exclude metrics from bundle
  [-g]                  - Exclude git from bundle
  [-i]                  - Include install logs
list                    - List existing diagnostic bundles
send                    - Send diagnostics bundle to Cribl Support, args:
   -c <caseNumber>      - Cribl Support Case Number
  [-p <path>]           - Diagnostic bundle path (if empty then new bundle will be created)
heapsnapshot            - Generate heap snapshot of a Cribl process, args:
   -p <pid>             - The pid of the process to dump the heap snapshot
```

#### Sample Responses

##### `diag create`

```text
Created a Cribl diagnostic bundle at /opt/cribl/diag/<product>-zedborcdb72f-20210820T204405.tar.gz
```

##### `diag heapsnapshot -p 12345`

The response format is `Heap-<epoch-timestamp>-<pid>.heapsnapshot`:

```text
Heap-1672574400000-12345.heapsnapshot
```
### `encrypt` {#encrypt}

Encrypts data with a secret key. 

#### Usage

` encrypt -v my_token -s <path to secret key>`

#### Sub-commands and Options 

```text
 -v <value>            - Value to encrypt.
[-s <criblSecretPath>] - cribl.secret encrypt key path. If you do not specify a cribl.secret file with -s options, the default cribl.secret is used.
```

#### Sample Response

```text
LzmJ3uqA6WXtXjaLPwuM++WX84o3fPxBaubA6Q0look=
```


### `git` {#git}

Manages Worker Group/Fleets'/Fleets' configuration.

#### Usage

`./cribl git <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
commit           - Commit, args:
  [-g <group>]   - Group ID.
  [-m <message>] - Commit message.
commit-deploy    - Commit and Deploy, args:
   -g <group>    - Group ID.
  [-m <message>] - Commit message.
deploy           - Deploy, args:
   -g <group>    - Group ID.
  [-v <version>] - Deploy version.
list-groups      - List Worker Groups/Fleets.
```
#### Sample Response

```text
Successfully committed version 7c04de1
```

### `groups`

Deprecated. See [`git`](#git).


### `keys`

Manages encryption keys. You must append the `-g <group>` argument to specify a Worker Group/Fleet. As a fallback, append the argument `-g default`, for example:
`./cribl keys list -g default`

#### Usage

`./cribl keys <sub-command> <options>  <args> -g <group> `

#### Sub-commands and Options

```text
add                - Add encryption keys, args:
  [-a <algorithm>] - Encryption algorithm. Supported values: aes-256-cbc (default), aes-256-gcm.
  [-c <keyclass>]  - Key class to set for the key.
  [-k <kms>]       - KMS to use. Must be configured; see cribl.yml.
  [-e <expires>]   - Expiration time, in epoch time.
  [-i]             - Use an initialization vector. (IV size configurable for algorithm aes-256-gcm & fixed to 16 for algorithm aes-256-cbc)
  [-s <ivSize>]    - (For algo 'aes-256-gcm' only) Initialization vector size (bytes). Supported values: 12 (default), 13, 14, 15, 16
  [-g <group>]     - Worker Group ID.
list               - List encryption keys, args:
  [-g <group>]     - Worker Group ID.
```
#### Sample Response

```text
Adding key succeeded. Key count=1
```

### `nc`

Listens on a port for traffic, and outputs stats and data. (Netcat-like utility.)

#### Usage

`./cribl nc -p <port> <options>  <args>`

#### Options

```text
 -p <port>           - Port to listen on.
[-f <family>]        - If this is "6" then nc will listen on ::1, otherwise it listens on 0.0.0.0
[-s <statsInterval>] - Stats output interval (ms), use 0 to disable.
[-u]                 - Listen on UDP port instead.
[-o]                 - Output received data to stdout.
[-t <throttle>]      - throttle rate in (unit)/sec, where units can be KB,MB,GB, and TB.
```

#### Sample Response

```text
2021-08-20T22:44:30.457Z - starting server on 0.0.0.0:9999
2021-08-20T22:44:30.462Z - server listening 0.0.0.0:9999
2021-08-20T22:44:31.461Z - messages: 0, socks: 0, thruput: 0MBps
2021-08-20T22:44:32.466Z - messages: 0, socks: 0, thruput: 0MBps
...
2021-08-20T22:44:39.212Z - got connection: 127.0.0.1:37190
2021-08-20T22:44:39.213Z - got connection: 127.0.0.1:37192
```

### `node`

Run with no options, displays a command prompt, as shown here:

```text
> 
```
To execute a JavaScript file, you can enter path/file name at the prompt.

With the `-v` option, prints the version of NodeJS that is running.

With `-e`, evaluates a string. Write to console to see the output, for example: 

```text
./cribl node -e 'console.log(Date.now())'
1629740667695
```

#### Usage

`./cribl node <options>  <args>`

#### Options

```text
[-e <eval>] - String to eval
[-v]        - Prints NodeJS version
```

#### Sample Response

```text
v14.15.1
```

### `pipe`

Feeds stdin to a Pipeline.

#### Usage

`./cribl pipe -p <pipelineName>  <options>  <args>`

Examples:

```text
cat sample.log |  ./cribl pipe -p <pipelineName> 
cat sample.log |  ./cribl pipe -p <pipelineName> 2>/dev/null
```

#### Options

```text
 -p <pipeline>    - Pipeline to feed data thru
[-d]              - Include dropped events
[-c <cpuProfile>] - Perform CPU profiling
[-t]              - Perform pipeline tracing
[-a <pack>]       - Optional Cribl Pack context. Mutually exclusive with -b.
[-b <project>]    - Optional Cribl Project context
[-i]              - Runs the pipe command without sandboxing javascript expressions from potential attackers
```

#### Sample Response

```text
...
{"time":"2021-08-20T20:37:00.017Z","cid":"api","channel":"commands","level":"info","message":"creating new pipeline","id":"main","conf":{"asyncFuncTimeout":1000,"functions":[{"id":"eval","disabled":false,"filter":"true","conf":{"add":[{"name":"cribl","value":"'yes'"}],"remove":[]}}]}}
{"time":"2021-08-20T20:37:00.019Z","cid":"api","channel":"pipe:main","level":"info","message":"start loading and initializing functions","count":1}
{"time":"2021-08-20T20:37:00.021Z","cid":"api","channel":"pipe:main","level":"info","message":"finished loading and initializing functions","count":1}
{"time":"2021-08-20T20:37:00.022Z","cid":"api","channel":"commands","level":"info","message":"START pushing stdin events","id":"main"}
{"time":"2021-08-20T20:37:00.028Z","cid":"api","channel":"GrokMgr","level":"info","message":"loaded grok patterns","count":152}
...
```

### `scope` 

Greps your apps by the syscalls. Executes immediately.

See the [AppScope CLI Reference](https://appscope.dev/docs/cli-reference) for usage and examples.

### `vars`

Manages Cribl Edge [Global Variables](global-variables-library).

#### Usage

`./cribl vars <sub-command> <options>  <args>`

#### Sub-commands and Options

```text
Sub-commands:
add                  - Add global variable, args:
   -i <id>           - Global variable ID
   -t <type>         - Type
   -v <value>        - Value
  [-a <args>]        - Arguments
  [-d <description>] - Description
  [-c <tags>]        - Custom Tags (comma separated list)
  [-g <group>]       - Group ID
get                  - List global variables, args:
  [-i <id>]          - Global variable ID
  [-g <group>]       - Group ID
remove               - Remove global variable, args:
   -i <id>           - Global variable ID
  [-g <group>]       - Group ID
update               - Update global variable, args:
   -i <id>           - Global variable ID
  [-t <type>]        - Type
  [-v <value>]       - Value
  [-a <args>]        - Arguments
  [-d <description>] - Description
  [-c <tags>]        - Custom Tags (comma separated list)
  [-g <group>]       - Group ID
```

#### Sample Response

```text
[
  {
    "type": "number",
    "lib": "cribl",
    "description": "Sample number variable ",
    "value": "42",
    "tags": "cribl,sample",
    "id": "theAnswer"
  }
]
```
