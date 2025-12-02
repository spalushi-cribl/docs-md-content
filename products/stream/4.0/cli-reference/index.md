# CLI Reference


Command line interface basics

---

In addition to starting and stopping the Cribl Stream server, Cribl Stream's command line interface enables you to initiate many configuration and administrative tasks directly from your terminal.

## Command Syntax

To execute CLI commands, the basic syntax is:

```shell
cd $CRIBL_HOME/bin
./cribl <command> <sub-command> <options> <arguments>
```

Not all commands have sub-commands.

To see help for any command, append the `--help` option, for example:

```shell
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

If you start Cribl Stream with the `CRIBL_VOLUME_DIR` variable, all subsequent CLI commands should have this variable defined. Otherwise, those commands will apply Cribl Stream's default directories, yielding misleading results.

You can set `CRIBL_VOLUME_DIR` as an [environment variable](/stream/deploy-distributed#environment-variables), or you can explicitly include it in each command, as in this example: 

`CRIBL_VOLUME_DIR=<writable-path-name> /opt/cribl/bin/cribl status`

Note that `$CRIBL_VOLUME_DIR`, when set, overrides `$CRIBL_HOME`. 


## Commands Available

To see a list of available commands, enter `./cribl` alone (or the equivalent `./cribl help`). To execute a command, or to see its required parameters, enter `./cribl <command>`.

### `help`

Displays a list of commands with a description (help) for each. Defaults to a selection of generally useful commands.

#### Usage

`./cribl help [-a]`

#### Options

```shell
-a              - Display the list of all commands, except for `scope`.
```

#### Sample Response

```shell
Software version: 3.4.0
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
git               - Manage Worker Groups config
keys              - Manage encryption keys
nc                - Listen on a port for traffic and output stats and data
node              - Execute a JavaScript file
pack              - Manage Cribl Packs
pipe              - Feed stdin to a pipeline
vars              - Manage global variables
```



> As of version 3.0, Cribl Stream's former "master" application components are renamed "leader." As long as some legacy terminology remains within CLI commands/​options, configuration keys/values, and environment variables, this document will reflect that.
>
{.box .info}

### `mode-master`

Configures Cribl Stream as a Leader instance.

#### Usage

`./cribl mode-master <options> <args>`

#### Options

```shell
[-H <host>]             - Host (defaults to 0.0.0.0).
[-p <port>]             - Port (defaults to 4200).
[-n <certName>]         – Name of saved certificate. Mutually exclusive with -k or -c.
[-k <privKeyPath>]      – Path on server to the private key to use. PEM format. Can reference $ENV_VARS. Requires -c, to successfully connect to the Leader. Mutually exclusive with -n.
[-c <certPath>]         – Path on server to the certificate to use. PEM format. Can reference $ENV_VARS. Requires -k, to successfully connect to the Leader. Mutually exclusive with -n.
[-u <authToken>]        - Optional authentication token to include as part of the connection header.
[-i <ipWhitelistRegex>] – Regex matching IP addresses that are allowed to establish a connection.
[-r <resiliency>]       – Resiliency mode for the Leader. Accepts agruments: none, failover.
[-v <failoverVolume>]   – If the ‑r option is set to failover, this defines the volume to use as shared storage.
```

#### Sample Response

```shell
Settings updated.
You will need to restart Cribl Stream before your changes take full effect.
```

### `mode-single`

Configures Cribl Stream as a single-instance deployment.

#### Usage

`./cribl mode-single [--help]`

#### Sample Response

```shell
Settings updated.
You will need to restart Cribl Stream before your changes take full effect.
```

### `mode-edge`

Configures Cribl Edge as a single-instance deployment.

#### Usage

`./cribl mode-edge [--help]`

#### Sample Response

```shell
Settings updated.
```

### `mode-worker` {#mode-worker}

Configures Cribl Stream as a Worker instance.

#### Usage

`./cribl mode-worker -H <host> -p <port>  <options> <args>`

The `-H <host> -p <port>` parameters are required.

#### Options

```shell
-H <host>          – Leader Node's Hostname or IP address.
-p <port>          – Leader Node's cluster communications port (defaults to 4200).
[-S <true|false>]  – Sets, or disables, secure communication between Leader and Worker Nodes via TLS. UI‑generated version-update scripts automatically set this option according to the Leader's configuration.
[-n <certName>]    – Name of saved certificate. Mutually exclusive with -k or -c.
[-k <privKeyPath>] – Path on server to the private key to use. PEM format. Can reference $ENV_VARS. Requires -c, to successfully connect to the Leader. Mutually exclusive with -n.
[-c <certPath>]    – Path on server to the certificate to use. PEM format. Can reference $ENV_VARS. Requires -k, to successfully connect to the Leader. Mutually exclusive with -n.
[-u <authToken>]   – Authentication token to include as part of the connection header. By default, this token is included and is set to 'criblmaster'.
[-e <envRegex>]    – Regex that selects environment variables to report to Leader. Requires slashes to pass validation, e.g., /test/
[-t <tags>]        – Tag values to report to Leader.
[-g <group>]       – Worker Group/Fleet to report to Leader.
```

#### Sample Response

```shell
Settings updated.
You will need to restart Cribl Stream before your changes take full effect.
```

### `mode-managed-edge`

Configures Cribl Edge as an Edge Node.

#### Usage

`./cribl mode-managed-edge -H <host> -p <port>  <options> <args>`

The `-H <host> -p <port>` parameters are required.

#### Options

```shell
-H <host>          – Leader Node's Hostname or IP address.
-p <port>          – Leader Node's cluster communications port (defaults to 4200).
[-S <true|false>]  – Sets, or disables, secure communication between Leader and Worker Nodes via TLS. UI‑generated version-update scripts automatically set this option according to the Leader's configuration.
[-n <certName>]    – Name of saved certificate. Mutually exclusive with -k or -c.
[-k <privKeyPath>] – Path on server to the private key to use. PEM format. Can reference $ENV_VARS. Requires -c, to successfully connect to the Leader. Mutually exclusive with -n.
[-c <certPath>]    – Path on server to the certificate to use. PEM format. Can reference $ENV_VARS. Requires -k, to successfully connect to the Leader. Mutually exclusive with -n.
[-u <authToken>]   – Authentication token to include as part of the connection header. By default, this token is included and is set to 'criblmaster'.
[-e <envRegex>]    – Regex that selects environment variables to report to Leader. Requires slashes to pass validation, e.g., /test/
[-t <tags>]        – Tag values to report to Leader.
[-g <group>]       – Worker Group/Fleet to report to Leader.
```

#### Sample Response

```shell
Settings updated.
```

### `pack`

Manages [Cribl Packs](https://docs.cribl.io/docs/packs).

#### Usage

`./cribl pack <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
export				- Export Cribl Packs, args:
   -m <mode>		- Mode to export. Accepts: merge_safe, merge, default_only.
  [-o <filename>]	- Where to export the pack on disk.
  [-n <name>]		- Name to override the installed pack's name on export.
  [-g <group>]		- The Worker Group/Fleet to execute within
