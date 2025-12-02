# Tee



The Tee Function tees events out to a command of choice, via `stdin`. The output is one JSON-formatted event per line. You can send the events to (for example) a local file on the Cribl Stream worker. This can be useful in verifying the data being processed in a Pipeline.

The [Filesystem/NFS](destinations-fs) Destination offers similar capability, but only after the data leaves the Pipeline. Tee, by comparison, can be inserted at any point in the Pipeline.

> In Cribl.Cloud, the Tee Function is only available on customer-managed [hybrid](cloud-enterprise#hybrid) Worker Nodes.
{.box .cloud}

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Command**: Command to execute and receive events (via `stdin`) – one JSON-formatted event per line.

**Args**: Click **Add Arg** to supply arguments to the command.

**Restart on exit**: Restart the process if it exits and/or we fail to write to it. Defaults to `Yes`.

**Environment variables**: Environment variables to set or overwrite. Click **Add Variable** to add key-value pairs.

### Communication Protocol

Data is passed to the command through its `stdin`, using the following protocol: 

* First line: Metadata serialized in JSON, containing the following fields:
    * **format**: Serialization format for event. Defaults to `JSON`.
    * **conf**: Full Function configuration.

* Remaining: Payload.

## Examples


Assume that we are parsing PANOS Traffic logs, and want to see how they look at a particular step in the processing Pipeline We’ll assume that the `Parser` Function is already in place, so we’ll insert the Tee Function at any (arbitrary) later point in the Pipeline.


### Scenario A:

The Tee Function itself requires only that we define the **Command** field. In this particular example, that **Command** will be `tee` itself. 

We’ve also clicked **Add Arg**, to specify a local output file in the resulting **Args** field. (A file path would normally be the first argument to a `tee` command executed from the command line. The Cribl Stream user must have write permission on the specified file path.)

Command: `tee`

Args: `/opt/cribl/foo.log`

In this first scenario, assume that we have the `Parser` configured to parse, but not keep any fields. After changes are deployed and PANOS logs are received, if we tail `foo.log`, we’d see the following:

`
Line 1: {"format":"json","conf":{"restartOnExit":true,"env":{},"command":"tee","args":["/opt/cribl/foo.log"]}
`

`
Line 2: {"_raw":"Oct 09 10:19:15 DMZ-internal.nsa.gov 1,2019/10/09 10:19:15,001234567890002,TRAFFIC,drop,2304,2019/10/09 10:19:15,209.118.103.150,160.177.222.249,0.0.0.0,0.0.0.0,InternalServer,,,not-applicable,vsys1,inside,z1-FW-Transit,ethernet1/2,,All traffic,2019/10/09 10:19:15,0,1,63712,443,0,0,0x0,udp,deny,60,60,0,1,2019/10/09 10:19:15,0,any,0,0123456789,0x0,Netherlands,10.0.0.0-10.255.255.255,0,1,0,policy-deny,0,0,0,0,,DMZ-internal,from-policy,,,0,,0,,N/A,0,0,0,0,1202585d-b4d5-5b4c-aaa2-d80d77ba456e,0","_time":1593185574.663,"host":"127.0.0.1"}
`

In Line 2 above, note that the `_raw` field makes up most of the contents,  with only the `_time` and `host` fields added. 

### Scenario B:

Assume that we use the Tee Function, using the same **Command** and arguments, but we’ve modified the `Parser` Function to retain five fields: `receive_time`, `source_port`, `destination_port` `bytes_received`, and `packets_received`.

This time, if we tail `foo.log`, we’ll see something like the following. If you compare this output to the previous output example, you’ll notice the five fields appended to this event:

`
Line 3: {"_raw":"Oct 09 10:19:15 DMZ-internal.nsa.gov 1,2019/10/09 10:19:15,001234567890002,TRAFFIC,drop,2304,2019/10/09 10:19:15,209.118.103.150,160.177.222.249,0.0.0.0,0.0.0.0,InternalServer,,,not-applicable,vsys1,inside,z1-FW-Transit,ethernet1/2,,All traffic,2019/10/09 10:19:15,0,1,63712,443,0,0,0x0,udp,deny,60,60,0,1,2019/10/09 10:19:15,0,any,0,0123456789,0x0,Netherlands,10.0.0.0-10.255.255.255,0,1,0,policy-deny,0,0,0,0,,DMZ-internal,from-policy,,,0,,0,,N/A,0,0,0,0,1202585d-b4d5-5b4c-aaa2-d80d77ba456e,0","_time":1593185606.965,"host":"127.0.0.1","receive_time":"2019/10/09 10:19:15","source_port":"63712","destination_port":"443","bytes_received":"0","packets_received":"0"}
`
<br/>

> In this Function’s **Command** field, you can specify commands other than `tee` itself. For example: By using `nc` as the command, and specifying `localhost` and a port number (as two separate arguments), you’ll see event data being received via `nc` on the specified port.
>
{.box .info}
