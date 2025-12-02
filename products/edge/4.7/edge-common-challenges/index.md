# Common Challenges on the Edge


Here are some common issues that users have encountered when using Cribl Edge, along with guidance to resolve them.

## Disable SNI Routing for Firewalls/Web Proxies {#sni}

There is a setting in `cribl.yml` to help you manage Server Name Indication (SNI) routing, which can be helpful if you're encountering difficulties with proxies or firewalls. 

If you have a proxy or firewall deployed between your Edge Nodes and the Leader, this new setting allows you to disable SNI routing for specific Fleets.

To disable SNI routing for a Fleet: Navigate to **Group Settings** > **General Settings** > **SNI** > **Disable SNI Routing** option. **Commit** and **Deploy** and then proceed with the upgrade. You must enable this setting for every Fleet that is behind a web proxy or firewall.

## Web UI Errors

While we hope you have an error-free experience, we want to prepare you in case
you run into issues. Here are some errors you might encounter in Edge, and
how to resolve them:

### Error: "No workers registered"

**Cause:** Occurs during a live data capture, when there are no Edge Nodes
available to fulfill the capture request. This error can be triggered if you
attempt to capture events too quickly following a commit & deploy.

**Recommendation:** There are two things you can try:

- If you recently committed and deployed changes, wait 30 seconds and retry the
  live capture.

- Navigate to **Manage > Edge Nodes** and confirm there are Nodes currently
  registered with the Leader. Note that the error appears on a per-Fleet basis,
  which means that even if you have registered Nodes, they may not be in the
  Fleet you're capturing live data from.

## Leader and Edge Node (In)Compatibility

