# Upgrade Leader Node


Upgrade the Leader Node of your Distributed deployment of Cribl Edge.

---

The Leader Node, which controls both Edge Nodes and Stream Workers, upgrades differently depending on your environment:

- In Cribl.Cloud, your Leader Node will be upgraded automatically as Cribl releases new product versions.
- In an on-prem deployment, you select when you want to upgrade your Leader Node.

There are two ways to upgrade the Leader Node in an on-prem deployment:
directly [from the UI](#ui), and [manually from the command line](#manually).

> Before you start upgrading your Leader Node, make sure to read [Prepare for an Upgrade](upgrading#prepare).
> 
> If your deployment uses a second Leader configured for failover, refer to [Upgrade with a Standby Leader](#second-leader).
{.box .success}

## Upgrade Leader from the UI {#ui}

To upgrade an on-prem Leader Node directly from the UI, go to **Settings** > **Global** > **System** > **Upgrade**.

You can choose to either use upgrade packages from the Cribl content delivery network (CDN),
or point to packages located in a specified remote or filesystem location:

|Package Source|Description|
|------------|-----------|
|<div style="width: 120px">CDN</div>  |Downloads an installation package directly from the Cribl content delivery network. The Leader will always upgrade to the current CDN version when you select **Upgrade**.|
|Path        |Uses an installation package from the filesystem or a remote URL. You must provide the correct path for your Leader platform and architecture. |

> If your Leader controls Stream Workers, upgrading it can cause Workers to upgrade automatically, depending on configuration.
> Refer to [Automatic Upgrades](/stream/upgrading#automatic-upgrades) for more information.
{.box .info}

### Upgrade Leader from CDN

To upgrade a Leader Node using packages hosted in Cribl CDN:

1. Go to **Settings** > **Global** > **System** > **Upgrade**.
2. Select **CDN** in **Package source**.

   ![Upgrade the Leader using CDN](upgrade-leader-cdn-4.14.png)
   {scale="80%" border="true"}

   If your Leader is running an older version, you will see the latest version of the Cribl package that is available on the CDN.

3. Upgrade the Leader by selecting **Upgrade to \<version\>**.

### Upgrade Leader from a Path

To upgrade a Leader Node using specified packages:

1. Go to **Settings** > **Global** > **System** > **Upgrade**.
2. Select **Path** in **Package source**.

![Upgrade the Leader using Path](upgrade-leader-path-4.14.png)
{scale="80%" border="true"}

3. Provide the Location of a Cribl package in **Platform-Specific Package Location**.

   You can either:

   - Point to an HTTP URL that houses the package, for example `https://cdn.cribl.io/dl/4.14.0/cribl-4.14.0-837595d5-linux-x64.tgz`, or:
   - Download the package from the [Cribl download page](https://cribl.io/download/) and point to local filesystem path, for example `/download/cribl-4.14.0-837595d5-linux-x64.tgz`.
   In the second case, the Leader must have access to the local filesystem location.
   When you stage these packages on your own servers, preserve the original file names.

   > Make sure you download the correct package for your platform and architecture.
   {.box .success}

4. Download the package's SHA or MD5 hash, or point to its URL in **Package Hash Location**.
   For a CDN location, you can append `.sha256` to the contents of the **Platform‑Specific Package Location** field.

   The **Leader** column in the table will confirm with an icon when the package will be used to upgrade the Leader.

   > When Cribl Edge is running in [FIPS mode](fips-mode), MD5 is not an acceptable hashing algorithm.
   {.box .info}

5. When you have added the correct package for the Leader, select **Save**.
6. Select **Upgrade to \<version\>** in the **Leader** section.

#### Additional Packages for Node Upgrades

You can use the **Platform-Specific Package Location** table to specify packages for different versions and platforms/architectures
for use when upgrading Stream Workers and Edge Nodes.

The **Stream** and **Edge** columns in the table will indicate which packages will be used to upgrade which Nodes in your deployment.

#### Missing Package Links

When you see a warning about "Missing packages", it means there are Worker Groups or Edge Fleets that can't be
upgraded due to a missing package path.

To add package links:

1. Select the **Copy** icon to add the link to your clipboard.
1. Paste the copied package link into a new **Platform-specific package location** field.
1. Add the appropriate path directory to the beginning of the installation package path (where your Cribl upgrade packages are located).

   For example: `/download/cribl-4.14.0-837595d5-linux-x64.tgz`

1. Select **Save** to add the upgrade package path and resolve the warning.

## Upgrade Leader Manually {#manually}

Besides upgrading from the UI, you can also upgrade your Leader Node manually, using the CLI.
This might be desired in, for example, airgapped deployments,
or when you want to select a Leader version lower than the latest available.

To upgrade the Leader Node manually:

1. Download the Cribl package in your target version, platform, and architecture
   from the [Cribl download page](https://cribl.io/download/).

1. Commit and deploy your current Leader configuration.

   Optionally, `git push` to your configured remote repo.

1. Stop the Leader.

   - Optional but recommended: Back up the entire `$CRIBL_HOME` directory.

   - Optional: Check that the Workers and Edge Nodes are still functioning as expected.
     In the absence of the Leader Node, they should continue to work with their last deployed configurations.

1. Uncompress the new Cribl package on top of the old one.

1. Change ownership of the `cribl` directory as needed:
  
   ```shell
   sudo chown -R cribl:cribl $CRIBL_HOME
   ```

1. Restart the Leader and log back in.

1. Wait for all the Workers and Edge Nodes to report to the Leader,
   and ensure that they are correctly reporting the last committed configuration version.

## Leader Upgrades and Edge Nodes

Edge Nodes will not upgrade automatically when you upgrade the Leader, even if set to current Leader version.
See [Upgrade Edge Nodes in a Fleet](upgrade-nodes-fleet) to learn how to upgrade all Edge Nodes in a Fleet.

## Upgrade with a Standby Leader {#second-leader}

For deployments with a [standby Leader](deploy-add-second-leader) configured for failover, follow this upgrade order:

1. Stop the standby Leader first.
1. Upgrade the standby Leader.
1. Start the standby Leader.
1. Stop the primary Leader; the upgraded standby will take over automatically through the HA failover mechanism.
1. Upgrade the now-stopped former primary Leader.
1. Start the former primary Leader. It will rejoin and assume the standby role (since the other Leader is currently active).
1. Upgrade Outpost Nodes, if present.
1. Upgrade Edge Nodes in rolling batches; do not upgrade them to a version newer than the Leader.
