# SELinux (Enforcing Mode) Configuration


SELinux (Security-Enhanced Linux) is a mandatory access control system for Linux
that provides a granular level of security by defining access controls for applications, processes, and files.
It uses security policies, which are a set of rules that tell SELinux what can or can't be accessed,
to enforce the access allowed by a policy.

SELinux has two modes, **enforcing** and **permissive**.
When running in enforcing mode, it strictly enforces the SELinux policy and denies access based on policy rules.
This can sometimes cause conflicts with applications that aren't explicitly allowed to perform certain actions.

## Cribl Edge and SELinux

To effectively integrate Cribl Edge with SELinux, consider the different policy requirements for in-stream processing and network ports.

* Cribl Edge's in-stream processing often involves predefined processes and files, allowing for a relatively restrictive SELinux policy.
* Network ports might require a more permissive policy because users configure them within the UI.

## Key Steps for SELinux Configuration

To ensure optimal integration of Cribl Edge with SELinux, begin by verifying the current SELinux status and the context of the Cribl Edge binary.

### Check SELinux Status

Use the `getenforce` command to determine the current status of SELinux on your system.

The output will be one of the following:

* `Enforcing`
* `Permissive`
* `Disabled`

If the output is `Enforcing`, SELinux is enabled and enforcing policies.

### Verify Cribl Edge's SELinux Context

Use the command `ls -Z /opt/cribl/bin/cribl` to list the contents of the directory `/opt/cribl/bin` while displaying their SELinux security contexts.

The output should show the SELinux context for the Cribl Edge binary.

Example output:

`-rwxr-xr-x. cribl cribl cribl unconfined /opt/cribl/bin/cribl`

> Deploy Cribl Edge directly to `/opt` to avoid context issues. Do not deploy Cribl Edge to a user’s home directory, or to root (`/`), and then move it to `/opt`.
>
{.box .warning}

### Revert Labels on Files and Directories

