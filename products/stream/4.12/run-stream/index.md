# Run Cribl Stream


Run Cribl Stream: Start, Stop, and Reload the Service

---

## Cribl Stream Commands {#run-stream}

> To run Cribl Stream in FIPS mode, **do not** use the commands below right away; instead, first consult the [FIPS Mode](fips-mode#running-fips) topic.
{.box .warning}

Go to the `$CRIBL_HOME/bin` directory, where the package was extracted (for example: `/opt/cribl/bin`). Here, you can use `./cribl` to:

- **Start**: `./cribl start`
- **Stop**: `./cribl stop`
- **Reload**: `./cribl reload`
- **Restart**: `./cribl restart`
- **Get status**: `./cribl status`
- **Switch a [distributed deployment](/stream/deploy-distributed) to single-instance mode**: 
`./cribl mode-single` (uses the default address:port `0.0.0.0:9000`)

> Executing the `restart` or `stop` command cancels any currently running [collection jobs](collectors). For other available commands, see [CLI Reference](cli-reference).
>
{.box .info} 

Next, go to `http://<hostname>:9000` and log in with default credentials (`admin:admin`). You can now start configuring Cribl Stream with [Sources](sources) and [Destinations](destinations), or start creating [Routes](routes) and [Pipelines](pipelines).

> In the case of an API port conflict, the process will retry binding for 10 minutes before exiting.
>
{.box .info}

##  Shutdown and Restart Sequence  {#shutdown}

Different types of configuration changes produce different behaviors in the Worker, as illustrated in the table below.

|Configuration change example|How Cribl Streamresponds|
|---|---|
|Change from one Route to another|Reloads the contents of memory|
|Add a new Source or Destination|Restarts the Worker Process|
|Make a change in system settings that requires restarting the API Process|Restarts the API Process and Worker processes |

The Leader initiates the configuration change by sending requests to each Worker to trigger deployment. If the change is in the third category from the table above, it will restart the Worker.

Beginning in Cribl Stream v.4.7, when a Worker receives an explicit shutdown command, it follows this sequence:

1. The API Process begins waiting for the Worker Processes to exit.
2. Each Worker Process shuts down its input Sources.
3. Each time one of its input streams ends, a given Worker Process receives a signal event from the Cribl Sream event processor directing it to flush out any stateful Pipeline Functions (such as [Aggregations](aggregations-function), [Sampling](sampling-function), [Dynamic Sampling](dynamic-sampling-function), and [Suppress](suppress-function)).
4. The Worker Process waits to allow data to "drain" – that is, to finish flowing through the streams processing engine. This waiting period, called the **Drain timeout**, defaults to 10 seconds; see [Shutdown settings](group-settings#shutdown). When it ends, any data not flushed out – for example, because of an error on downstream receivers – will be lost. If this happens, the `shutdown.lost_events` metric reports the number of events lost.
5. When all the Worker Processes have finished flushing and exited, the API Process reports metrics to the Leader.
6. The API Process exits, and the Worker has now shut down.

When the Leader sends a signal that causes Workers to restart, they all restart at the same time. This can result in a time interval during which no Workers are available to accept connections from upstream senders. If you encounter this problem, you can try configuring a shorter drain timeout, enabling Workers to finish restarting more quickly. This workaround, however, increases the risk of losing outbound data by allowing Worker Processes less time to flush it.

#### Shutdown/Restart with PQ

Enabling [Persistent Queues](persistent-queues) on Destinations that support it brings both an advantage and a risk: 

* The advantage is that enabling PQ generally helps ensure data delivery to your downstream systems.
* The risk is that you might see duplicate events when a Worker Process restarts.

This can happen because PQ only marks events as safe to discard once they have been handed off to the host OS to send out. So, if the Worker Process exits **before** all events have flushed, the final handful of events will not have been marked as committed and removed. Upon restart, Cribl Stream will still see them, and will resend them.

##  Enabling Start on Boot  {#boot-start}

Cribl Stream ships with a CLI utility that can update your system's configuration to start Cribl Stream at system boot time. The basic format to invoke this utility is:

```shell
[sudo] $CRIBL_HOME/bin/cribl boot-start [enable|disable] [options] [args]
```

> You will need to run this command as root, or with `sudo`. For options and arguments, see the [CLI Reference](cli-boot-start).
>
{.box .info}

Most Linux distributions now use `systemd` to start processes at boot, while older distributions might still use `initd`.
If you are not sure which service should be configured at startup, check with your Linux administrator. Then follow the corresponding procedure below.

### Using systemd {#systemd-boot}

To **enable** Cribl Stream to start at boot time with **systemd**, you need to run the `boot‑start` [command](#boot-start). Make sure you first create any user you want to specify to run Cribl Stream. For example, to run Cribl Stream on boot as existing user `cribl`, you'd use:

`sudo $CRIBL_HOME/bin/cribl boot-start enable -m systemd -u cribl ` 

This will install a unit file (as shown below) named `cribl.service`, and will start Cribl Stream at boot time as user `cribl`. A `‑configDir` option can be used to specify where to install the unit file. If not specified, this location defaults to `/etc/systemd/system/`.

If necessary, change ownership for the Cribl Stream installation:

`[sudo] chown -R cribl $CRIBL_HOME` 

Next, use the `enable` command to ensure that the service starts on system boot:

`[sudo] systemctl enable cribl`

To **disable** starting at boot time, run the following command: 

`sudo $CRIBL_HOME/bin/cribl boot-start disable` 

Other available `systemctl` commands are:

`systemctl [start|stop|restart|status] cribl`

Note the file's default `65536` hard limit on maximum open file descriptors (known as a `ulimit`). The minimum recommended value is `65536`. Linux tracks this per user account. You can view the current soft `ulimit` for max open file descriptors with `$ ulimit -n` while logged in as the same user running the `cribl` binary.

``` {title="Installed systemd File"}
[Unit]
Description=Systemd service file for Cribl Stream.
After=network.target

[Service]
Type=forking
User=cribl
Restart=always
RestartSec=5
LimitNOFILE=65536
PIDFile=/install/path/to/cribl/pid/cribl.pid
ExecStart=/install/path/to/cribl/bin/cribl start
ExecStop=/install/path/to/cribl/bin/cribl stop
ExecReload=/install/path/to/cribl/bin/cribl reload
TimeoutSec=60

[Install]
WantedBy=multi-user.target
```

#### Persisting Overrides on systemd {#persisting-overrides}

By default, disabling and re-enabling boot start will regenerate the `cribl.service` file. To persist any overrides – such as proxy or privileged port usage – use this command:

`systemctl edit cribl`

This opens a text editor that prompts you to enter overrides, then saves them to a persistent file at:

`/etc/systemd/system/cribl.service.d/override.conf`

##### Using a Literal `%` Character

To use a literal `%` character, make sure to escape it by using a double `%%`.

For example, suppose that you want to persist the environment variable [`CRIBL_DIST_WORKER_PROXY`](environment-variables#cribl_dist_worker_proxy) in a systemd unit file, and the value includes a password that contains the special character `@`: `special@password`. You must encode the `@` as `%40`. However, because `%` is a special character that is used for [specifiers](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html#Specifiers) in systemd unit files, you must also escape the `%` itself by doubling it.

In this example, `%%40` represents the literal `@` character in the password:

```shell
Environment=CRIBL_DIST_WORKER_PROXY=socks5://proxyuser:special%%40password@mydomain.com:1080
```

##### Using Ports 1-1024 With Systemd {#low-ports-systemd}

To listen on low ports 1–1024, Cribl Stream needs privileged access.

> Do not run Cribl Stream as root!
{.box .warning}

You can enable privileged access on systemd without running as root in the following way:

1. Stop the Cribl Stream server: `systemctl stop cribl`.
1. Add the following configuration key to the `override.conf` file:

   ```shell
   [Service]
   AmbientCapabilities=CAP_NET_BIND_SERVICE 
   ```

   If you want to add extra capabilities, such as reading certain resources (such as, `/var/log/*`), add `CAP_DAC_READ_SEARCH` and `CAP_SYS_PTRACE` in a space-separated format as follows:

   ```shell
   [Service]
   AmbientCapabilities=CAP_NET_BIND_SERVICE CAP_DAC_READ_SEARCH CAP_SYS_PTRACE
   ```

1. Either restart the host OS, or run: `systemctl daemon-reload`.
1. Start the Cribl Stream server again: `systemctl start cribl`.

To check whether a port is ready to use, you can run:
`netstat -an | grep <port-number>`

### Using initd

To **enable** Cribl Stream to start at boot time with **initd**, you need to run the `boot‑start` command. If the user that you want to run Cribl Streams does not exist, create it prior to executing. For example, running Cribl Stream as user `cribl` on boot:   

`sudo $CRIBL_HOME/bin/cribl boot-start enable -m initd -u cribl ` 

This will install an `init.d` script in `/etc/init.d/cribl.init.d`, and will start Cribl Stream at boot time as user `cribl`. A `‑configDir` option can be used to specify where to install the script. If not specified, this location defaults to `/etc/init.d`.

If necessary, change ownership for the Cribl Stream installation:

`[sudo] chown -R cribl $CRIBL_HOME` 

To **disable** starting at boot time, run the following command: 

`sudo $CRIBL_HOME/bin/cribl boot-start disable` 

To control Cribl Stream, you can use the following initd commands:

`service cribl [start|stop|restart|status]`

#### Persisting Overrides on initd {#persisting-overrides-initd}

Notes on preserving required permissions across restarts and upgrades:

##### Using Ports 1-1024 With Initd {#low-ports-initd}

To listen on low ports 1–1024, Cribl Stream needs privileged access.

> Do not run Cribl Stream as root!
{.box .warning}

You can enable privileged access on a Linux system with POSIX capabilities without running as root by adding the `CAP_NET_BIND_SERVICE` capability.
For example:

`# setcap cap_net_bind_service=+ep $CRIBL_HOME/bin/cribl`

On some OS versions (such as CentOS), you must add an `-i` switch to the `setcap` command.
For example:

`# setcap -i cap_net_bind_service=+ep $CRIBL_HOME/bin/cribl`

> Upgrading Cribl Stream will remove the `CAP_NET_BIND_SERVICE` capability from the `cribl` executable, so you'll need to re‑run the appropriate `setcap` command again after each upgrade.
{.box .warning}

Upon starting the Cribl Stream server, a `bind EACCES 0.0.0.0:<port>` error in the API or Worker logs (depending on the service) might indicate that `setcap` did not successfully execute.

## System Proxy Configuration {#proxy}

For details on configuring Cribl Stream to send and receive data through proxy servers, see our [System Proxy Configuration](proxy-config) topic.

## Scaling Up

A single-instance installation can be configured to scale up and utilize as many resources on the host as required. See [Sizing and Scaling](scaling) for details.

##  Anti-Virus Exceptions  {#av}

If you are running anti-virus software on a Cribl Stream instance's host OS, here are general guidelines for minimizing accidental blockage of Cribl Stream's normal operation. 

Your overall goals are to prevent the anti-virus software from locking any files while Cribl Stream needs to write to them, and from triggering any changes that Cribl Stream would detect as needing to be committed.

First, if [Persistent Queues](persistent-queues) are enabled on any Destinations, exclude any directories that these Destinations write to. This is especially relevant if you're writing queues to any custom locations outside of `$CRIBL_HOME`.

Next, for any non-streaming Destinations that you've configured, exclude their staging paths.

Next, exclude these subdirectories of `$CRIBL_HOME`:

- `state/`
- `log/`
- `.git/` (usually only exists on Leader Nodes)
- `groups/` (on Leader Nodes)
- `local/` (on Workers or Leader)

Finally, avoid scanning any processes. Except for the queueing/staging directories already listed above, Cribl Stream runs everything in memory, so scanning process memory will slow down Cribl Stream's processing and reduce throughput.

> ###### Collecting Host Metadata
> 
> Cribl Stream can add a `__metadata` property to every event produced from every enabled Source.
> See [Configure Host Metadata](host-metadata) for more information.
{.box .success}
