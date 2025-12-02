# Upgrading


Teleporting an Edge installation into the future. 

---

This page outlines how to upgrade a Cribl Edge
[single-instance](deploy-single-instance) or
[distributed deployment](setting-up-leader-and-edge-nodes) along one of the
following supported upgrade paths, which apply to UI-based upgrades:

| Current Version            | Upgrade Path                       |
|----------------------------|------------------------------------|
| 4.x                        | 4.x                                |
| 3.x                        | 3.x through 4.x                    |
| 2.x                        | 2.x through 4.x                    |
| 1.7.x or 2.0.x             | 2.x.x, then 3.x or 4.x             |
| 1.6.x or below             | 1.7.x, then 2.x.x, then 3.x or 4.x |

> If you're upgrading Edge Nodes directly from the command-line interface, you
> don't have to worry about a version upgrade path - there are no restrictions on
> the versions you can upgrade to or from.
>
{.box .info}

You can upgrade Cribl Edge in two ways:

- [Upgrade and Rollbacks via the UI](#ui)
- [Manual Upgrades and Rollbacks](#manual) allows you to upgrade by
  uncompressing the new version on top of the old one. 

## Considerations {#upgrade-considerations}

Here are some considerations to look at before you upgrade.

### General Upgrade Considerations

- Upgrading Edge Nodes from the Leader requires a Standard or Enterprise [license](licensing).

- Cribl Edge does **not** support direct upgrades from any pre-GA version (such as
  a Cribl-provided test candidate) to a GA version. To get the GA version running,
  you must perform a new install. 

- Before upgrading your Leader to v.4.0 and later, see
  [Persisting Socket Connections](setting-up-leader-and-edge-nodes#socket), to
  prepare the host to keep communications open from Workers. 

- Cribl Edge 4.1 and later encrypt TLS certificate private key files when you
  add or modify them. See instructions just below for backing up your keys from
  earlier versions.

- In Kubernetes, an Edge Node that is upgraded via the Leader reverts to its
  helm chart image after restarting a pod. There might be a version mismatch with
  the Leader as a result. Use the Helm Charts to upgrade the Edge Nodes on
  Kuberbenetes: `helm upgrade --install`.

## Leader and Edge Nodes Compatibility {#compatible}

Leaders on v.4.2.x are compatible with Edge Nodes on v.4.1.2, v.4.1.3, and
later. Due to a security update, Edge Nodes running on v.4.0.4 cannot receive
configurations from v.4.2.x Leaders.

If you’re planning to upgrade to v.3.5.4, note that any Edge Nodes must be at
the same version as the Leader for v.3.5.4. Leaders running v.3.5.4 and later
should test whether Edge Nodes are running a compatible version before deploying
configs that could break Edge Nodes' data flow. The Leader will prompt you to
upgrade these Nodes as needed.

To prevent Edge Nodes from failing on incompatible configurations, we introduced
three new behaviors announced in Cribl Edge 3.5.4 release notes. These will make data
throughput more resilient, but might enforce more-frequent (automated or
explicit) upgrades to Workers' Edge versions:

- The Edge Leader will block configuration deployments to Edge Nodes running an
  earlier Edge version that's incompatible with the new configs. The Leader will
  prompt you to upgrade these Nodes to a compatible version.
- If Workers detect a Source or Destination type for which they have no
  supporting configuration, they will ignore (skip initializing) that
  integration, rather than failing.
- For unsupported Destination types, Workers will create a temporary placeholder
Destination, but will exert Blocking backpressure until the configuration is
updated. You will see a warning of the form: `Skipping configuring
Destination...due to unrecognized type`.

## Safeguarding Unencrypted Private Keys for Rollback {#safeguarding}

Cribl Edge 4.1 and later encrypt TLS certificate private key files when you add
or modify them. 

> Before upgrading from a pre-4.1 version, make a backup copy of all
> unencrypted TLS certificate private key files. Having access to the unencrypted
> files is essential if you later find that you need to roll back to your previous
> version. 
>
{.box .warning}

To safeguard your unencrypted private keys, make a full backup of all Cribl
config files. Files in the `auth/certs` directory are particularly important,
such as those in:

- `groups/default/local/cribl/auth/certs/`
- `groups/<groupname>/local/cribl/auth/certs/`
- `cribl/local/cribl/auth/certs/`
-  Etc.

Take appropriate precautions to prevent unauthorized access to these unencrypted
private key files. If you need to roll back to a pre-4.1 version, see [Restoring Unencrypted Private Keys](securing-and-monitoring#restoring). 

## Upgrade and Rollback via the UI {#ui}

These options are available for Linux Edge Nodes. Currently, to upgrade **Windows** Edge Nodes, you must re‑run a Windows installer from the filesystem, rather than working from the UI. See [Upgrading Windows Edge Nodes](#win-upgrade). This Windows limitation also applies to the [Backup and Rollback](#back) subsection below.

### Upgrade the Leader Node via the UI

To upgrade the Leader in a distributed deployment, go to **Settings** > **Global Settings** > **System** > **Upgrade**. The following controls are available:

### Package Source

The default **CDN** button downloads a package directly from Cribl's content
delivery network. Selecting the alternative **Path** button exposes these
additional controls:

  - **Package location**: Enter either a URL (HTTP) or a local path to the
    upgrade package. HTTP example:
    `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz`
  - **Package hash location**: Enter either a URL (HTTP) or a local path to the
    hash that validates the package. Supports `sha256` and `md5` formats. You
    can simply append `.sha256` to the contents of the
    **Platform‑specific package location** field. HTTP example:  
  `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz.sha256`
  - **Save**/**Cancel** buttons: Click **Save** to store the specified
    locations. Clicking **Cancel** restores the **CDN** package-source
    selection.

> You can add multiple rows to this table to specify packages for
> different platforms/architectures. To obtain the latest packages from
> https://cribl.io/download, use the drop-down list to specify each platform
> (e.g., x64 versus ARM). When you stage these packages on your own servers,
> preserve the original file names. 
>
{.box .success}

### Automatic Upgrades {#automatic-upgrades}

**Disable automatic upgrades** defaults to `Yes` in
on-prem/​customer-managed deployments, to prevent Edge from automatically
upgrading out-of-date Worker Nodes.

The automatic upgrade process works like this: 

1. The Leader pulls packages and checks their hashes.

    * The Leader must (1) be able to connect to the path, and (2) have
      privileges to download the files.

    * If the path is a file path, the Leader copies the files to a known
      location in its filesystem.

2. Edge Nodes pull packages and check their hashes.

    * Edge Nodes pull from the leader through HTTP, not directly from the
      Leader's filesystem.

    * Each Edge Nodes pulls the package that is appropriate for that Node's
      platform and architecture. 

3. Edge Nodes install the packages.

### Upgrading Fleets via the UI {#upgrade-fleets}

To upgrade a Fleet, go to **Settings** > **Edge Settings** > **Fleet Upgrade**. Click any row's **Upgrade** button to upgrade that Fleet. The resulting
**Upgrade Fleet** modal offers two states: Basic Upgrade and Advanced Upgrade.
 
#### Basic Upgrade Configuration

In this default **Upgrade Fleet** modal, you can simply upgrade the whole
Fleet, by clicking the modal's **Upgrade** button to confirm. 

When one or more Edge Nodes can be upgraded in the Fleet, the **Upgrade** button will be enabled.

By clicking the **Upgrade** button, you can initiate the upgrade job with a task for each Edge Node that can be upgraded.

Cribl Edge will check to ensure that Edge Nodes are upgraded no higher than the
Leader's version. Upgrades are performed as the user that was running Edge on
each machine.

The **Nodes** column will display a warning icon with a tooltip if the Fleet contains Edge Nodes that can't be upgraded for one or more of the following reasons:

- Edge Nodes are already running the current version.
- (Linux) Nodes v.2.4.4 or older are too old to upgrade.
- (Windows) Nodes v.4.3.0 or older are too old to upgrade.

When none of the Edge Nodes in the Fleet can be upgraded, the **Upgrade** button will be disabled. 

#### Advanced Upgrade Configuration

Click the modal's gear (⚙️) button to expose these additional options:

**Quantity %**: Specify what percentage of the Fleet's Edge Nodes to upgrade in
this operation. If you enter a value less than the default `100`%, Edge will
perform a partial upgrade, keeping the remaining Edge Nodes active to process
data.

**Rolling upgrade**: This option upgrades Edge Nodes in batches, each sized
according to your **Quantity %** value. When enabled, this toggle also enables
the modal's two remaining controls: 

**Retry delay (ms)**: How many milliseconds to wait between upgrade attempts.
Defaults to `1000` ms (1 second).

**Retry count**: How many times to retry a failed upgrade. Defaults to `5`.

After you confirm the Fleet upgrade, the table on the Fleet Upgrade page will display an additional button on this Fleet's row:

* **View**: Click to display the upgrade task's status in the Job Inspector modal
– select that modal's **System** tab to access details.

> When you initiate an upgrade via the UI, the new package is untarred to
> `$CRIBL_HOME/unpack.<random‑hash>.tmp`. This location inherits the permissions
> you've already assigned to `$CRIBL_HOME`. 
>
{.box .info}

### Backup and Rollback {#back}

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
the Edge server fails to start, or if the Worker Node fails to connect to the
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


## Manual Upgrades and Rollbacks {#manual}

### Standalone/Single-Instance {#single}

This path requires upgrading only the single/standalone Node:

1. Stop Edge. 

2.  Download the package on your instance of choice [here](https://www.cribl.io/download).

3. Uncompress the new version on top of the old one, e.g., in the `/opt/cribl-edge` directory:   `tar xvzf cribl-<version>-<build>-<arch>.tgz`

   On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot
   open: File exists`. In this case, please remove the `cribl/bin/cribl`
   directory if it's empty, and untar again. If you have **custom functions** in
   `cribl/bin/cribl`, please move them under
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

4. Restart Edge.

> The current upgrade process creates a new Cribl Edge installation in `/opt/cribl` by default, requiring you to manually move it to your existing Cribl Edge directory (usually `/opt/cribl-edge`) and adjust permissions.  For a simpler upgrade, the new version should be extracted directly into your existing Cribl Edge directory, overwriting the old one.  
>
{.box .info}


###  Distributed Deployment  {#distributed}

For a distributed deployment, the general order of upgrade is: First upgrade the
Leader Node, then upgrade the Edge Nodes, then commit and deploy the changes on
the Leader.

> For distributed environments with a
> [second Leader](deploy-add-second-leader) configured, the order of upgrade is:
> First upgrade the Primary Leader Node, then upgrade the Secondary Leader Node,
> then upgrade the Edge Nodes. 
>
{.box .info}

#### Upgrading the Leader Node

1. Commit and deploy your desired last version. (This will be your most recent
   checkpoint.)

   - Optionally, `git push` to your configured remote repo.

2. Stop Cribl Edge. 

   - Optional but recommended: Back up the entire `$CRIBL_HOME` directory.

   - Optional: Check that the Edge Nodes are still functioning as expected. In
     the absence of the Leader Node, they should continue to work with their
     last deployed configurations. 

3. Download the package on your instance of choice [here](https://www.cribl.io/download).

4. Uncompress the new Edge version on top of the old one, e.g., in the `/opt/cribl-edge` directory: `tar xvzf cribl-<version>-<build>-<arch>.tgz`

5. Restart Edge and log back in. 

6. Wait for all the Edge Nodes to report to the Leader, and ensure that they are
   correctly reporting the last committed configuration version. 

> Cribl Edge's UI will not be available until the Edge version has been
> upgraded to match the version on the Leader. Errors will appear
> until the Edge Nodes are upgraded. 
>
{.box .info}

#### Upgrading the (Linux) Edge Nodes 

These are the same basic steps as when upgrading a [Single Instance](#single),
above:

1. Stop Cribl Edge on each Edge Node.

2. Download the package on your instance of choice
   [here](https://www.cribl.io/download). If the Leader is on a prior release,
   see [Cribl Past Releases](https://cribl.io/download/past-releases/) for older
   packages. 

3. Uncompress the new Edge version on top of the old one, e.g., in the `/opt/cribl-edge` directory: `tar xvzf cribl-<version>-<build>-<arch>.tgz`

4. Restart Edge.

#### Upgrading the (Windows) Edge Nodes {#win-upgrade}

To manage Windows Nodes from the Leader, follow these steps to upgrade to v.4.3.1. 

Before making these changes to your production Fleet, please test this upgrade on a staging environment.

Consider using existing out-of-band automation to perform the following steps: 

1. Stop Cribl Edge on each Edge Node.

2. Uninstall the existing Cribl Edge version:

- If you have the original MSI that you installed, you can run:
`msiexec /x cribl.xxx.msi`

- If not, you can get the `GUID` from:

`HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion > Uninstall`

And then run `msiexec /x {GUID}`

- If you prefer to use PowerShell to uninstall the MSI, run:

`$app = Get-WmiObject -Class Win32_Product -Filter "Name = 'Cribl Edge'"
$app.Uninstall()`

3. Install the new Cribl Edge version: 

`msiexec /i cribl-4.3.1-*-win32-x64.msi /qn /l*v C:\cribl-install.log KEEPDATA=1`

`KEEPDATA=1` persists data between installs. `KEEPDATA` will not overwrite the existing configurations, state, logs, etc.

When an older Edge Node is uninstalled, the product configurations are left behind under `C:\ProgramData\Cribl`, which the installer can reuse for the new Cribl Edge Node.

As soon as the new MSI is installed, it will automatically connect to the same Leader and join the same Fleet as before.

> The Leader can upgrade Windows Nodes by running Cribl Edge 4.3.0 or later under the `Local System` account, instead of under the `Administrator` account.
>
{.box .info}

### Commit and Deploy Changes from the Leader Node

1. Ensure that newly upgraded Edge Nodes report to the Leader with their new
   software version. 

2. Commit and deploy the newly updated configuration **only after all**
   Edge Nodes have upgraded.


### Manual Rollback {#rollback}

Using [CLI commands](cli-reference#command-syntax), it's possible to explicitly
roll back an on-prem Leader, and Nodes, to an earlier release. This works much
like an upgrade.

Explicit rollback might be necessary if an [automatic rollback](#back)
fails. Otherwise, Cribl recommends
first considering other options – an upgrade, or working with Cribl Support –
before a manual rollback. Rollback can encounter these complications:

- Your current configuration might take advantage of dependencies not supported
  in an earlier release.
- Do not manually roll back any Cribl Edge instance running in a container.
  Instead, locate, download, and launch a
  [container image](https://hub.docker.com/r/cribl/cribl/tags) hosting the
  earlier version you want.

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


## Upgrade Troubleshooting

- Cribl Edge does **not** support direct upgrades from any pre-GA version (such as a Cribl-provided test candidate) to a GA version. To get the GA version running, you must perform a new install. 

- If you’re upgrading to v.3.5.4 or later, all Edge Nodes will need to be on the same version as the Leader. Leaders running v.3.5.4 and later test whether Edge Nodes are running a compatible version before deploying configs that could break Nodes' data flow. The Leader will prompt you to upgrade these Nodes as needed..

- Version 3.5.4 is also a compatibility breakpoint for the Cribl HTTP [Source](sources-cribl-http) and [Destination](destinations-cribl-http), and for the Cribl TCP [Source](sources-cribl-tcp) and [Destination](destinations-cribl-tcp). When running on Cribl Edge 3.5.4 and later, these two Sources can send data only to Workers running v.3.5.4 and later, and these two Destinations can receive data only from Nodes running v.3.5.4 and later. When running on Edge 3.5.3 and earlier, these four integrations can similarly interoperate only with Nodes running v.3.5.3 and earlier.
