# Upgrade Edge Nodes Manually


Upgrade individual Edge Nodes using CLI.

---

Upgrading individual Edge Nodes manually via the CLI gives you more control over the process than automatic Fleet upgrade,
letting you pause and resume upgrades as needed and select specific versions to upgrade to.

To upgrade an individual Edge Node manually:

## Prepare the Installation

1. Download the Cribl package from the [Cribl download page](https://www.cribl.io/download).
   Pay attention to select the correct package for your platform and processor type.
   If the Leader is on a prior release, see Cribl Past Releases section at the bottom of the page for older packages. 
1. Stop the Edge Node.

## Install the New Edge Node

{{% tabs "linux" "linux" "Linux" "windows" "Windows" "macos" "macOS" %}}
{{% tab-item "linux" %}}

1. Uncompress the new Edge version on top of the old one, for example in the `/opt/cribl-edge` directory: 

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

{{% /tab-item %}}
{{% tab-item "windows" %}}

1. Uninstall the existing version of Cribl Edge. Run:

   ```shell
   msiexec /x {GUID}
   ```
   
   You can get the `GUID` from `HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Uninstall`.

   Alternatively, if you have the original MSI that you installed, you can run:

   ```
   msiexec /x <cribl-package-msi-file>
   ```

   If you prefer to use PowerShell to uninstall the MSI, run:

   ```powershell
   $app = Get-WmiObject -Class Win32_Product -Filter "Name = 'Cribl Edge'"
   $app.Uninstall()
   ```

1. Install the new Cribl Edge version: 

   ```
   msiexec /i <criblpackage-msi-file> /qn /l*v C:\cribl-install.log KEEPDATA=1
   ```

`KEEPDATA=1` persists data between installs. `KEEPDATA` will not overwrite the
existing configurations, state, logs, etc.

When an older Edge Node is uninstalled, the product configurations are left
behind under `C:\ProgramData\Cribl`, which the installer can reuse for the new
Cribl Edge Node.

As soon as you install the new MSI, it will automatically connect to the same
Leader and join the same Fleet as before.

{{% /tab-item %}}
{{% tab-item "macos" %}}

1. Rerun the installer:

   ```shell
   sudo installer -pkg <package-file>.pkg -target /
   ```

{{% /tab-item %}}
{{% /tabs %}}

## Finalize Installation

To finalize the installation:

1. Restart Cribl Edge.
1. Ensure that newly upgraded Edge Nodes report to the Leader with their new software version. 
1. Commit and deploy the newly updated configuration **only after all** Edge Nodes have upgraded.

Your Edge Node upgrade is now complete.

## Upgrade RPM Installations {#rpm}

Manual upgrade is the only possible way of upgrading Edge Nodes install using RPM packages.

To upgrade an RPM installation:

1. Download the new Cribl Edge RPM package for your desired version.
1. Use your system's package manager (such as `yum` or `apt-get`) to upgrade the existing Edge Node:
   
   ```shell
   sudo yum update <path-to-RPM-package>
   ```

The upgrade retains all changes you make to environment variables.

For large organizations, Cribl recommends establishing an internal repository for RPM packages.
This centralized approach streamlines upgrades by eliminating the need to download individual RPMs to each node, improving efficiency and consistency.