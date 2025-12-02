# Persistent Socket Connections




[Distributed deployments](deploy-distributed) use Unix domain sockets for inter-process communication
(IPC) between a Leader Node's distributed processes and services. Multiple Unix
domain sockets must exist (created by their respective process) on a Leader's
disk. One of these domain socket files ensures that the metrics services
operate correctly. On the host where the Leader Node is running, the default
location for Unix domain socket files is the operating system temporary directory –
for example, `/tmp`.

Many Linux distributions maintain a system cleaner service (such as [systemd-tmpfiles](https://man.archlinux.org/man/systemd-tmpfiles-clean.service.8)) that removes files from the temporary directory periodically, such as every 10 days. If Cribl's sockets are removed, certain product pages (such as [Monitoring](monitoring)) break. 

You can protect the sockets by either blocking the cleaner or moving the Cribl socket files.

## Block the Cleaner {#block}

You can stop the host operating system from cleaning socket files out of `/tmp/cribl-*` subdirectories. Using Amazon Linux 2 instances as an example: 

1. Add a new `tmp.conf` file to `/etc/tmpfiles.d` with the line: `X /tmp/cribl-*`

1. Restart the system cleaner and reload its configuration using this command: 
`systemctl restart systemd-tmpfiles-clean.service`

1. Restart the Cribl server via the [UI](configuration-files#restart) or [command line](cli-restart).

## Move the Sockets {#move}

Alternatively, you can move the Cribl socket files to a different, protected directory. The user that owns the `cribl` process must have the necessary permissions to write to this directory.

In Unix-like operating systems, the maximum length for Unix domain socket paths is 108 bytes. To minimize the path length, use a directory that is close to the root, such as `/var/tmp`.

Specify the directory in **Settings** at **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Helper processes socket dir**.