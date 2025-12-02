# Installing Cribl Edge on Windows


You can install Cribl Edge on Windows Server 2016, 2019, or 2022. To start: 

1. Set a compatible browser as your default browser: Microsoft Edge, Firefox, or Chrome. (Cribl Edge is not compatible with Internet Explorer.)
2. Cribl Edge on Windows relies on PowerShell for certain operational tasks. Verify that PowerShell is installed and accessible on your Windows systems.
3. Ensure that the required ports are available (see [Network Ports](deploy-planning#fleet-checklist)).
4. From Cribl's [Download page](https://www.cribl.io/download), set the **Software** drop-down to either the `.exe` or `.msi` installer.
5. Click **Download Now** to get the selected installer.

## Select the Installation Type

You now have two installation options: 
- Launch the `.exe` to install via an interactive [wizard](#ui).
- Use the `.msi` to install via a [command prompt](#msi), or to script bulk installs. 

Choose one of these methods, but not both, because the `.exe` versus `.msi` installers can install incompatible configurations.

Either method installs Cribl Edge as a Windows service. This enables Cribl Edge to automatically restart whenever the  Windows Server reboots.

If you are on Cribl.Cloud, the [Enabling TLS After Installation](#tls) section will walk you through creating an `instance.yml` file upon initial Cribl Edge/Windows installation, and then copying it to the same location for each subsequent install. 

> Please see [Known Issues](known-issues) for any current limitations on automatic version upgrades.
>
{.box .info}

### Using the Wizard {#ui}

1. Click the Cribl Edge `.exe` to launch the wizard installer.

2. On the installer's first page (below), select either **Single Instance** or **Managed Edge Node** as the installation type.

    > For Cribl.Cloud, Select **Managed Edge Node**.
    {.box .cloud}

    ![Select installation type](ed-windows-install-1.e6e66d1046.png)
{border="true"}

3. Enter the **Leader URI** (`<Leader-hostname-or-IP>:4200`), and optionally enter the **Fleet** and **Auth Token**. 

    > If you're on Cribl.Cloud, see [Retrieving Cribl.Cloud Credentials](#cloud-creds) to get these values.
    {.box .cloud}

    ![Managed Edge Node details](ed-windows-install-2.84b9fa72b4.png)
    {border="true"}

4. On the wizard's next page, confirm or change the **Destination location**. (The default path will expand as `C:\Program Files\Cribl\bin`).

    ![Select location](ed-windows-install-3.95090f1d15.png)
    {border="true"}

5. On the third page, optionally select **Create a desktop shortcut**.

    ![Desktop shortcut option](ed-windows-install-5.03b6a9a674.png)
    {border="true"}

6. On the final **Ready to Install** page, review your chosen options and click **Install** to confirm.

    ![Installation summary](ed-windows-install-4.892e499310.png)
    {border="true"}

7. When installation is complete, double-click the Cribl Edge icon on your Desktop or File Explorer. This will launch Cribl Edge's login page in your default browser. Or go to `http://localhost:9420` and log in with default credentials (`admin`:`admin`).

### Retrieving Cribl.Cloud Credentials {#cloud-creds}

On Cribl.Cloud, retrieve the **Leader URI** and **Auth Token** from your Cloud instance like this:

1. Navigate to the [**Manage Edge Nodes**](managing-edge-nodes#) page.
1. From the **Add/Update Edge Node** control at upper right, select **+ Bootstrap new**.
1. Copy the **Leader hostname/IP** value, excluding the `https://`. 
1. The **Leader Port** field is `4200`. 
1. Display the **Auth Token** value, and copy/paste it into the installer. The **Auth Token** is required to enable communication between the Leader and Edge Node. 

> **Cribl Edge 4.7.2 and newer includes a Powershell-specific bootstrap script.**
>
> The provided Cribl Edge bootstrap command might not work directly in
> PowerShell due to unescaped quotes in the `APPLICATIONROOTDIRECTORY` variable.
> When running the bootstrap command in PowerShell, escape the double quotes
> within the `APPLICATIONROOTDIRECTORY` variable. Use backticks as escape
> characters.
{.box .warning}

### Using the MSI Installer {#msi}

From the command line, you can install Cribl Edge as a single instance or as a Managed Node.

#### Installing Single-Instance Cribl Edge {#si}

To run Edge locally, as a single-instance deployment on your own machine, enter the following at your command prompt for a silent install:  

  ```shell
  msiexec /i cribl-<version>-<build>-<arch>.msi
   ```
Next, go to `http://localhost:9420` and log in with default credentials (`admin:admin`). 

You can now start configuring Cribl Edge with [Sources](/Edge/sources) and [Destinations](/Edge/destinations), or start creating [Routes](/Edge/routes) and [Pipelines](/Edge/pipelines).


#### Installing Managed Edge Nodes {#node}

To run Cribl Edge as a managed Node, enter the following at your command prompt for a silent install: 

 ```shell
 msiexec /i cribl-<version>-<build>.msi COMMAND="mode-managed-edge -H <master-hostname-or-IP> -p <port> -u myAuthToken [-S <true|false>]" 
```

You must include the `-H` and `-p` parameters to specify the Leader's host and port. Optionally, you can include the `-S true` or `-S false` parameters to (respectively) enable or disable TLS communication between this Node and the Leader.

You can now manage this node from the specified Leader.

For other parameter options, see the [CLI Reference](cli-reference#mode-managed-edge). Here is an example command:

 ```shell
msiexec /i cribl-<version>-<build>.msi COMMAND="mode-managed-edge -H 192.0.2.1 -p 4200 -u myAuthToken -g myfleet"
```

### Enabling TLS After Installation {#tls}

Some on-prem deployments don't require secure (TLS) communication between the Leader and Edge Node. In these cases, the Managed Node will complete its configuration, followed by removing the `admin/admin` password. There will be no reason to locally log in, as you can manage the Edge Node via the Leader UI. 

For sites using secure TLS connections to the Leader, including Cribl.Cloud, you must configure the local Edge Node to enable TLS. To do this:

1. Log into your local instance (at `http://localhost:9420`) with the default credentials (`admin/admin`). 
1. Enable TLS locally. For details, see [Connecting to the Leader Securely](securing-communications#connect-workers). 
1. When prompted, restart your Cribl Edge instance.
1. Locate the[`instance.yml`](instanceyml) file in `Program Files\cribl\local\_system`.

To avoid this post-installation update for subsequent Windows installations to other servers, you can copy the `instance.yml` from this server. After the installation is complete, paste the `instance.yml`file into subsequent servers' `\Program Files\cribl\local\_system` folder. 

To connect each new Cribl Edge instance to this config file, you'll need to either restart the Windows Server, or simply run the following commands as a Windows Administrator:

  ```shell
 net stop cribl

 net start cribl
  ```

## Troubleshooting and Logging

Here are some helpful tips: 

To debug `.msi` installation or upgrade issues, you can run `msiexec.exe` to set logging options. See Microsoft's [Logging Options](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec#logging-options) topic.


If you want to change the installation directory, use the `msiexec` command `APPLICATIONROOTDIRECTORY`. For example:

```shell
msiexec.exe /i Cribl-Edge.msi APPLICATIONROOTDIRECTORY="C:\test\" /L*V "C:\Log\CriblInstall.log"
```

If you're using PowerShell to run the silent install, add two extra pairs of quotes to escape the command's own quote delimiters. For example:

```shell
msiexec /i cribl-<version>-<build>.msi COMMAND="""mode-managed-edge -H 192.0.2.1 -p 4200 -u myAuthToken -g myfleet"""
```

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