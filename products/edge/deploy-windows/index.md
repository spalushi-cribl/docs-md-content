# Installing Cribl Edge on Windows


Install and deploy Cribl Edge Nodes on-prem on Windows.

---

You can install Cribl Edge in a [supported Windows environment](#supported-windows-versions). 

## Minimum Requirements



The following requirements are the minimum for installing and running an Edge Node on Windows.

> Below is a summary of requirements for a Cribl Edge deployment.
> See [Requirements](requirements) for more detailed information.
{.box .warning}

### OS Requirements



Cribl Edge supports these versions of Windows:

|<div style="width: 150px">Version</div>|Notes|
|-------------|----------------|
|Windows 10 and 11| You can deploy Cribl Edge on laptops, desktops, and workstations running Windows 10 and 11. <br>Keep these things in mind when deploying Cribl Edge on Windows 10 and 11 workstations:<ul><li>Cribl Edge might lose some data sent to TCP Destinations after a workstation resumes from a suspended or sleep state. This happens when the data is in local network buffers but hasn't been sent out to the Destination. Instead, you can send data to an HTTP Destination.</li><li>You might see partially broken events after a laptop resumes from a suspended state.</li><li>HTTP Destinations might contain duplicate events.</li><li>Some users have observed excessive logging when a Leader is unavailable to receive heartbeat messages from an Edge Node.</li></ul>|
| Windows Server 2016 <br> Windows Server 2019 <br> Windows Server 2022 <br> Windows Server 2025 | Functionality is the same as generally supported Windows Server platforms. |


### Application Requirements

Minimum application requirements for Edge Nodes are:

- ~1Ghz processor
- 512MB RAM
- 5GB free disk space per volume for operational purposes

## Before You Install Cribl Edge on Windows

1. Set a compatible browser as your default browser: Microsoft Edge, Firefox, or Chrome. (We support the five most-recent versions of these browsers. Cribl Edge is not compatible with Internet Explorer.)
1. Ensure that the required ports are available (see [Ports](ports)).
1. Go to the Cribl [Download page](https://www.cribl.io/download) and set the **Software** drop-down to `Cribl Edge for Windows`.
1. Click **Download Now** to get the `.msi` installer.

You can also concatenate and copy/paste the [bootstrap script](edge-nodes-add-update#bootstrap) in your command prompt to add Windows Nodes.

> To install Cribl Edge, you might need to add exceptions to your antivirus or endpoint security tools (such as Windows Defender). For details, see [Installing or Upgrading Nodes with Endpoint Security​](edge-common-challenges#installing-or-upgrading-nodes-with-endpoint-security).
{.box .warning}

## About Environment Variables {#env-var}

Environment variables specified during installation are added to the service configuration in the Windows registry under `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\cribl` in the `Environment` key. 

When setting environment variables:

* You can add but not remove environment variables using the interactive wizard.
* Variables are merged in the following priority order (highest to lowest):
  1. Variables specified via command line during installation.
  2. Existing environment variables already configured for the service.
  
This ensures that command-line specifications take precedence over existing configurations, while preserving any previously set variables not explicitly overridden.

The `CRIBL_VOLUME_DIR` variable is stored separately in the Windows registry using the NSSM configuration path, which is different from where other environment variables are stored. NSSM is the service management tool used by Cribl Edge to run as a Windows service. For details, see [What is the `nssm.exe` process](#nssm) in the Frequently Asked Questions section below.


## Select the Installation Type

You now have two installation options: 
- Launch the `.msi` to install via an interactive [wizard](#ui).
- Use the `.msi` to install via a [command prompt](#msi), or to script bulk installs. 

Either method installs Cribl Edge as a Windows service. This enables Cribl Edge to automatically restart whenever the  Windows Server reboots.

If you are on Cribl.Cloud, the [Enabling TLS After Installation](#tls) section will walk you through creating an `instance.yml` file upon initial Cribl Edge/Windows installation, and then copying it to the same location for each subsequent install. 

> Please see [Known Issues](known-issues) for any current limitations on automatic version upgrades.
>
{.box .info}

## Use the Wizard {#ui}

1. Double-click the Cribl Edge `.msi` to launch the Cribl Edge Setup Wizard.
   Select **Next** to start. 

1. Read the **End-User License Agreement** and check the box to accept the
   terms. For the most current copy and details, see
   [Terms of Service](https://cribl.io/legal/terms/). 

1. On the wizard's next page, confirm or change the **Destination Folder**. (The
   default path will expand as `C:\Program Files\Cribl`).

   ![Select location](ed-windows-install-3.6ab4c1845d.png)
   {border="true" scale="75%"}

1. If the installer detects a config file from a previous Cribl Edge
   installation, the modal will ask you if you want to keep
   your current config or create a new config file for the installation.
   Select **Keep config** to persist your previous configurations stored in the
   `instance.yml` file.

   ![Keep Config](wizard-previous-config.png)
   {border="true" scale="75%"}

1. Select either **Managed** or **Single-instance** as the installation type.
   See [Types of Deployment](deploy-planning#types-of-deployment) for information
   on these options.
    
   If you chose **Keep config** in the previous window, the installer will skip
   the installation type selection.

   > For Cribl.Cloud, Select **Managed Edge Node**.
   {.box .cloud}

   ![Select installation type](wizard-install-type.png)
   {border="true"}

1. If you select **Managed** mode, enter the Leader hostname or IP address,
   port, auth token, and optionally the Fleet and any tags. For details on getting these credentials from Cribl.Cloud, see [Retrieve Cribl.Cloud Credentials](#cloud-creds).

   ![Connection Details](wizard-managed-settings.png)
   {border="true"}

1. Select a Windows account type for the Cribl Edge service:

    - **Local System Account**: The default account type. This is a local
       Windows service account that does not have a corresponding user profile.
    - **Username / Password**: Enter your **Username** and **Password**
      credentials to run Cribl Edge. To successfully log Cribl Edge as a service,
      you might need to include the domain name in the **Username**.

   ![Use Credentails](wizard-service-account.png)
   {border="true"}

1. Optionally, select **Set Environment Variables** to set environment variables for the Edge service during installation.   

   ![Set Environment Variables](environment-variables1.png)
   {border="true"} 

    In this example, HTTP and HTTPS proxy settings are being defined to enable Cribl Edge to communicate through corporate proxies, with each variable specified on a separate line.

   > Setting environment variables is optional and typically not needed for standard installations. This feature is primarily used for special configurations such as configuring proxy settings, enabling FIPS compliance mode, or other advanced scenarios.
   {.box .info}  <br>

   <br> If you've already provided environment variables via the command line, or if there are existing variables, the UI will be pre-populated with these values when you open it. For details, see [About Environment Variables](#env-var) above.


1. Optionally, you can create a desktop shortcut. 

   ![Desktop shortcut option](ed-windows-install-5.8770c95762.png)
   {border="true"}

1. On the final **Ready to Install** page, click **Install** to confirm.

   ![Installation summary](ed-windows-install-4.7d20fd6f8e.png)
   {border="true"}

1. When installation is complete, double-click the Cribl Edge icon on your Desktop or File Explorer. This will launch Cribl Edge's login page in your default browser. Or go to `http://localhost:9420` and log in with default credentials (`admin`:`admin`).

> As of Cribl Edge v.4.0, the modified data (including the `instance.yml`) now goes to `C:\ProgramData\Cribl` instead of `C:\Program Files\Cribl`.
>
{.box .info}

### Retrieve Cribl.Cloud Credentials {#cloud-creds}

On Cribl.Cloud, retrieve the **Leader URI** and **Auth Token** from your Cloud instance like this:

1. Navigate to the [**Manage Edge Nodes**](managing-edge-nodes) page.
1. From the **Add/Update Edge Node** control at upper right, select **Bootstrap new**.
1. Select **Add Windows**.
1. Copy the **Leader or Outpost hostname/IP** value.
1. The **Leader Port** field is `4200`. 
1. Display the **Auth Token** value, and copy/paste it into the installer. The **Auth Token** is required to enable communication between the Leader and Edge Node. 

## Use the MSI Installer {#msi}

From the command line, you can install Cribl Edge as a single instance or as a Managed Node. See [User Context and MSI Installer Options](deploy-windows-msi-options) for important details regarding user accounts and installer settings.

### Install Single-Instance/Standalone Cribl Edge {#si}

To run Edge locally, as a single-instance deployment on your own machine, enter the following at your command prompt for a silent install:  

```shell
msiexec /qn /i cribl-<version>-<build>-<arch>.msi 
```

Next, go to `http://localhost:9420` and log in with default credentials (`admin:admin`). 

You can now start configuring Cribl Edge with [Sources](/edge/sources) and [Destinations](/edge/destinations), or start creating [Routes](/edge/routes) and [Pipelines](/edge/pipelines).


### Install Managed Edge Nodes {#node}

You can use the Leader UI to concatenate and copy/paste the [bootstrap script](edge-nodes-add-update#bootstrap) for adding a Windows Node. 

Or, to run Cribl Edge as a managed Node, enter the following at your command prompt for a silent install: 

```shell
msiexec /qn /i cribl-<version>-<build>.msi MODE=mode-managed-edge HOSTNAME=<yourhostname> PORT=4200 AUTH=myAuthToken FLEET=myfleet
```

To set one or more environment variables during installation, use the `SETENVVARS` parameter with pipe-delimited key-value pairs. The entire string must be quoted:

```shell
msiexec /qn /i cribl-<version>-<build>.msi setenvvars="HTTP_PROXY=http://203.0.113.1:8080|HTTPS_PROXY=http://203.0.113.1:8080|CRIBL_DIST_WORKER_PROXY=socks5://203.0.113.1:1080|NODE_EXTRA_CA_CERTS=C:\Temp\mitmproxy-ca-cert.pem" MODE=mode-managed-edge HOSTNAME=<yourhostname> PORT=4200 AUTH=myAuthToken FLEET=myfleet 
```

The variable names and values cannot contain the pipe character ("|") as it's used as the separator.

For Cribl Edge v.4.1.0 or later, use the flag `KEEPDATA=1` to persist data between installs. `KEEPDATA` will not overwrite the existing configurations, state, logs, etc.

```shell
msiexec /qn /i cribl-<version>-<build>.msi KEEPDATA=1
```

For prior versions, use the flag `COPYDATA=1` to persist data between installs. `COPYDATA` copies the `instance.yml` file from `C:\Program Files\Cribl` to `C:\ProgramData\Cribl`.

You can now manage this node from the specified Leader.

For other parameter options, see the [CLI Reference](cli-mode-managed-edge). Here is an example command:

```shell
msiexec /qn /i cribl-<version>-<build>.msi  MODE=mode-managed-edge HOSTNAME=192.0.2.1 PORT=4200 AUTH=myAuthToken FLEET=myfleet COPYDATA=1
``` 

### MSI Installer Properties

Below is a comprehensive list of installer properties specific to the Cribl Edge MSI package, along with their descriptions. These properties allow you to customize your Cribl Edge installation when using the `msiexec` command.

| Installer Property       | Description                                                                                                                                                                                                                                                                                                                              |
| :----------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MODE` | Sets the deployment mode for Cribl Edge. For example, use `mode-managed-edge` for a managed node. If not specified, a single-instance deployment is implied.                                                                                                                                                                            |
| `HOSTNAME` | Specifies the **Leader's hostname or IP address** when installing Cribl Edge in `mode-managed-edge`.                                                                                                                                                                                                                                 |
| `PORT`| Specifies the **Leader's port** when installing in `mode-managed-edge`. The default port is `4200`.                                                                                                                                                                                                                                      |
| `AUTH` | Provides the **authentication token** required for the Edge Node to connect to the Leader.                                                                                                                                                                                                                                                 |
| `FLEET`| Assigns the newly installed Edge Node to a **specific Fleet** within your Cribl deployment.                                                                                                                                                                                                                                               |
| `SETENVVARS` |Lets you **define multiple environment variables** for the installed Cribl Edge service. You pass a string of pipe-delimited `KEY=VALUE` pairs. This property injects environment variables into the service's configuration at installation time.                                              |
| `KEEPDATA` (v4.1.0+)   | When set to `1`, this property instructs the installer to **preserve existing configuration, state, and logs** during an upgrade, ensuring your data is not overwritten.                                                                                                                                                               |
| `COPYDATA` (pre-v4.1.0) | When set to `1`, this property copies the `instance.yml` file from the old installation path to the new data location during an upgrade, helping to **persist your configuration**.                                                                                                                                                   |
| `USERNAME` | Specifies the **Windows account** under which the Cribl Edge service will run. Examples include `LocalSystem` (the default) or a specific user account.                                                                                                                                                                                 |
| `PASSWORD` | Specifies the login password for the above username, only valid when using a specific user account (not LocalSystem).                                                                                                                                                                                |
| `APPLICATIONROOTDIRECTORY`| Defines the **installation directory for the Cribl Edge application binaries**. This is where the core program files will be placed.                                                                                                                                                                                                   |
| `VOLUMEDIR` | Specifies a **separate directory for Cribl Edge's runtime data**, including configurations, logs, and state information. This allows you to store data independently from the application binaries.                                                                                                                                        |


## Enable TLS After Installation {#tls}

Some on-prem deployments don't require secure (TLS) communication between the Leader and Edge Node. In these cases, the Managed Node will complete its configuration, followed by removing the `admin/admin` password. There will be no reason to locally log in, as you can manage the Edge Node via the Leader UI. 

For sites using secure TLS connections to the Leader, including Cribl.Cloud, you must configure the local Edge Node to enable TLS. To do this:

1. Log into your local instance (at `http://localhost:9420`) with the default credentials (`admin/admin`). 
1. Enable TLS locally. For details, see [Secure Leader/Edge Nodes Communication](securing-communications). 
1. When prompted, restart your Cribl Edge instance.
1. Locate the [`instance.yml`](instanceyml) file in:
   - Cribl Edge v.4.0.X or older: `Program Files\cribl\local\_system`.
   - Cribl Edge v.4.1 or newer: `C:\ProgramData\Cribl`.

To avoid this post-installation update for subsequent Windows installations to other servers, you can copy the `instance.yml` from this server. After the installation is complete, paste the `instance.yml`file into subsequent servers' `\Program Files\cribl\local\_system` folder. 

To connect each new Cribl Edge instance to this config file, you'll need to either restart the Windows Server, or simply run the following commands as a Windows Administrator:

```shell
net stop cribl

net start cribl
```

## Add or Update a Windows Node

Using the Cribl Edge UI, you can add or update Windows Edge Nodes in your existing deployment.

For more information
about adding or updating Edge Nodes, see [Adding and Updating Edge Nodes](edge-nodes-add-update).

To add or update a Windows Edge Node:

1. In your Cribl Edge instance, select **Edge Nodes** from the sidebar (you can also
   select **Fleets**).
1. Select **Add/Update Edge Node** then **Windows** > **Add** or **Update**.
1. Fill out the fields in the Add Windows Node window. The scripts update in real-time as you change the
   fields.

   ![Add or update a Windows Edge Node](add-windows-node-4101.png)
   {border="true" scale="75%"}
1. Copy and paste the resulting bootstrap script into a command prompt.
1. The result is a new Edge Node configured with the options you chose. You should
   see the Edge Node appear in the **Edge Nodes** list.

### Log the Time of Installation

The Windows bootstrap script includes a log file with a string of zeros 
(`/l*v "%SYSTEMROOT%\Temp\cribl-msiexec-0000000000000.log"`). You can replace the
zeros with an epoch timestamp (in milliseconds), to log when you first installed the Windows
Node.

For example, if you're adding a Windows Node on October 4, 2024 at 6:07:44 PM Pacific Time,
update the log file in the script to `/l*v "%SYSTEMROOT%\Temp\cribl-msiexec-1728090464000.log"`. 

This log is included in the Cribl Edge diag, which is helpful for
troubleshooting issues with Windows Node deployments.

> When the Leader initiates an upgrade of Cribl Edge, the log file name will
> automatically include Linux epoch time in milliseconds as the timestamp.
{.box .success}

## Troubleshooting and Logging

Here are some helpful tips: 

### Set Logging Options

To debug `.msi` installation or upgrade issues, you can run `msiexec.exe` to set logging options. See Microsoft's [Logging Options](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec#logging-options) topic.

### Change the Installation Directory

If you want to change the installation directory, use the `msiexec` command `APPLICATIONROOTDIRECTORY`. For example:

```shell
msiexec.exe /i Cribl-Edge.msi APPLICATIONROOTDIRECTORY="C:\test\" /L*V "C:\Log\CriblInstall.log"
```

If you're using PowerShell to run the silent install, add two extra pairs of quotes to escape the command's own quote delimiters. For example:

```shell
msiexec /i cribl-<version>-<build>.msi MODE=mode-managed-edge HOSTNAME=192.0.2.1 PORT=4200 AUTH=myAuthToken FLEET=myfleet
```
### Change the Data Directory Location

By default, Cribl Edge stores its runtime data in `C:\ProgramData\Cribl`. To move this data directory to a different location, use the `VOLUMEDIR` property during installation. 

`VOLUMEDIR` specifies the runtime data location, not where application binaries are stored. If the directory doesn't already exist, it will be created. This setting must be configured during the initial MSI installation or any subsequent upgrades. For example: 

```shell
msiexec /qn /i cribl-<version>-<build>.msi  VOLUMEDIR=D:\CriblData MODE=mode-managed-edge HOSTNAME=192.0.2.1 PORT=4200 AUTH=myAuthToken FLEET=myfleet KEEPDATA=1 
```
This stores runtime data in `D:\CriblData` instead of the default `C:\ProgramData\Cribl`, while keeping application binaries in `C:\Program Files\Cribl`.


## Frequently Asked Questions {#nssm}

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

**Q: What registry changes does a Cribl Edge installation make?**

A: The Cribl Windows installer (MSI) uses the Windows Registry behind the scenes, but Cribl itself doesn't need it to run. The installer uses the registry to set up the Cribl service and to remember your settings for future upgrades. Specifically, it uses these locations:
- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Cribl` for service definition
- `HKEY_LOCAL_MACHINE\SOFTWARE\Cribl` for upgrade settings

The MSI installer uses the registry for installation and upgrades, but direct interaction is unnecessary, as Cribl manages its configuration through other means.