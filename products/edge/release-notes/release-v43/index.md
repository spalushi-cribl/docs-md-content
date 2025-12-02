# Cribl Edge 4.3


What's the best thing about September? Cribl Edge 4.3, of course! All-new
features served piping hot, best enjoyed with a pumpkin-flavored treat and your
favorite fall activity.

## New Features 

This release provides the following improvements:

### Windows Upgrade via UI

The Cribl Edge 4.3 release lays the foundation for future upgrades of Windows Nodes from the Leader's UI. Follow specific steps for a one-time upgrade of Windows Nodes to take advantage of managing them and receiving Leader updates. For details, see [Upgrading the (Windows) Edge Nodes](upgrading#win-upgrade).

### New Exec Source Fields

The [Exec Source](/edge/sources-exec) is the proud owner of some neat updates:

- A new event field lists the hostname of the event. It's aptly called `host`. 
- The `source` field now accurately identifies the source of the event (the
  `exec` command string).
- Lastly, to facilitate traceability, the **Logs** tab now contains a new log
  message including the command executed, its elapsed time, and its exit
  code.

### New Kubernetes Logs Internal Fields

The Kubernetes Logs Source now displays more internal fields in log events. They
are the full objects for the namespace, node, and pod where the log originated:

- `__kube_namespace`
- `__kube_node`
- `__kube_pod`

### Compressed and Archived File Preview on the Inspect File Tab

When inspected, compressed files (e.g. `foo.bar.gz`) include a File preview that
shows the beginning of the file contents.

Archived files (e.g. `.zip`, `.tgz`, `.tar.gz`) include a File listing that
shows the files within it.

### Custom Hash Length of File Headers

In the File Monitor, you can now customize the hash length of file headers to
ensure files can be uniquely identified.

## Corrections

This release includes the following fixes:

CRIBL-19818 When set to XML format, the Windows Event Logs Source now accepts
log names that contain spaces. 

CRIBL-14672 Restored Leader UI's ability to specify TLS settings for managed
Edge Nodes. (Corrected erroneous validity check for certificate paths.)

CRIBL-11229 On the **List View** tab, we've relabeled three column headers for
clarity — **Edge Version**, **Last Heartbeat**, and **Started**.

CRIBL-18337 Removed broken Diagnostics option from Fleet Settings.
