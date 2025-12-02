# Manually Upgrading Windows Edge Nodes

Upgrade Cribl Edge Nodes on Windows.

---
## Manually Upgrading Windows Edge Nodes {#win-upgrade}

These are the steps to upgrade Windows Edge Nodes that are on version 4.3.0 and older. 

> Test this upgrade on a staging environment before making these changes to your
> production Fleet. 
{.box .warning}

To manage Windows Nodes from the Leader, upgrade to version 4.3.1 or newer. 

Consider using existing [out-of-band automation](https://en.wikipedia.org/wiki/Out-of-band_management) 
to perform the following steps: 

1. Stop Cribl Edge on each Edge Node.

1. Uninstall the existing version of Cribl Edge:

   - If you have the original MSI that you installed, you can run:

      ```
      msiexec /x cribl.xxx.msi
      ```

   - If not, you can get the `GUID` from:

      ```
      HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Uninstall
      ```

   And then run `msiexec /x {GUID}`

   - If you prefer to use PowerShell to uninstall the MSI, run:

      ```
      $app = Get-WmiObject -Class Win32_Product -Filter "Name = 'Cribl Edge'"
      $app.Uninstall()
      ```

1. Install the new Cribl Edge version: 

   ```
   msiexec /i cribl-<X.X.X>-*-win32-x64.msi /qn /l*v C:\cribl-install.log KEEPDATA=1
   ```

`KEEPDATA=1` persists data between installs. `KEEPDATA` will not overwrite the
existing configurations, state, logs, etc.

When an older Edge Node is uninstalled, the product configurations are left
behind under `C:\ProgramData\Cribl`, which the installer can reuse for the new
Cribl Edge Node.

As soon as the new MSI is installed, it will automatically connect to the same
Leader and join the same Fleet as before.

> The Leader can upgrade Windows Nodes by running Cribl Edge 4.3.0 or later under the `Local System` account, instead of under the `Administrator` account. 
>
{.box .info}