install   			- Install a Cribl Pack, args:
  [-d ]				- Run install in debug.
  [-f ]				- Force install.
  [-n <name>]		- Name of the pack to install; defaults to source.
  [-g <group>]		- The Worker Group/Fleet to execute within.
list				- List Cribl Packs, args:
  [-v ]				- Display all pack info.
  [-g <group>]		- The Worker Group/Fleet to execute within.
uninstall			- Uninstall a Cribl Pack, args:
  [-d ]				- Run uninstall in debug.
  [-g <group>]		- The Worker Group/Fleet to execute within.
upgrade				- Upgrade a Cribl Pack, args:
  [-d ]				- Run upgrade in debug.
  [-s <source>]		- Provide the pack source.
  [-m <minor>]		- Only upgrade to minor version.
  [-g <group>]		- The Worker Group/Fleet to execute within.
```

#### Sample Response

```shell
id          version  spec  displayName    author       description                          source                            
------------------------------------------------------------------------------------------------------------------------------
HelloPacks  1.0.0    ----  Hello, Packs!  Cribl, Inc.  A sample pack with a simple example  file:/opt/cribl/default/HelloPacks
```

### `reload`

Reloads Cribl Stream. Executes immediately.

#### Usage

`./cribl reload [--help]`

#### Sample Response

```shell
Reload request submitted to Cribl
```

### `restart` 

Restarts Cribl Stream. Executes immediately.

> Executing this command cancels any running [collection jobs](/stream/collectors).
>
{.box .warning}

#### Usage

`./cribl restart [--help]`

#### Sample Response

```shell
Stopping Cribl, process 18
............
Cribl Stream is not running
Starting Cribl Stream...
...
Cribl Stream started
```

### `start`

Starts Cribl Stream. Executes immediately. Upon first run, echoes Cribl Stream's default login credentials.

#### Usage

`./cribl start <options> <args>`

#### Options

```shell
[-d <dir>]  - Configuration directory
[-r <role>] - Process role
```

#### Sample Response

```shell
Starting Cribl Stream...
...
Cribl Stream started
```

### `status`

Displays status of Cribl Stream, including the API Server address, instance's mode (Leader or Worker), process ID, and GUID (fictitious example below). Executes immediately.

#### Usage

`./cribl status [--help]`

#### Sample Response

```shell
Cribl Stream Status

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

