# Upgrading Edge Leader via UI


Upgrade a Cribl Edge Node to a newer version via the UI.

---

This topic explains how to use the UI to upgrade a Cribl Edge Leader Node in a
Linux [distributed](setting-up-leader-and-edge-nodes) deployment. 

If you need to upgrade a single-instance deployment of Cribl Edge, or you prefer
to perform upgrade operations in a command-line interface, see the 
[Upgrade Manually](upgrading-manually) topic.

When you upgrade Cribl Edge via the UI, you must follow one of these upgrade
paths:

| Current Version            | Upgrade Path                       |
|----------------------------|------------------------------------|
| 4.x                        | 4.x                                |
| 3.x                        | 3.x through 4.x                    |

## Upgrade the Leader Node

To upgrade the Cribl Edge Leader, go to **Settings**, then select **Upgrade**
under **Global Settings**. 

> Remember, these steps only apply to on-prem, distributed deployments
> of Cribl Edge, not Cribl.Cloud deployments or single-instance deployments.
{.box .info}

You can choose to download upgrade packages from one of two sources:

- The Cribl content delivery network (CDN).
- A filesystem location that you specify in the form of a path. The running
  Leader application must have the connectivity and permissions to access the
  filesystem location.

### Select a Package Source

Once you're in **Global Settings**, you will select between **CDN** and **Path**
for your upgrade. 

> In Cribl.Cloud, **CDN** and **Path** options don't appear.
{.box .cloud}

|Package Source|Description|When You Select This Package Source|
|------------|-----------|-----------|
|CDN         |Downloads an installation package directly from Cribl's content delivery network.|You'll see the currently installed version and the version available on the CDN. The Leader will always upgrade to the current CDN version when you select **Upgrade**.|
|Path        |You specify the path to the installation package.|You must specify paths to installation packages that have compatible versions and architectures with your Leader and Edge Fleets. For example, a Worker Group installed on an ARM64 architecture will require an ARM64 installation package.|

Use the buttons to choose the desired package source. Then, the UI will display
settings and information appropriate for the package source you chose, as
described in the respective [**CDN**](#package-source-cdn) and
[**Path**](#package-source-path) sections below.

#### Configure Settings for CDN Upgrade

If you select **CDN** as your upgrade method, you will see the following options:

- **Current CDN version**: Lists the latest version of Cribl Stream available on the CDN.
- **Leader**: This shows the currently installed version of Cribl Stream on the
  Leader. If a newer version is available, you will be able to use the **Upgrade to** button.
- **Stream Worker Groups**: Provides information and options for upgrading
  Stream Groups. See the [Stream Documentation](/stream/upgrading#stream-worker-groups-with-cdn-selected)
  for more details.
- **Edge Fleets**: Displays **Upgrade Options**, including the deprecated
  **Enable Legacy Edge upgrades**.

##### Enable Legacy Edge Upgrades (Deprecated)

>While this functionality still exists, it is deprecated and we do not recommend using it.
{.box .warning}

Toggle defaults to off. Enabling Legacy upgrades removes the ability to automatically
upgrade Nodes on a per-Fleet basis. 

If you experience any upgrading issues with the 4.5.0+ Fleet upgrade
functionality, consider toggling this on to use the job-based upgrade
framework and manually upgrade Fleets via the Fleet Upgrade UI and
**Upgrade** button. You'll also need to toggle **Disable Jobs/Tasks** off. 
See [Disable Jobs/Tasks](fleet-settings#job-limits).

You may need to refresh the Global Settings page after enabling
legacy upgrades to view the **Fleet Upgrade** tab.

#### Configure Edge Settings for Path Upgrade

With **Path** as your upgrade method, you'll see the following settings and
information in the **Package Source** table:

- **Platform-Specific Package Location**: Enter or paste the path to the Cribl 
  installation package. This can be either of the following: 
    
    * An HTTP URL, for example `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz`
    * A local filesystem path, for example `myfolder/directory/cribl-package.tgz`

- **Package Hash Location**: Enter either of the following:

    * An HTTP URL, for example `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz.sha256`
    * A local filesystem path to the hash that validates the package.

    Supports SHA‑256 and MD5 formats. You can simply append `.sha256` to the
    contents of the **Platform‑specific package location** field.

- **Leader**: Indicates the version the Leader will be upgraded to when an
  upgrade is available and you select the **Upgrade** button.
- **Stream**: The version in this row will be used to upgrade the Stream Worker Group,
  because it matches the Leader's version.
- **Edge**: The version in this row will be used to [upgrade Edge Fleets](#upgrading-edge-nodes) 
  with a matching target version.
- **Version**: Displays the version of the upgrade package you added in this row.
- **Platform**: Displays the platform architecture for the package in this row.

Select **X** to immediately delete a row – there is no confirmation prompt.

> You can add multiple rows to this table to specify packages for different
> versions and platforms/architectures. To obtain the latest packages from
> https://cribl.io/download, use the drop-down list to specify each platform
> (for example, x64 versus ARM). When you stage these packages on your own servers,
> preserve the original file names.
>
{.box .success}

**Leader** area: Shows you the currently installed Leader version and
whether an upgrade is available. Check the [backup and
rollback](#backup-and-rollback) settings before you upgrade.

**Stream Worker Groups**: Provides information and options for upgrading
Stream Groups. See the [Stream Documentation](/stream/upgrading#configuring-stream-settings-for-path-upgrade)
for more details.

**Edge Fleets**: Offers **Upgrade Options** for Edge. See 
[Enable Legacy Edge Upgrades](#enable-edge-legacy-upgrades) 
for more information.

##### Missing Package Links

When you see this warning, it means there are Edge Fleets that can't be
upgraded due to a missing package path. 

To add a missing installation package:

1. Select the `?` icon, which copies the path to your clipboard.
2. Paste the copied package link into a new **Platform-specific package location** field.
3. Add the appropriate path directory to the beginning of the installation package path (where your Cribl upgrade packages are located).
   
   For example: `myfolder/directory/cribl-package.tgz`

4. Select **Save** to add the upgrade package path and resolve the warning.

