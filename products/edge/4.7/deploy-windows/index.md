# Installing Cribl Edge on Windows


You can install Cribl Edge in a [supported Windows environment](#supported-windows-versions). 

[Snippet not found: content/edge/4.7/snippets/_windows-supported-versions.md]

## Before You Install Cribl Edge on Windows

1. Set a compatible browser as your default browser: Microsoft Edge, Firefox, or Chrome. (We support the five most-recent versions of these browsers. Cribl Edge is not compatible with Internet Explorer.)
2. Cribl Edge on Windows relies on PowerShell for certain operational tasks. Verify that PowerShell is installed and accessible on your Windows systems.
3. Ensure that the required ports are available (see [Network Ports](deploy-planning#fleet-checklist)).
4. Go to the Cribl [Download page](https://www.cribl.io/download) and set the **Software** drop-down to `Cribl Edge for Windows`.  
5. Click **Download Now** to get the `.msi` installer.

You can also concatenate and copy/paste the [bootstrap script](managing-edge-nodes#add-node) in your command prompt to add Windows Node.

## Select the Installation Type

You now have two installation options: 
- Launch the `.msi` to install via an interactive [wizard](#ui).
- Use the `.msi` to install via a [command prompt](#msi), or to script bulk installs. 

Either method installs Cribl Edge as a Windows service. This enables Cribl Edge to automatically restart whenever the  Windows Server reboots.

If you are on Cribl.Cloud, the [Enabling TLS After Installation](#tls) section will walk you through creating an `instance.yml` file upon initial Cribl Edge/Windows installation, and then copying it to the same location for each subsequent install. 

> Please see [Known Issues](known-issues) for any current limitations on automatic version upgrades.
>
{.box .info}

### Using the Wizard {#ui}

1. Double-click the Cribl Edge `.msi` to launch the Cribl Edge Setup Wizard. Click **Next** to start. 

1. Read the **End-User License Agreement** and check the box to accept the terms. For the most current copy and details, see [Terms of Service](https://cribl.io/legal/terms/). 

1. On the wizard's next page, confirm or change the **Destination Folder**. (The default path will expand as `C:\Program Files\Cribl\bin`).

   ![Select location](ed-windows-install-3.6ab4c1845d.png)
   {border="true"}

1. If the installer detects a config file from a previous installation (prior to v.4.1), the modal will ask you if you want to keep your current config. Check the box next to `Keep Config` to persist your previous configurations stored in the `instance.yml` file. Click **Next** to continue.

   ![Keep Config](ed-windows-install-3b.65bea5f2d2.png)
   {border="true"}

1. If you chose to keep your previous configuration, the installer will skip this option to **Select the installation type**. Select either **Managed** or **Standalone** as the installation type.

   > For Cribl.Cloud, Select **Managed Edge Node**.
   >
   {.box .cloud}

   ![Select installation type](ed-windows-install-1.7b0a8dc11b.png)
   {border="true"}

1. If you select **Managed** mode, enter the Leader URI (`<Leader-hostname-or-IP address>)`, Auth Token, and optionally the Fleet. 

   ![Connection Details](ed-windows-install-3d.1f2c4ca673.png)
   {border="true"}

1. Optionally, **Log in as Local System** is checked by default. Uncheck it to enter your **Username** and **Password** credentials to run Cribl Edge. To successfully log Cribl Edge as a service, you might need to include the domain name in the **Username**. Click **Next** to continue.

   ![Log in as Local System](ed-windows-install-3c.cf450d416d.png)
   {border="true"}

   ![Use Credentails](ed-windows-install-3f.42ef24b00a.png)
   {border="true"}

1. Optionally select **Create a desktop shortcut**.

   ![Desktop shortcut option](ed-windows-install-5.8770c95762.png)
   {border="true"}

1. On the final **Ready to Install** page, click **Install** to confirm.

   ![Installation summary](ed-windows-install-4.7d20fd6f8e.png)
   {border="true"}

1. When installation is complete, double-click the Cribl Edge icon on your Desktop or File Explorer. This will launch Cribl Edge's login page in your default browser. Or go to `http://localhost:9420` and log in with default credentials (`admin`:`admin`).

> As of Cribl Edge v.4.0, the modified data (including the `instance.yml`) now goes to `C:\ProgramData\Cribl` instead of `C:\Program Files\Cribl`.
>
{.box .info}

### Retrieving Cribl.Cloud Credentials {#cloud-creds}

On Cribl.Cloud, retrieve the **Leader URI** and **Auth Token** from your Cloud instance like this:

1. Navigate to the [**Manage Edge Nodes**](managing-edge-nodes#) page.
1. From the **Add/Update Edge Node** control at upper right, select **Bootstrap new**.
1. Select **Add Windows**.
1. Copy the **Leader hostname/IP** value.
1. The **Leader Port** field is `4200`. 
1. Display the **Auth Token** value, and copy/paste it into the installer. The **Auth Token** is required to enable communication between the Leader and Edge Node. 


> The provided Cribl Edge bootstrap command might not work directly in PowerShell due to unescaped quotes in the `APPLICATIONROOTDIRECTORY` variable.
> When running the bootstrap command in PowerShell, escape the double quotes within the `APPLICATIONROOTDIRECTORY` variable. Use backticks as escape characters. 
> 
{.box .warning}

### Using the MSI Installer {#msi}

From the command line, you can install Cribl Edge as a single instance or as a Managed Node.

#### Installing Single-Instance/Standalone Cribl Edge {#si}

To run Edge locally, as a single-instance deployment on your own machine, enter the following at your command prompt for a silent install:  

```shell
msiexec /i cribl-<version>-<build>-<arch>.msi /qn 
```

Next, go to `http://localhost:9420` and log in with default credentials (`admin:admin`). 

You can now start configuring Cribl Edge with [Sources](/Edge/sources) and [Destinations](/Edge/destinations), or start creating [Routes](/Edge/routes) and [Pipelines](/Edge/pipelines).


#### Installing Managed Edge Nodes {#node}

You can use the Leader UI to concatenate and copy/paste the [bootstrap script](managing-edge-nodes#add-node) for adding a Windows Node. 

Or, to run Cribl Edge as a managed Node, enter the following at your command prompt for a silent install: 

```shell
msiexec /i cribl-<version>-<build>.msi /qn MODE=mode-managed-edge HOSTNAME=<yourhostname> PORT=4200 AUTH=myAuthToken FLEET=myfleet
```

For Cribl Edge v.4.1.0 or later, use the flag `KEEPDATA=1` to persist data between installs. `KEEPDATA` will not overwrite the existing configurations, state, logs, etc.

```shell
msiexec /i cribl-<version>-<build>.msi /qn KEEPDATA=1
```

For prior versions, use the flag `COPYDATA=1` to persist data between installs. `COPYDATA` copies the `instance.yml` file from `C:\Program Files\Cribl` to `C:\ProgramData\Cribl`.

You can now manage this node from the specified Leader.

For other parameter options, see the [CLI Reference](cli-reference#mode-managed-edge). Here is an example command:

```shell
msiexec /i cribl-<version>-<build>.msi /qn MODE=mode-managed-edge HOSTNAME=192.0.2.1 PORT=4200 AUTH=myAuthToken FLEET=myfleet COPYDATA=1
```

### Enabling TLS After Installation {#tls}

Some on-prem deployments don't require secure (TLS) communication between the Leader and Edge Node. In these cases, the Managed Node will complete its configuration, followed by removing the `admin/admin` password. There will be no reason to locally log in, as you can manage the Edge Node via the Leader UI. 

For sites using secure TLS connections to the Leader, including Cribl.Cloud, you must configure the local Edge Node to enable TLS. To do this:

1. Log into your local instance (at `http://localhost:9420`) with the default credentials (`admin/admin`). 
1. Enable TLS locally. For details, see [Connecting to the Leader Securely](securing-communications#connect-workers). 
1. When prompted, restart your Cribl Edge instance.
1. Locate the [`instance.yml`](instanceyml) file in:
   - Cribl Edge v.4.0.X or earlier: `Program Files\cribl\local\_system`.
   - Cribl Edge v. 4.1 or later: `C:\ProgramData\Cribl`.

To avoid this post-installation update for subsequent Windows installations to other servers, you can copy the `instance.yml` from this server. After the installation is complete, paste the `instance.yml`file into subsequent servers' `\Program Files\cribl\local\_system` folder. 

To connect each new Cribl Edge instance to this config file, you'll need to either restart the Windows Server, or simply run the following commands as a Windows Administrator:

```shell
net stop cribl

net start cribl
```

### Troubleshooting and Logging

Here are some helpful tips: 

#### Setting Logging Options

To debug `.msi` installation or upgrade issues, you can run `msiexec.exe` to set logging options. See Microsoft's [Logging Options](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec#logging-options) topic.

#### Changing the Installation Directory

If you want to change the installation directory, use the `msiexec` command `APPLICATIONROOTDIRECTORY`. For example:

```shell
msiexec.exe /i Cribl-Edge.msi APPLICATIONROOTDIRECTORY="C:\test\" /L*V "C:\Log\CriblInstall.log"
```

If you're using PowerShell to run the silent install, add two extra pairs of quotes to escape the command's own quote delimiters. For example:

```shell
msiexec /i cribl-<version>-<build>.msi """MODE=mode-managed-edge HOSTNAME=192.0.2.1 PORT=4200 AUTH=myAuthToken FLEET=myfleet
"""
```


### Frequently Asked Questions

Here are some common questions about Cribl Edge on Windows, and the answers!

**Q: What is the `nssm.exe` process?**

A: `nssm.exe` is a wrapper for the `cribl.exe` binary. It gives Windows Service
Manager the ability to manage an executable program without having to provide a
number of required binary APIs on the executable. So, instead of the `cribl.exe`
binary needing to provide the APIs, `nssm.exe` acts as an intermediary, providing
the necessary APIs itself and subsequently spawning `cribl.exe` as a child
process.

**Q: Why is `nssm.exe` important?**

A: The `nssm.exe` process ensures that Cribl Edge is stable and reliable on
Windows, both by booting up Cribl Edge and restarting it for you. With
the `nssm.exe` process running, Cribl Edge remains operational even if it closes
unexpectedly. While some antivirus software might flag `nssm.exe` due to its
monitoring and restart functionality, `nssm.exe` is a legitimate and essential
component of Cribl Edge.

> High CPU utilization may appear shortly after launching Cribl Edge but is not
> necessarily related to `nssm.exe`. We recommend waiting for Cribl Edge to fully
> boot and stabilize before checking CPU usage.
{.box .info}