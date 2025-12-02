# Upgrading Edge via UI


Transporting an Edge installation into the future. 

---

This topic explains how to use the UI to upgrade, back up, or roll back a Cribl
Edge [single-instance](deploy-single-instance) or
[distributed](setting-up-leader-and-edge-nodes) deployment on Linux. If you
prefer to do these operations manually – that is, at the command line – see
the [Upgrading Manually](upgrading-manually) topic. 

When you upgrade Cribl Edge via the UI, you must follow one of these upgrade
paths:

| Current Version            | Upgrade Path                       |
|----------------------------|------------------------------------|
| 4.x                        | 4.x                                |
| 3.x                        | 3.x through 4.x                    |
| 2.x                        | 2.x through 4.x                    |
| 1.7.x or 2.0.x             | 2.x.x, then 3.x or 4.x             |
| 1.6.x or below             | 1.7.x, then 2.x.x, then 3.x or 4.x |

## Upgrading the Leader Node

To upgrade the Leader in a distributed deployment, go to **Settings**, then
select **Upgrade** under **Global Settings**.

When upgrading, you can choose to either download upgrade packages from Cribl's
content delivery network (CDN), or from a filesystem location that you specify
in the form of a path.

The filesystem location must be accessible to the running Leader application,
both in connectivity and permissions. 

### Selecting a Package Source

Here, you'll select between **CDN** and **Path** for your upgrade (in an
on-prem, distributed deployment):

|Package Source|Description|When You Select This Package Source|
|------------|-----------|-----------|
|CDN         |Downloads an installation package directly from Cribl's content delivery network.|You'll see the currently installed version and the version available on the CDN. The Leader will always upgrade to the current CDN version when you click **Upgrade**.|
|Path        |You specify the path to the installation package.|You must specify paths to installation packages that have compatible versions and architectures with your Leader and Edge Fleets. For example, a Worker Group installed on an ARM64 architecture will require an ARM64 installation package.|

