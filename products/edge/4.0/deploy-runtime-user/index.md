# Running Edge as an Uprivileged User


Privileged access might be necessary if Cribl Edge needs to read certain resources (e.g., `/var/log/*`), or to listen on low ports 1–1024. Features like auto-discovery of logs and information in the Processes UI also require permissions to access `/proc`. The regular non-root permissions are not sufficient in these cases.

There are two alternatives to running Cribl Edge as `root`:
- Set Linux capabilities that grant Cribl Edge sufficient rights to perform specific privileged tasks. For details, see [Set Capabilities for Cribl Edge](#cap) below. 

- Take advantage of Linux systems' option to layer an Access Control List (ACL) over the default Linux permissions. By using ACLs, you can assign a more specific set of permissions to a file or directory without (necessarily) changing the base ownership. For details, see [Using ACLs to Allow Cribl Edge to Read Files](usecase-edge-acls).

## Set Capabilities for Cribl Edge {#cap}

Capabilities are permissions that grant privileged processes sufficient rights to accomplish a specific task, based on a kernel privilege. To run Cribl Edge as non-root user, consider setting the following capabilities: 

| Capability| Permissions|
|--|--|
| `CAP_NET_BIND_SERVICE` | Allows Cribl Edge to push Sources that bind to TCP/UDP port numbers below `1024`. |
| `CAP_DAC_READ_SEARCH` | Allows the `cribl` user to access files in  **Explore** > **Files** > [Manual/Browse](explore-edge#file-discovery-modes), and to access the File Monitor Source's [Manual mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. This capability bypasses the default Linux permissions for files and directories.|
| `CAP_SYS_PTRACE`  | Allows the `cribl` user to scan open files for running processes, to discover active logs in **Explore** > **Files** > [Auto](explore-edge#file-discovery-modes), and to access the File Monitor Source's [Auto-mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. | 

For details about setting these capabilities, see [Persisting Overrides](deploy-boot-start#persisting-overrides).


## OS-Specific Options

To read file resources on a Linux system that are typically restricted to the root user, you can add the `CAP_DAC_READ_SEARCH` capability. For example: 

On some OS versions (such as CentOS), you must add an `-i` switch to the `setcap` command. For example: 
`# setcap -i cap_dac_read_search=+ep $CRIBL_HOME/bin/cribl`

Upgrading Edge will remove the `CAP_DAC_READ_SEARCH` capability from the `cribl` executable, so you'll need to re‑run the appropriate `setcap` command after each upgrade.

## Fallbacks from Privileged Access

If installing and running Edge with `root`-level privileges is forbidden or impractical in your environment, certain Sources, like Exec and System Metrics, can run on a user with lower permissions. You can also run the File Monitor Source in `Manual` mode, and collect from any files that this user can read.
