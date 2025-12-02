# Scripts


> Scripts are available only in on-prem deployments, not in Cribl.Cloud.
{.box .cloud}

Admins can run scripts (for example, shell scripts) from within Cribl Stream by configuring and executing them at **Settings** > **Global** > **Scripts**. Scripts are typically used to call custom automation jobs or, more generally, to trigger tasks on demand. For example, you can use Scripts to run an Ansible job, or to place a call to another automation system, when Cribl Stream configs are updated.

> ##### With Great Power Comes Great Responsibility!
>
> Scripts will allow you to execute almost anything on the system where Cribl Stream is running. Make sure you understand the impact of what you're executing before you do so!
> 
{.box .warning}

## Enable Scripts

For security reasons, using scripts is disabled by default on all new deployments.
You can re-enable them by using the [`limits`](cli-reference#limits) command
(available since Cribl Stream 4.8.2).

To control script availability, run:

```text
# Enable scripts
./cribl limits -s true
# Disable scripts
./cribl limits -s false
```

When you enable or disable scripts, you need to restart the instance for the changes to take effect.

## Add Scripts

To add a new script, select **Add Script** on the scripts settings page.

![Settings > Scripts page](new-script.fc63cf4440.png)
{border="true"}

The **New Script** modal provides the following fields:

* **ID**: Unique ID for this script. 
* **Command**: Command to execute for this script. 
* **Description**: Brief description about this script. Optional.
* **Arguments**: Arguments to pass when executing this script.
* **Env variables**: Extra environment variables to set when executing script.

> ##### Scripts in Distributed Deployments
>
> * Scripts can be deployed from the Leader Node, but can be **run** only locally from each Worker Node.  
> 
> * If the Script command is referencing a file (for example, `herd.sh`), that file must exist on the Cribl Stream instance. In other words, the Script management interface cannot be used to upload or manage script files.
>
{.box .info}