Use the buttons to choose the desired package source. Then, the UI will display
settings and information appropriate for the package source you chose, as
described in the respective [**CDN**](#package-source-cdn) and
[**Path**](#package-source-path) sections below.

#### Configuring Settings for CDN Upgrade

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

Defaults to `No`. Enabling Legacy upgrades removes the ability to automatically
upgrade Nodes on a per-Fleet basis. 

If you experience any upgrading issues with the 4.5.0+ Fleet upgrade
functionality, consider toggling this to `Yes` to use the job-based upgrade
framework and manually upgrade Fleets via the Fleet Upgrade UI and
**Upgrade** button. You'll also need to toggle **Disable Jobs/Tasks** to `No`. 
See [Disable Jobs/Tasks](settings-group-fleet#job-limits).

You may need to refresh the Global Settings page after enabling
legacy upgrades to view the **Fleet Upgrade** tab.

#### Configuring Edge Settings for Path Upgrade

![Upgrade Edge using Path](upgrade-edge-4-6.png)
{scale="60%" border="true"}

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
  upgrade is available and you click the **Upgrade** button.
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

1. Click the `?` icon, which copies the path to your clipboard.
1. Paste the copied package link into a new **Platform-specific package location** field.
1. Add the appropriate path directory to the beginning of the installation package path (where your Cribl upgrade packages are located).
   
   For example: `myfolder/directory/cribl-package.tgz`

1. Click **Save** to add the upgrade package path and resolve the warning.

### Upgrading Edge Nodes

You can upgrade the Nodes in a Fleet from the **Manage > Fleets**
page. Upgrade options are on a per-Fleet basis:

- Choose **None** to never upgrade any of the Nodes in the Fleet.
- Select a target software version to upgrade all of the eligible Nodes in the
  Fleet to the selected version.

To upgrade the Nodes in a selected Fleet to a target version:

1. Go to **Manage > Fleets**.
2. In the **Target Software Version** column, click the current version (or **None**, if applicable).

![New Fleet upgrade interface](target-software-version.png)

3. Click the **Upgrade target version** drop-down and select a version.

![Target version window](target-version-modal.png)
{scale="50%"}

The next time the Node in the selected Fleet connects to the Leader, the Node
will request the selected target version, and the Leader will upgrade the Node
to that version. 

Nodes in the selected Fleet will not upgrade to any future
version until you select or change the target software version in the drop-down
(even if you upgrade the Leader).

If you set the target software version to **None**, the Nodes in the selected
Fleet will never upgrade. This lets you keep Edge Nodes at a specific version,
even if you upgrade the Leader.

#### Subfleets

Subfleets do not inherit the **Target software version** from their parent
Fleet, but you can set the target software version for each Subfleet using the
same steps you used in the parent Fleet.

#### Fleet Upgrade Information

To view the upgrade-eligible Nodes in the selected Fleet and the
Nodes that are on versions 4.4.4 and older, click the **Upgrade status** icon in
the Target Software Version column.

![Upgrade status icon](upgrade-status-icon.png)
{scale="35%"}

To find upgrade details for Nodes that were upgraded prior to 4.5.0, click the
**Legacy Upgrade Jobs** tab.

#### Containerized Node Upgrades

If you're asking, "Why didn't my Kubernetes Node upgrade?" or perhaps, "Why
_did_ my Docker Node upgrade?", then you're in the right place. 

Cribl Edge handles upgrades for containerized Nodes (4.5.0 and newer) differently than standalone
or on-prem Nodes. Leader Nodes don't upgrade Edge Nodes that are running in a
container (such as containerd, Docker, or Kubernetes), because Edge Nodes revert
back to their Helm chart image after a Pod restarts. Instead, upgrade
containerized Nodes manually. See [Upgrading Kubernetes Edge Nodes](upgrading-manually#upgrading-kubernetes-edge-nodes).

Upgrades for containerized Nodes also work a little differently in 4.5.0 and
newer compared to 4.4 and older. Consider these upgrade scenarios:

|Scenario                                                                                                                                    | Do the Nodes Upgrade? | Logic Explanation                                                                                                                                                                                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A Fleet contains  **Kubernetes / containerized**  Nodes running version 4.5.0. The Fleet has a set  **Target Software Version**  of 4.5.2. | No                    | <ul><li>The Nodes are running in a container. Containerized (Docker, Kubernetes, etc) Nodes will not  be upgraded automatically.</li>  <li>Nodes on version 4.5.0 and later inform the Leader that they are containerized Nodes.</li></ul>                                |
| A Fleet contains  **Kubernetes / containerized**  Nodes running version 4.4. The Fleet has a set  **Target Software Version**  of 4.5.0.   | Yes                   | <ul><li>Nodes on version 4.4 and older do not have the ability to inform the Leader that they're containerized.</li></ul>                                                                                                                                              |

## Greatgreen Goats: A Fleet Upgrading Use Case

To help you conceptualize automatic Fleet upgrades, this use case walks you
through how the fictitious company Greatgreen Goats upgrades Nodes in their two
Fleets.

Greatgreen Goats is a small but rapidly growing company focused on
installing and maintaining solar energy systems at goat farms around the country.

They use Cribl Edge to manage data on their edge nodes, divided into two
distinct Fleets:

- **Fleet 1: Linux Operations.** This Fleet contains 20 Nodes, all running Edge
  v.4.4.4. When the IT admin upgrades the Leader of Fleet 1 to 4.5, and they set the
  target upgrade version in the drop-down to **None**. Therefore, Nodes in
  Fleet 1 are never upgraded. They continue to remain at their current version,
  4.4.4.
- **Fleet 2: Windows Monitoring.** This Fleet contains 15 Nodes. In this Fleet,
  the IT admin selects the current Leader version (in this case, 4.5) in the
  **Target upgrade version** drop-down. Now, when Nodes connect to the Leader,
  they pull down version 4.5 and upgrade to that version. 
  
Fast forward four weeks: Cribl Edge has released a new version: 4.5.2. The
Leader is now at version 4.5.2.  

Since Fleet 1 does not have a target version selected, Nodes in Fleet 1 remain
at their current version, 4.4.4. If the IT admin decides to change the
**Target upgrade version** to match the Leader in Fleet 1, the Nodes in Fleet 1
will be upgraded to 4.5.2. 

Nodes in Fleet 2 will remain on version 4.5, which was the Leader's previous
version, until the admin selects the next **Target upgrade version** (4.5.2, the
Leader's new version). Then, Nodes will request the new version the next time
they connect to the Leader and automatically upgrade.

This approach gives the IT admin at Greatgreen the flexibility to upgrade Nodes in
Fleets independently from the Leader. Greatgreen can now scale and upgrade their
Fleets with more reliability, since Nodes are pulling the release down from the
Leader (instead of the Leader creating an overwhelming number of upgrade jobs).

### Legacy Upgrade Jobs

To view upgrades for Nodes older than 4.4.4, click the info icon in the **Target
Software Version** column and click the **Legacy Upgrade Jobs** tab.

All Nodes in the selected Fleet that are on version 4.4.4 or older will be
upgraded using the jobs framework. If legacy upgrade jobs exist, you can find
them here in the Legacy Job Upgrades tab.

## Backup and Rollback

When you initiate an upgrade through the UI, Edge first stores a backup of your
current stable deployment. If the upgrade fails, then by default, Edge will
automatically roll back to the stored backup package. You can adjust this
behavior at **Settings** > **Global Settings** > **System** >
**General Settings** > **Upgrade & Share Settings**, using the following
controls. 

> Edge can perform rollbacks only on Nodes that were running at least
> v.3.0.0 before the attempted upgrade. 
>
{.box .warning}

**Enable automatic rollback**: Edge will automatically roll back an upgrade if
the Edge server fails to start, or if the Edge Node fails to connect to the
Leader. (Toggle to `No` to defeat this behavior.)

**Rollback timeout (ms)**: Time to wait, after an upgrade, before checking each
Node's health to determine whether to roll back. Defaults to `30000`
milliseconds, i.e., 30 seconds.

**Rollback condition retries**: Number of times to retry the health check before
performing a rollback. Defaults to `5` attempts.

**Check interval (ms)**: Time to wait between health-check retries. Defaults to
`1000` milliseconds, i.e., 1 second.

**Backups directory**: Specify where to store backups. Defaults to
`$CRIBL_HOME/state/backups`.

**Backup persistence**: A relative time expression specifying how long to keep
backups after each upgrade. Defaults to `24h`.