If needed, you can also use [`restorecon`](https://man7.org/linux/man-pages/man8/restorecon.8.html),
an SELinux policy utility from the `policycoreutils-python-utils` package,
to revert labels on files and directories to their default SELinux security context:

```bash
restorecon -R -v /opt/cribl
```

### Check for SELinux Denials

Use `auditd` to log SELinux denials. Look for messages related to Cribl Edge in the `/var/log/audit/audit.log` file.

### Modify SELinux Policy 

If you encounter SELinux denials, you might need to modify the SELinux policy. There are three options ranging from least secure to most secure. 

#### Use `semanage` (Less Secure)

Using `semanage` is the simplest but least secure method. It grants permissive access to the `cribl_t type`, allowing all operations.

```bash
semanage permissive -a cribl_t
```

#### Use `audit2allow` (Slightly More Secure)

Using `audit2allow` creates SELinux policy rules to permit actions that were previously denied.

To identify and address SELinux denials related to Cribl Edge, follow these steps:
1. Find denied operations:
   
   ```
   sudo grep cribl /var/log/audit/audit.log | audit2allow -M cribl
   ```

1. Install the generated policy:
   
   ```
   sudo semodule -i cribl.pp
   ```

#### Create a Custom Policy (Most Secure)

Use tools like `secil` or `policycoreutils` to create a custom policy tailored to Cribl Edge's needs. This provides the highest level of security but requires more technical expertise.

### Reload SELinux Policy

Once you've altered the SELinux policy using tools like `semanage`, `audit2allow`, or through custom policy creation, use this command to ensure that the new rules take effect immediately.

```bash
sudo semanage reload -a
```

## Specific Cribl Edge Considerations

In addition to the general SELinux guidelines, pay close attention to these Cribl Edge–specific factors: 

* **Cribl Edge configuration**: Review Cribl Edge configuration files for any settings that might conflict with SELinux restrictions.
* **Integration with other applications**: Ensure that Cribl Edge's interactions with other applications are compatible with their SELinux contexts and permissions.

## Additional Considerations

In addition to the basic configuration, these factors are also important for optimal performance:

* **SELinux booleans**: Check the SELinux documentation for relevant booleans that might affect Cribl Edge's behavior.
* **SELinux context labeling**: Ensure files and directories used by Cribl Edge have the correct SELinux context. Use tools like `restorecon` to relabel files.
* **SELinux troubleshooting tools**: Use tools like `audiscan` and `audit2why` to analyze SELinux audit logs.

## SELinux Troubleshooting

SELinux problems usually surface when you try to start the Cribl Edge service. Users immediately receive an error like the following:

```shell
Job for cribl.service failed because the control process exited with error code.
See "systemctl status cribl.service" and "journalctl -xe" for details.
```

One indication that the problem is in SELinux configuration, not in the Cribl Edge service itself, is when (1) nothing has been logged in `$CRIBL_HOME/log/cribl.log`, and (2), when you run `journalctl -xe` you see errors like the following:

```shell {title="Errors from journalctl -xe"}
cribl.service: Failed to execute command: Permission denied
cribl.service: Failed at step EXEC spawning /opt/cribl/bin/cribl: Permission denied

cribl.service: Control process exited, code=exited status=203
cribl.service: Failed to execute command: No such file or directory
cribl.service: Failed at step EXEC spawning /bin/rm -f /opt/cribl/pid/cribl.pid: Permission denied
```

You can confirm that SELinux configuration is the issue by verifying that the Cribl Edge service is able to start up on its own. To do this, run the `cribl start` command.

Here are three commands that you can think of as a basic SELinux troubleshooting toolkit:
* `getenforce` verifies that SELinux is enabled and enforcing policy.
* `sestatus -v` shows details of the SELinux policy that's in effect.
* `ls` with the `-Z` or `--context` option shows the SELinux tags on files and/or folders. In the output, the fifth column reports the SELinux context in the form `user:role:type:level`.

### Example Problem

Suppose you run `ls -lZ /opt/cribl` to see the SElinux context for `/opt/cribl`. 

If the output looks like the following, there is a problem: the presence of "home" among the `type` part of the Linux context. In this case, `user_home_t` is the offending type. When the type is `user_home_t` or `admin_home_t`, SELinux will not permit the service to read, write, or execute files.

```shell {title="Problematic SELinux context output"}
$ ls -laZ /opt/cribl
total 0
drwxr-xr-x. 6 cribl cribl unconfined_u:object_r:user_home_t:s0  62 Feb 16 08:31 .
drwxr-xr-x. 3 root  root  system_u:object_r:usr_t:s0            19 Feb 16 19:54 ..
drwxr-xr-x. 2 cribl cribl unconfined_u:object_r:user_home_t:s0 202 Feb 16 08:31 bin
drwxr-xr-x. 5 cribl cribl unconfined_u:object_r:user_home_t:s0  49 Feb 16 08:20 data
drwxr-xr-x. 6 cribl cribl unconfined_u:object_r:user_home_t:s0  59 Feb 16 08:31 default
drwxr-xr-x. 3 cribl cribl unconfined_u:object_r:user_home_t:s0  22 Feb 16 08:31 thirdparty
```

### Example Solution

To fix the labeling, download Cribl Edge again directly to `/opt`. **Do not** download Cribl Edge to a user's home directory, or to root (`/`), and then move it to `/opt` - that was what caused our example problem.

```shell {title="Downloading Cribl to /opt"}
curl -Lso - $(curl -s https://cdn.cribl.io/dl/latest) | sudo tar zxvf - -C /opt
```

A simpler option is to revert the labels using `restorecon`:

```shell {title="Reverting labels with restorecon"}
restorecon -R -v /opt/cribl
systemctl restart cribl
```

Once you correct the problem, the output will look like the following. Instead of `*home_t` listings in the `type` field, this SELinux context has `usr_t`, a type that will satisfy SELinux's criteria for allowing the service to read, write, and execute files. 

```shell {title="Corrected SELinux context output"}
# ls -laZ /opt/cribl
total 0
drwxr-xr-x. 6 cribl cribl unconfined_u:object_r:usr_t:s0  62 Feb 16 08:31 .
drwxr-xr-x. 3 root  root  system_u:object_r:usr_t:s0      19 Feb 16 20:05 ..
drwxr-xr-x. 2 cribl cribl unconfined_u:object_r:usr_t:s0 202 Feb 16 08:31 bin
drwxr-xr-x. 5 cribl cribl unconfined_u:object_r:usr_t:s0  49 Feb 16 08:20 data
drwxr-xr-x. 6 cribl cribl unconfined_u:object_r:usr_t:s0  59 Feb 16 08:31 default
drwxr-xr-x. 3 cribl cribl unconfined_u:object_r:usr_t:s0  22 Feb 16 08:31 thirdparty
```