Stops Cribl Stream. Executes immediately.

> Executing this command cancels any running [collection jobs](/stream/collectors).
>
{.box .warning}

#### Usage

`./cribl stop [--help]`

#### Sample Response

```shell
Stopping Cribl Stream, process 3951
............
Cribl Stream is not running
```

### `version` 

Displays Cribl Stream version. Executes immediately.

#### Usage

`./cribl version [--help]`

#### Sample Response

```shell
Software Version: 3.1.0-f765e418
```

### `auth`

Log into or out of Cribl Stream.

#### Usage

`./cribl auth <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
login             - Log in to Cribl Stream/Edge, args:
  [-h <oldHost>]  - undefined
  [-H <host>]     - Host URL (e.g. http://localhost:9000)
  [-u <username>] - Username
  [-p <password>] - Password
  [-f <file>]     - File with credentials
logout            - Log out from Cribl Stream/Edge
```

#### Login Examples

Launch interactive login:

`$CRIBL_HOME/bin/cribl auth login`

Append credentials as command arguments:

`$CRIBL_HOME/bin/cribl auth login -h <url> -u <username> -p <password>`

> All `-h` and `host` arguments are optional, provided that the API host and port are listed in the `cribl.yml` file's `api:` section.
>
{.box .info}

Provide credentials in environment variables:

`CRIBL_HOST=<url> CRIBL_USERNAME=<username> CRIBL_PASSWORD=<password> $CRIBL_HOME/bin/cribl auth login`

Provide credentials in a file:

`$CRIBL_HOME/bin/cribl auth login -f <path/to/file>`

--

Corresponding file contents: 

```shell
host=<url>
username=<username>
password=<password>
```

### `boot-start` 

Enables or disables Cribl Stream boot-start.

#### Usage

`./cribl boot-start <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
disable             - Disable Cribl Stream/Edge boot-start, args:
  [-m <manager>]    - Init manager (systemd|initd)
  [-c <configDir>]  - Config directory for the init manager
enable              - Enable Cribl Stream/Edge boot-start, args:
  [-m <manager>]    - Init manager (systemd|initd)
  [-u <user>]       - User to run Cribl Stream/Edge as
  [-c <configDir>]  - Config directory for the init manager
```

#### Sample Response

```shell
Enabling Cribl Stream/Edge to be managed by initd...
boot-start enable command needs root privileges...
Enabled Cribl Stream/Edge to be managed by initd as user=root.
```

### `diag`

Manages diagnostic bundles.

#### Usage

