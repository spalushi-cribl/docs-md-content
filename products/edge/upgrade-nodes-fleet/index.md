# Upgrade Edge Nodes in a Fleet


Upgrade a whole Fleet of Edge Nodes by defining a target version.

---

To automatically upgrade Edge Nodes in one Fleet, you can set the desired target version for the Fleet through the UI.


> Upgrading Edge Nodes from the Leader is available on certain plan/license tiers.
> For details, see [Pricing](https://cribl.io/pricing/).
{.box .info}

Automatic Fleet upgrades have the following limitations:

- You can't automatically upgrade Edge Nodes installed via RPM. Refer to [Upgrade RPM Installations](upgrade-nodes-manually#rpm) for the manual upgrade process for those Nodes.
- You can't automatically upgrade Edge Nodes on Windows running version 4.3.0 or older. Use [manual upgrade](upgrade-nodes-manually) instead.
- Upgrading a Single-instance deployment requires a manual upgrade.

## Upgrade Edge Nodes

To automatically upgrade Edge Nodes in a Fleet:

1. In the sidebar, select **Fleets**.
1. In the row for the Fleet you want to upgrade, select the button in the **Target Software Version** column.
1. In the **Target Version** modal, select a version from the **Upgrade target version** dropdown. You have two options:
   - **None** - when set to None, the Nodes in the Fleet will never upgrade automatically.
     Choose this option for highly critical Edge Nodes or Nodes managed by other departments that require strict control over upgrades.
   - **\<version\> (current leader version)** - when set to this option, the Nodes will be upgraded to match the software version of the currently running Leader Node.
1. Confirm with **Save**.

The next time the Node in the selected Fleet connects to the Leader, it will upgrade to the selected version. 

Nodes in the selected Fleet will not upgrade to any future version until you change the target software version in the drop-down (even if you upgrade the Leader).

> The **Add/Update Edge Node** command uses the **Target Software Version** for your Fleet when constructing the script for upgrading your Edge Nodes. For details, see [Edge Node Updates Using Fleet Version Settings](edge-nodes-add-update#fleets-target).
{.box .info}

### Subfleet Target Version

Subfleets do not inherit the **Target software version** from their parent
Fleet, but you can set the target software version for each Subfleet using the
same steps you used in the parent Fleet.

## Package Sources for Offline Upgrades {#package-sources}

Automatic Edge Node upgrade in Fleets will download packages from the Cribl CDN.
For offline use in an on-prem deployment, you can also pre-download packages for specific platforms and architectures.
To indicate that the system should use those packages:

1. Go to **Settings** > **Global** > **System** > **Upgrade**.
1. Select **Path** in **Package source**.
1. In **Platform-Specific Package Location**, enter the path to your locally downloaded package.
   You can add multiple paths to target different platforms or target Cribl Edge versions.
1. In **Package Hash Location**, point to the SHA or MD5 hash file for each of the packages.
1. Select **Save**.

When the Fleet next upgrades, it will use those downloaded packages instead of CDN.

## Enable Legacy Edge Upgrades (Deprecated)

> While this functionality still exists, it is deprecated and we do not recommend using it.
{.box .warning}

Enabling legacy upgrades removes the ability to automatically upgrade Nodes on a per-Fleet basis. 

If you experience issues with the Fleet upgrade functionality,
this option lets you use the job-based upgrade framework
and manually upgrade Fleets via the Fleet Upgrade UI and **Upgrade** button.

To enable legacy upgrades, go to **Settings** > **Global** > **System** > **Upgrade**,
and in **Edge Fleets**, under **Upgrade Options**, select **Enable Legacy Edge upgrades**.
You'll also need to toggle **Disable Jobs/Tasks** off. 
See [Jobs and Tasks](fleet-settings#job-limits).