Leaders on v.4.2.x are compatible with Edge Nodes on v.4.1.2 and later. Due to a security update, Edge Nodes running on v.4.0.4 cannot receive configurations from v.4.2.x Leaders. For details, see [Leader and Edge Nodes Compatibility](upgrading#compatible).

## Duplicate Edge Node GUID

When you install and first run the software, a GUID is generated and stored in a `.dat` file located in `CRIBL_HOME/local/cribl/auth/`. For example: 

**Linux**

```shell 
# cat /opt/cribl/local/cribl/auth/676f6174733432.dat

{"it":1570724418,"phf":0,"guid":"48f7b21a-0c03-45e0-a699-01e0b7a1e061"}
```

**Windows**

- PowerShell

  ```powershell
  PS C:\ProgramData\Cribl\local\cribl\auth> type 676f6174733432.dat

  {"it":12345678901,"phf":0,"guid":"ff983143-1f93-4b61-a442-e5db8ccb1f50","lics":["license-example"]}
  ```

- CMD

  ```shell
  type C:\ProgramData\Cribl\local\cribl\auth> type 676f6174733432.dat

  {"it":12345678901,"phf":0,"guid":"ff983143-1f93-4b61-a442-e5db8ccb1f50","lics":["license-example"]}
  ```

When deploying Cribl Edge as part of a host image or VM, be sure to remove this file so that you don't end up with duplicate GUIDs. The file will regenerate on the next run.

## Running Edge (Linux) as Non-root User

Privileged access might be necessary if Cribl Edge needs to read certain resources (e.g., `/var/log/*`), or to listen on low ports `1–1024`. Features like auto-discovery of logs and information in the Processes UI also require permissions to access `/proc`. The regular non-root permissions are not sufficient in these cases. For guidelines, see [Running Edge as an Unprivileged User](deploy-runtime-user).

## Debugging Cribl Edge on Windows 

Common issues users encounter when deploying Cribl Edge on Windows include: 

### Using a Pre-4.3.0 MSI Installer

MSIs earlier than v.4.3.0 install the app so it belongs to the user running the installer. If you later try to upgrade it as another user, things go awry — resulting in a rather confusing mess with a seemingly random mix of the old and new package contents on disk.

Moving forward, we recommend adding `ALLUSERS=1` to the `msiexec` command when installing or upgrading pre-4.3.0 versions for testing. Example command: 

`msiexec /i cribl-<version>-<build>.msi /qn MODE=mode-managed-edge HOSTNAME=<yourhostname> PORT=4200 AUTH=myAuthToken FLEET=myfleet  USERNAME=LocalSystem ALLUSERS=1`

### Debugging MSI Installations

To debug MSI installation or upgrade issues, you can run `msiexec.exe` to set logging options. See [Microsoft's Logging Options](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec#logging-options) topic.

For troubleshooting an MSI installation that does not pull configuration settings from the Leader node, you need to know the log path on Windows: The full path depends on where you chose to install. 

`$INSTALL_PATH\cribl\log` and `local\edge\` are the log location and conf location, respectively.

### CPU/Memory Issues on Windows

If you notice high CPU/Memory usage on your Cribl Edge Windows:

- The Users and Groups collector in the System State source can trigger high CPU utilization in the Windows lsass.exe process where there are many user accounts on the host; this is not uncommon on Domain Controllers. Consider disabling that collector if you see that process consuming excessive CPU.

- If you are interested in collecting logs only, disable the Windows Metrics source.

- During the Windows Event Log capture, run the collection for 10 seconds and tweak the settings if necessary so that collection doesn't always have to catch up. Consider changing the Windows Events Log Batch Size and Frequency.

   Example command to capture a certain number of events: 

   `measure-command {Get-WinEvent -Oldest -MaxEvents 500 @{LogName='Security'} | ForEach-Object -Process {ConvertTo-Json -Compress $_}}`

- For Domain Controllers with heavy log volumes, consider using Windows Event Forwarder and send them to Stream instead of using Windows Event Log Source in Cribl Edge.

### Installing or Upgrading Nodes with Endpoint Security

If you are having trouble installing or upgrading Cribl Edge, check for
antivirus or other endpoint security tools that may be blocking the `cribl`
process (like Windows Defender). To install Cribl Edge, you may need to add
temporary or permanent exceptions to these tools.

To tell Windows Defender to exclude the Cribl Edge process from its security
scan, you can add this command to the appropriate Windows Edge Nodes:

```
Add-MpPreference -ExclusionProcess "C:\Program Files\Cribl\bin\nssm.exe"
```

### Local Files When Uninstalling Windows

This uninstall process does not automatically delete the local, log, pid, and state directories, because those folders include contents generated after the original installation completed. To completely remove the application, you must explicitly delete these folders from the filesystem.

## Edge via Kubernetes

Common mistakes users make when deploying via Kubernetes include: 

### Failing Health Checks 

By default, the Cribl Edge API listens on port 9420, instead of on Cribl Stream's default 9000 port. This requires special care when you configure your Edge Fleet in the Cribl UI.

The API server is configured to listen on the loopback interface (127.0.0.1). Change the listening interface address to 0.0.0.0 to bind to all IPs in the container. If you do not make this change, your health checks will start failing after the Edge agent downloads its initial config bundle.

Another side effect of not changing the Listening Interface address, is the Edge Node will not appear on the Leader UI.

### Not Setting the Environment Variables for Invalid Certifications

If your Kubernetes deployment doesn't use valid certificates, set the `CRIBL_K8S_TLS_REJECT_UNAUTHORIZED` environment variable to 0 to disable that expectation. When you disable this environment variable, all Kubernetes features (including Metadata, Metrics, Logs, and AppScope metadata) will tolerate invalid (expired, self-signed, etc.) TLS certificates when connecting to the Kubernetes APIs. If this environment variable is not defined or set to 1 and the certificate validation fails, all connection attempts to the Kubernetes API will be rejected.

### Not Authorizing the Cribl Edge Pod Service Account

Cribl Edge's Kubernetes Logs and Kubernetes Metrics Sources use the Kubernetes API to read logs and metrics from the cluster. If you have role-based access control (RBAC) implemented, you will need to authorize the Cribl Edge Pod Service Account to read the relevant data. For details, see [RBAC](deploy-running-kubernetes#rbac).

### Not Deploying Cribl Edge as a Daemonset

The Kubernetes Sources (Events, Logs, Metrics)  can generate redundant events when it is running inside the cluster and can't determine which DaemonSet it's in or which Node it's on. This happens when you run Cribl Edge inside the cluster in a way that it can't detect the local Pod/Node. In these cases, Edge will emit an error when it initializes this Source.

For the Kubernetes Events Source to work as designed, we recommend deploying Cribl Edge as a Daemonset. For details, see [Deploying via Kubernetes](deploy-running-kubernetes).


## Which Destination to Use for Cribl Stream

Always use Cribl TCP or Cribl HTTP as the Destinations to send data to Cribl Stream.  Otherwise, you might see duplication of certain data (such as host). Also, data sent to Cribl Stream via TCP/HTTP is not counted against the license.


## Unsupported Files on the Journal Files Source

This Source does not support the new `COMPACT` binary file format. The logs will contain an error message when the parser encounters this type of file. (A fix is planned for v.4.3.1.). Sample  error message when enabling the Journal Files Source: 

```JSON
{ [- ]
channel: input: in_journal_local cid: WO err: { [-]
message: Cannot read properties of undefined (reading 'data')
stack: TypeError: Cannot read properties of undefined (reading 'data')
at w. readEntry (/opt/cribl-edge/bin/crib1.js:14:18884713)
at w. readEvents (/opt/cribl-edge/bin/crib1.js:14:18885930)
at w._transform (/opt/crib1-edge/bin/crib1.js:14:18886631)
at w. Transform. _write (node: internal/streams/transform:205:23) at writeOrBuffer (node: internal/streams/writable:391:12) at _write (node: internal/streams/writable:332:10) at w.Writable.write (node:internal/streams/writable:336:10) at a.ondata (node: internal/streams/readable: 754:22) at a.emit (node: events: 513:28) at a.emit (node: domain: 489:12)
}
level: warn
message: Error while stopping journal collector time: 2023-08-15T21:04:51.929Z
Show as raw text
host = ip-10-99-99-38.us-west-2.compute. internal
source = cribl: sourcetype = cribledge
```

Also, this Source does not currently support all of the compression options that the journald service supports. The parser this Source uses to read the files can handle `LZ4` and `ZSTD` compression. When the parser encounters compressed fields it can't unpack, an error message emits in the logs. Here is an example of the error message:

`Error reading journald event, dropping offset=31678152 evt="Mar 02 21:29:54 goats-xps15 multipassd: undefined"`

## File Monitor Source 
Common mistakes users make when exploring the File Monitor Source include the following:

### Using an Editor

Do not test the Source by editing and saving files in a directory using a text editor (vi or Notepad). Under the hood, these editors operate on a temporary file that they move into place when saved. As a result, the File Monitor Source sees  files get replaced and moved rather than appended to. This confuses the Source’s logic that assumes logs are appended to the existing file.

Run the command below to append logs to files:
`echo "{ \"time\":\"$(date -Is -u)\", \"message\":\"...\" }" >> eg.log`

You end up with lines like this in the logs:
`
{ "time":"2023-09-19T20:08:42+00:00", "message":"..." }`

### Misunderstanding the Filename Allowlist 

The Filename allowlist in the Source applies the wildcard patterns to the entire path of the discovered file, not just the file name. 

The search path itself is not excluded from the match. For example, if you want to collect the file /var/log/messages when you've set your search path to `/var/log,` your filename allowlist needs to be either `/var/log/messages`, `/var/*/messages`, or `*/log/messages`.

For details, see Using [Allowlists Effectively](sources-file-monitor#using-allowlist).

### Not configuring the Hash Length to Avoid Common Headers
It’s common for a batch of CSV files or IIS logs to have identical first lines listing columns that appear in subsequent lines. For files with identical first lines, configure the Hash length  to be longer than the first line to ensure that the header includes something unique. For details, see [Configuring Hash Lengths](sources-file-monitor#hash).

### Allowing Race Conditions
The hashing logic isn’t instantaneous, so it’s possible for the common header problem described above to be confusing. Quickly copying the same example file to separate eg1.log and eg2.log files in the Source’s path will usually result in both files being collected even though they share the same hash. However, copying it to eg3.log shortly (not immediately) after will result in that third file being ignored because its head-hash matches an existing entry in the state-store.

### Testing Multiple Copies of the Same File 
The head-hashing logic used by the Source to determine what it’s seen before and how far it’s collected already prevents you from successfully testing multiple copies of the same. You can’t copy the same file into the Source’s path over and over and expect them to be collected repeatedly.

### Pointing Multiple File Monitor Sources to the Same File 
Multiple File Monitor Sources set to collect the same file will result in duplicate events.  Sources run independently and don’t coordinate with each other. 

### Testing with Small Files
The Source doesn’t track state using filenames. Instead, it uses hashes of the head and tail of the file to identify it later so it can resume where it left off. There is some exception logic for dealing with files smaller than the hash sizes that cannot be made perfect. If you try to test with small files (smaller than 256 bytes), you will be working with the fallback logic and not the logic used for normal logs.

### Panicking over Variable File Sizes 

The File Monitor Source can handle a variety of file sizes without "starving" the smaller ones. In Cribl Edge, multiple workflows can be multiplexed using the event loop, even though it runs on a single core.

### Not Testing with Event Breakers

To make sure that incoming files are splitting into individual events as
expected, test sample file content with [Event Breakers](event-breakers). When
you configure the Event Breaker Ruleset, configure the timestamp correctly and
verify that it works. The Event Breaker will default to “Now” if it can’t find a
timestamp in the raw log, and it saves the cursor state at that point. 

If the Event Breaker encounters another log later in the file with a timestamp
before “Now”, it will ignore it. For details, see [Timestamp
Settings](event-breakers#timestamp-settings).