`./cribl diag <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
create              - Creates diagnostic bundle for Cribl Stream/Edge, args:
  [-d ]             - Run create in debug mode
  [-j ]             - Do not append '.txt' to js files
  [-M ]             - Exclude metrics from bundle, to reduce file size
list                - List existing Cribl Stream/Edge diagnostic bundles

send                - Send Cribl Stream/Edge diagnostics bundle to Cribl Support, args:
   -c <caseNumber>  - Cribl Support Case Number
  [-p <path>]       - Diagnostic bundle path (if empty then new bundle will be created)
```

#### Sample Response

```shell
Created @[product] diagnostic bundle at /opt/cribl/diag/logstream-zedborcdb72f-20210820T204405.tar.gz
```

### `git` {#git}

Manages Worker Group/Fleets'/Fleets' configuration.

#### Usage

`./cribl git <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
commit            - Commit, args:
  [-g <group>]    - Group ID.
  [-m <message>]  - Commit message.
commit-deploy     - Commit & Deploy, args:
   -g <group>     - Group ID.
  [-m <message>]  - Commit message.
deploy            - Deploy, args:
   -g <group>     - Group ID.
  [-v <version>]  - Deploy version.
list-groups       - List Worker Groups/Fleets.
```
#### Sample Response

```shell
Successfully committed version 7c04de1
```

### `groups`

Deprecated. See [`git`](#git).


### `keys`

Manages encryption keys. You must append the `-g <group>` argument to specify a Worker Group/Fleet. As a fallback, append the argument `-g default`, e.g.: 
`./cribl keys list -g default`

#### Usage

`./cribl keys <sub-command> <options>  <args> -g <group> `

#### Sub-commands and Options

```shell
add               - Add encryption keys, args:
  [-c <keyclass>] - key class to set for the key
  [-k <kms>]      - KMS to use, must be configured, see cribl.yml
  [-e <expires>]  - expiration time, epoch time
  [-i ]           - use an initialization vector
   -g <group>     - Group ID
list              - List encryption keys, args:
   -g <group>     - Group ID
```
#### Sample Response

```shell
Adding key succeeded. Key count=1
```

### `nc`

Listens on a port for traffic, and outputs stats and data. (Netcat-like utility.)

#### Usage

`./cribl nc -p <port> <options>  <args>`

#### Options

```shell
 -p <port>           - Port to listen on.
[-f 6]               - Listen on IPv6 addresses ("::" format). Omit this option – or use "-f 4" – to listen on the default IPv4 addresses ("0.0.0.0").
[-s <statsInterval>] - Stats output interval (ms), use 0 to disable.
[-u]                 - Listen on UDP port instead.
[-o]                 - Output received data to stdout.
[-t <throttle>]      - throttle rate in (unit)/sec, where units can be KB,MB,GB, and TB.
```

#### Sample Response

```shell
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

```shell
> 
```
To execute a JavaScript file, you can enter path/filename at the prompt.

With the `-v` option, prints the version of NodeJS that is running.

With `-e`, evaluates a string. Write to console to see the output, for example: 

```shell
./cribl node -e 'console.log(Date.now())'
1629740667695
```

#### Usage

`./cribl node <options>  <args>`

#### Options

```shell
[-e <eval>] - String to eval
[-v]        - Prints NodeJS version
```

#### Sample Response

```shell
v14.15.1
```

### `pipe`

Feeds stdin to a pipeline. 

#### Usage

`./cribl pipe -p <pipelineName>  <options>  <args>`

Examples:

```shell
cat sample.log |  ./cribl pipe -p <pipelineName> 
cat sample.log |  ./cribl pipe -p <pipelineName> 2>/dev/null
```

#### Options

```shell
 -p <pipeline>    - Pipeline to feed data thru
[-d]              - Include dropped events
[-c <cpuProfile>] - Perform CPU profiling
[-a <pack>]       - Optional Cribl Pack context
```

#### Sample Response

```shell
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

Manages Cribl Stream [Global Variables](global-variables-library).

#### Usage

`./cribl vars <sub-command> <options>  <args>`

#### Sub-commands and Options

```shell
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

```shell
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
