# Upgrading Edge on the Command Line


Learn how to upgrade Edge Nodes manually using a command line interface (CLI). 

---

In this guide, we'll cover benefits to upgrading manually and how to use the
command line to upgrade your Edge Nodes. 

> **A Word About Words**
> 
> We will use "upgrading manually" and "on the command line / CLI" interchangeably in this article.
>
{.box .info}

## Why Upgrade Cribl Edge Manually?

With Edge, you have the option to upgrade manually or [via the UI](upgrading-ui). Here are some
use cases and benefits to using the command line to upgrade:

### Flexibility and Control

Manually upgrading means you can pause and resume upgrades as needed,
which gives you more granular control over the process.

You can also upgrade individual Nodes separately. This is useful when you're
testing upgrades on a subset of Nodes before rolling out to the entire
deployment.

### Air-Gapped Environments and Custom Deployments

Consider upgrading via CLI when your organization can't connect to the internet
for security or compliance reasons. You don't need to be online to upgrade on
the command line.

When Cribl Edge is deployed in non-standard ways, such as via RPM package,
within containers, or using a container orchestrator like Kubernetes, manual
upgrades might be necessary.

### Specific Upgrade Paths

When you upgrade Edge Nodes on a CLI, you don't have to worry about a version
upgrade path - there are no restrictions on the versions you can upgrade to or
from.

## Upgrading a Standalone/Single-Instance {#single}

This path requires upgrading only the single/standalone Node:

1. Stop Edge. 

1. Download the package on your instance of choice [here](https://www.cribl.io/download).

1. Uncompress the new version on top of the old one, e.g., in the `/opt/cribl-edge` directory: 

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

## Upgrading a Distributed Deployment  {#distributed}

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

1. Uncompress the new Edge version on top of the old one, e.g., in the `/opt/cribl-edge` directory: 

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

1. Uncompress the new Edge version on top of the old one, e.g., in the `/opt/cribl-edge` directory: 

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

1. Restart Edge.

### Upgrading Edge Nodes Installed via RPM

See [Upgrading Edge via RPM](deploy-rpm#upgrading-edge-via-rpm).

### Upgrading Kubernetes Edge Nodes

[Edge Nodes deployed in Kubernetes](deploy-running-kubernetes) can't be upgraded
via the Leader. Instead, use our [Helm charts](https://github.com/criblio/helm-charts)
to upgrade the Edge Nodes deployed in your Kubernetes cluster: 
  
 ```
 helm upgrade --install
 ```


### Commit and Deploy Changes from the Leader Node

1. Ensure that newly upgraded Edge Nodes report to the Leader with their new
   software version. 

1. Commit and deploy the newly updated configuration **only after all**
   Edge Nodes have upgraded.


### Manual Rollback {#rollback}

Using [CLI commands](cli-reference#command-syntax), it's possible to explicitly
roll back an on-prem Leader, and Nodes, to an earlier release. This works much
like an upgrade.

Explicit rollback might be necessary if an [automatic rollback](upgrading-ui#back)
fails. Otherwise, Cribl recommends
first considering other options – an upgrade, or working with Cribl Support –
before performing a manual rollback. 

Rollback can encounter these complications:

- Your current configuration might take advantage of dependencies not supported
  in an earlier release.

> Do not manually roll back any Cribl Edge instance running in a container.
> Instead, locate, download, and launch a
> [container image](https://hub.docker.com/r/cribl/cribl/tags) hosting the earlier
> version you want. 
{.box .warning}

#### Rollback Outline {#rollback-outline}

We assume that you are rolling back to a previously deployed version that you
know to be stable in your environment. The broad steps are:

1. Ideally, [link](/stream/version-control#remote-setup) your deployment to a
   Git remote repo, and
   [commit and push](/stream/version-control#pushing-to-a-remote-repo) your
   Leader's configuration to that remote. The repo will provide a stable
   location from which to restore your config, if necessary.

2. Stop the Leader instance. (From `$CRIBL_HOME/bin/`, execute `./cribl stop`.)

3. Also create a local backup of your Leader's whole `$CRIBL_HOME` directory.

4. Obtain the installation package for the earlier release and platform you
   need. (See the [Rollback Example](#rollback-example).)

5. [Uncompress](deploy-single-instance#install) the earlier version to your
   original deployed directory. You can do this from the command line or
   programmatically (see the [Rollback Example](#rollback-example)).

  If installing to your existing target directory fails, try the same
  mitigations we list [above](#single) for upgrades.

6. In a [distributed deployment](#distributed), Nodes must not run a higher
   version than the Leader. Repeat steps 2, 3, and 5 above on all Nodes,
   substituting “Node” for “Leader.” (On Nodes where you've integrated
   Cribl Edge with systemd, stop the Cribl server with: `systemctl stop
   cribl-edge`.)

#### Rollback Example {#rollback-example}

While rollbacks can be partially scripted, discovering earlier installation
packages cannot be automated – the initial steps here are manual:

1. Stop and back up the Leader. (Follow steps 1–3 in the [Rollback Outline](#rollback-outline).)

1. Open the **Cribl Past Releases** page: https://cribl.io/download/past-releases/.

1. Locate the earlier version that you want to restore.

1. Use the adjacent drop-down to select your target platform.

1. Right-click (or `Ctrl`-click) the corresponding **Download** button, and copy
   its URL to your clipboard. In this example, we've copied:  
`https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-arm64.tgz`

1. Swap that URL into an untar command like the following. Run this command from
   the parent (typically `/opt/`) of your existing `$CRIBL_HOME` directory:  
`[sudo] curl -Lso - https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-arm64.tgz | tar zxv`

1. Repeat the preceding steps to adjust all Nodes to a compatible version.
