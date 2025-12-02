# Enabling Start on Boot {#boot-start}


Cribl Edge ships with a CLI utility that can update your system's configuration to start Edge at system boot time. The basic format to invoke this utility is: 

```shell
[sudo] $CRIBL_HOME/bin/cribl boot-start [enable|disable] [options] [args]
```

> You must run this command as root, or with `sudo`. For options and arguments, see the [CLI Reference](/edge/cli-reference#boot-start).
> 
> The script will create a user named `cribl` to install, own, and run Cribl Edge/Stream.
>
{.box .info}

Most, if not all, popular Linux distributions use `systemd` now to start processes at boot, while older or more obscure distributions may still use `initd`. Verify with your Linux distribution vendor if you aren't sure which method your systems use in order to know which procedure listed below to follow.

### Using systemd {#systemd-boot}

To **enable** Cribl Edge to start at boot time with **systemd**, you must run the `boot‑start` [command](#boot-start). Make sure you first create any user you want to specify to run Edge. E.g., to run Edge on boot as existing user `cribl`, you'd use:

`sudo $CRIBL_HOME/bin/cribl boot-start enable -m systemd -u cribl ` 

This will install a unit file (as shown below) named `cribl-edge.service`, and will start Cribl Edge at boot time as user `cribl`. A `‑configDir` option can be used to specify where to install the unit file. If not specified, this location defaults to `/etc/systemd/system/`.

If necessary, change ownership for the Cribl Edge installation:

`[sudo] chown -R cribl $CRIBL_HOME` 

Next, use the `enable` command to ensure that the service starts on system boot:

`[sudo] systemctl enable cribl-edge`

To **disable** starting at boot time, run the following command: 

`sudo $CRIBL_HOME/bin/cribl boot-start disable` 

Other available `systemctl` commands are:

`systemctl [start|stop|restart|status] cribl-edge`

Note the file's default `65536` hard limit on maximum open file descriptors (known as a `ulimit`). The minimum recommended value is `65536`. Linux tracks this per user account. You can view the current soft `ulimit` for max open file descriptors with `$ ulimit -n` while logged in as the same user running the `cribl` binary.

``` {title="Installed systemd File"}
[Unit]
Description=Systemd service file for Cribl Edge.
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

#### Persisting Overrides {#persisting-overrides}

By default, disabling and re-enabling boot start will regenerate the `cribl-edge.service` file. To persist any overrides – such as proxy or privileged port usage – use this command:

`systemctl edit cribl-edge`

This opens a text editor that prompts you to enter overrides, then saves them to a persistent file at:

`/etc/systemd/system/cribl-edge.service.d/override.conf`

##### Using a Literal `%` Character

To use a literal `%` character, make sure to escape it by using a double `%%`.

For example, suppose that you want to persist the environment variable [`CRIBL_DIST_WORKER_PROXY`](environment-variables#cribl_dist_worker_proxy) in a systemd unit file, and the value includes a password that contains the special character `@`: `special@password`. You must encode the `@` as `%40`. However, because `%` is a special character that is used for [specifiers](https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html#Specifiers) in systemd unit files, you must also escape the `%` itself by doubling it.

In this example, `%%40` represents the literal `@` character in the password:

```shell
Environment=CRIBL_DIST_WORKER_PROXY=socks5://proxyuser:special%%40password@mydomain.com:1080
```

##### Avoid Running Cribl Edge as Root

> **Do not run Cribl Edge as root!**
>
{.box .warning}

To listen on low ports 1-1024, Cribl Edge needs privileged access. You can enable this on systemd by adding this configuration key to your `override.conf` file:

```shell
[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE 
```

If you want to add extra capabilities, such as reading certain resources (e.g., `/var/log/*`), add `CAP_DAC_READ_SEARCH` in a space-separated format as follows:

```shell
[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE CAP_DAC_READ_SEARCH
```

To unlock advanced file monitoring in Cribl Edge, such as scanning open files for running processes, discovering active logs automatically in Explore, and using the File Monitor Source's auto-mode feature, add `CAP_SYS_PTRACE` :

```shell
[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE CAP_DAC_READ_SEARCH CAP_SYS_PTRACE
```

Alternatively, you can use [ACLs to Allow Cribl Edge to Read Files](usecase-edge-acls).

For details, see [Running Edge as an Unprivileged User](deploy-runtime-user).


### Using initd

To **enable** Cribl Edge to start at boot time with **initd**, you must run the `boot‑start` command. If the user that you want to run Cribl Edge does not exist, create that user prior to executing. E.g., running Edge as user `cribl` on boot:   

`sudo $CRIBL_HOME/bin/cribl boot-start enable -m initd -u cribl ` 

This will install an `init.d` script in `/etc/init.d/cribl.init.d`, and will start Cribl Edge at boot time as user `cribl`. A `‑configDir` option can be used to specify where to install the script. If not specified, this location defaults to `/etc/init.d`.

If necessary, change ownership for the Cribl Edge installation:

`[sudo] chown -R cribl $CRIBL_HOME` 

To **disable** starting at boot time, run the following command: 

`sudo $CRIBL_HOME/bin/cribl boot-start disable` 

To control Cribl Edge, you can use the following initd commands:

`service cribl-edge [start|stop|restart|status]`

#### Persisting Overrides on initd

Notes on preserving required permissions across restarts and upgrades:

> To read file resources on a Linux system that are typically restricted to the root user, you can add the `CAP_DAC_READ_SEARCH` capability. For example: 
> 
> On some OS versions (such as CentOS), you must add an `-i` switch to the `setcap` command. For example: 
> `# setcap -i cap_dac_read_search=+ep $CRIBL_HOME/bin/cribl`
> 
> **Important**: Upgrading Edge will remove the `CAP_DAC_READ_SEARCH` capability from the `cribl` executable, so you'll need to re‑run the appropriate `setcap` command after each upgrade.
>
{.box .warning}

