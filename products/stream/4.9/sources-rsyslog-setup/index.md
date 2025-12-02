# Configuring an rsyslog Source



> This page is not for public release, at least in its current form.
>
{.box .warning}

This page outlines how to set up and test an Cribl Stream Source to ingest data from a generic Linux environment's rsyslog protocol.

1. On a Worker Node, create a syslog Source ( **Data** > **Sources** (Stream) or **More** > **Sources** (Edge)) with the ID `generic`. Set it to listen on UDP port `514` (and optionally, on TCP, too).

1. Add an identifier for this syslog input in the `Input ID` field.

1. Commit, then deploy, this configuration change. 
  Now we're listening for the syslog protocol, on its `514` UDP incoming port. (The port would differ for a specific syslog source. For example, an Cisco ESXi firewall would send to TCP port `1517`, its default destination.)

1. On the Worker's syslog Source, click **Live**. On the **Live Data** tab, set the **Capture Time** to `100` seconds, then click **Capture** to test whether we're receiving test events.

1. Spoof individual events, by pasting `echo` statements like this into the Worker instance's SSH terminal tab: <br/>
  `echo “Test” | logger --server 127.0.0.1 --port 514 --id=1234 --rfc5424`

> The `netcat` alternative to `logger` is TCP-only, not UDP.
>
{.box .info}

1. Set up an rsyslog client on a Worker instance, as outlined in [this tutorial](https://www.tecmint.com/install-rsyslog-centralized-logging-in-centos-ubuntu/). (Skip over the server setup steps.)

1. Edit the configuration file, using: `sudo vi syslog.conf`

1. Add the corrected line from [the tutorial](https://www.tecmint.com/install-rsyslog-centralized-logging-in-centos-ubuntu/) to the end of the file.
