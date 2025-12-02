# Running Cribl Stream as a Non-Root User


We strongly recommended to never run Cribl Stream as the root user. Privileged access might be necessary, however, if Cribl Stream needs to read certain resources (such as `/var/log/*`), or to listen on low ports 0–1023.  Here are two methods to grant this access while maintaining a non-root user:

-  **Using Systemd capabilities**: Set Linux capabilities that grant Cribl Stream sufficient rights to perform specific privileged tasks. For details, see [Set Capabilities for Cribl Stream](#cap) below. 

- **Using setcap (Linux with POSIX capabilities)**: This method directly modifies the executable file to grant it specific privileges. For details, see [Persisting Overrides on initd](deploy-single-instance#persisting-overrides-initd).

## Set Capabilities for Cribl Stream {#cap}

Capabilities are permissions that grant privileged processes sufficient rights to accomplish a specific task, based on a kernel privilege. To run Cribl Stream as non-root user, consider setting the following capabilities: 

| Capability| Permissions|
|--|--|
| `CAP_NET_BIND_SERVICE` | Allows Cribl Stream to push Sources that bind to TCP/UDP port numbers below `1024`. |
| `CAP_DAC_READ_SEARCH` | Allows the `cribl` user to access the File Monitor Source's [Manual mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. This capability bypasses the default Linux permissions for files and directories.|
| `CAP_SYS_PTRACE`  | Allows the `cribl` user to access the File Monitor Source's [Auto-mode](sources-file-monitor#discovering-and-filtering-files-to-monitor) feature. | 

For details about setting these capabilities, see [Persisting Overrides](deploy-single-instance#persisting-overrides).

## OS-Specific Options 

On some OS versions (such as CentOS), you must add an `-i` switch to the `setcap` command. For example: 
`# setcap -i cap_net_bind_service=+ep $CRIBL_HOME/bin/cribl`

Upgrading Cribl Stream will remove the `CAP_NET_BIND_SERVICE` capability from the `cribl` executable, so you'll need to re‑run the appropriate `setcap` command after each upgrade.

## Changing Ownership of Cribl Stream After Startup

To change the ownership of Cribl Stream from root to a non-root user after the application has already been started, you must modify the ownership of both the primary installation directory (`$CRIBL_HOME`) and any Cribl Stream-related directories in the `/tmp` directory. If you changed the default `/tmp` directory location using the **System** > **General Settings** > **Sockets** > **Directory** setting, then adjust the location accordingly. Use the `chown` command to change the owner of these directories to the desired non-root user. To complete the process, restart the Cribl Stream application instance to ensure the changes take effect.