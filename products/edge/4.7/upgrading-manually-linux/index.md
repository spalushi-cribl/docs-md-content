# Upgrading Cribl Edge on Linux (CLI)


Learn how to upgrade Edge Nodes manually using a command line interface (CLI). 

---

> Prior to upgrading, review [Specific Considerations for Linux environments](upgrading-better-practices#linux-upgrades).
>
{.box .info}

## Upgrading a Linux Standalone/Single-Instance {#single}

This path requires upgrading only the single/standalone Node:

1. Stop Edge. 

1. Download the package on your instance of choice [here](https://www.cribl.io/download).

1. Uncompress the new version on top of the old one, for example in the `/opt/cribl-edge` directory: 

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

   - On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot
   open: File exists`. In this case, please remove `cribl/bin/cribl`
   and untar again. 
   - If you have **custom functions** in
   `cribl/bin/cribl`, please move them to
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

1. Restart Edge.

> The current upgrade process creates a new Cribl Edge installation in `/opt/cribl` by default, requiring you to manually move it to your existing Cribl Edge directory (usually `/opt/cribl-edge`) and adjust permissions.  For a simpler upgrade, the new version should be extracted directly into your existing Cribl Edge directory, overwriting the old one.  
>
{.box .info}

## Upgrading a Distributed Deployment on Linux {#distributed}

For a Distributed Deployment, the general order of upgrade is:

1. Upgrade the Leader Node.
1. Upgrade the Edge Nodes.
1. Commit and deploy the changes on the Leader.

> For distributed environments with a [second Leader](deploy-add-second-leader)
> configured for failover, this is the upgrade order:
>
> 1. Stop both Leaders.
> 2. Upgrade the primary Leader.
> 3. Upgrade the second Leader.
> 4. Upgrade each Edge Node, respectively.
>
{.box .info}

### Upgrading the Leader Node

1. Commit and deploy your desired last version. (This version will be your most recent
   checkpoint.)

   - Optionally, `git push` to your configured remote repo.

1. Stop Cribl Edge. 

   - Back up the entire `$CRIBL_HOME` directory (recommended, but optional).

   - Check that the Edge Nodes are still functioning as expected. In
     the absence of the Leader Node, they should continue to work with their
     last deployed configurations (optional).

1. Download the package on your instance of choice [here](https://www.cribl.io/download).

1. Uncompress the new Edge version on top of the old one, for example  in the `/opt/cribl-edge` directory: 

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

1. Restart Edge and log back in. 

1. Wait for all the Edge Nodes to report to the Leader, and ensure that they are
   correctly reporting the last committed configuration version. 

> Cribl Edge's UI will not be available until the Edge version has been
> upgraded to match the version on the Leader. Errors will appear
> until the Edge Nodes are upgraded. 
>
{.box .info}

### Upgrading the (Linux) Edge Nodes 

These are the same basic steps as when upgrading a [Single Instance](#single),
above:

1. Stop Cribl Edge on each Edge Node.

1. Download the package on your instance of choice
   [here](https://www.cribl.io/download). If the Leader is on a prior release,
   see [Cribl Past Releases](https://cribl.io/download/past-releases/) for older
   packages. 

1. Uncompress the new Edge version on top of the old one, for example in the `/opt/cribl-edge` directory: 

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

1. Restart Edge.

### Commit and Deploy Changes from the Leader Node

1. Ensure that newly upgraded Edge Nodes report to the Leader with their new
   software version. 

1. Commit and deploy the newly updated configuration **only after all**
   Edge Nodes have upgraded.