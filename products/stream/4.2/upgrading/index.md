# Upgrading


This page outlines how you can upgrade a Cribl Stream [single-instance](deploy-single-instance)
or [distributed deployment](deploy-distributed) along one of these
supported upgrade paths, which apply to UI-based upgrades:

| Current Version            | Upgrade Path                       |
|----------------------------|------------------------------------|
| 4.x                        | 4.x                                |
| 3.x                        | 3.x through 4.x                    |
| 2.x                        | 2.x through 4.x                    |
| 1.7.x or 2.0.x             | 2.x.x, then 3.x or 4.x             |
| 1.6.x or below             | 1.7.x, then 2.x.x, then 3.x or 4.x |

> If you're upgrading Worker Nodes directly from the command-line interface, you
> don't have to worry about a version upgrade path - there are no restrictions on
> the versions you can upgrade to or from.
>
{.box .info}

## Considerations {#upgrade-considerations}

- Cribl Stream does **not** support direct upgrades from any pre-GA version 
  (such as a Cribl-provided test candidate) to a GA version. To get the GA
  version running, you must perform a new install. 

- If you’re upgrading to v.3.5.4 or later, all Worker Nodes will need to be on the same version as the Leader. Leaders running v.3.5.4 and later test whether Worker Nodes are running a compatible version before deploying configs that could break Workers' data flow. The Leader will prompt you to upgrade these Nodes as needed.

- Version 3.5.4 was also a compatibility breakpoint for the Cribl HTTP [Source](sources-cribl-http) and [Destination](destinations-cribl-http), and for the Cribl TCP [Source](sources-cribl-tcp) and [Destination](destinations-cribl-tcp). When running on Cribl Stream 3.5.4 and later, these two Sources can send data only to Nodes running v.3.5.4 and later, and these two Destinations can receive data only from Nodes running v.3.5.4 and later. When running on Stream 3.5.3 and earlier, these four integrations can similarly interoperate only with Nodes running v.3.5.3 and earlier.

- Before upgrading your Leader to v.4.0 and later, see [Persisting Socket Connections](deploy-distributed#socket) 
  to prepare the host to keep communications open from Workers. 

- Cribl Stream 4.1 and later encrypt TLS certificate private key files when you
  add or modify them. See instructions just below for backing up your keys from
  earlier versions. 

## Safeguarding Unencrypted Private Key Files for Rollback {#safeguarding}

Before upgrading from a pre-4.1 version, make a backup copy of all unencrypted
TLS certificate private key files. Having access to the unencrypted files is
essential if you later find that you need to roll back to your previous version.

To safeguard your unencrypted private keys, make a full backup of all Cribl
config files. Files in the `auth/certs` directory are particularly important,
such as those in: 

- `groups/default/local/cribl/auth/certs/`
- `groups/<groupname>/local/cribl/auth/certs/`
- `cribl/local/cribl/auth/certs/`
- Etc.

Take appropriate precautions to prevent unauthorized access to these unencrypted
private key files. If you need to roll back to a pre-4.1 version, see 
[Restoring Unencrypted Private Keys](securing-and-monitoring#restoring).

## Standalone/Single-Instance {#single}

This path requires upgrading only the single/standalone node:

1. Stop Cribl Stream. 

2. Uncompress the new version on top of the old one.

   On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot
   open: File exists`. In this case, please remove the `cribl/bin/cribl`
   directory if it's empty, and untar again. If you have **custom functions** in
   `cribl/bin/cribl`, please move them under
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

3. Restart Cribl Stream.

##  Distributed Deployment  {#distributed}

For a distributed deployment, this is the general upgrade order: 

First, upgrade the Leader Node. Then, upgrade the Worker Nodes. Lastly, commit
and deploy the changes on the Leader.

> For distributed environments with a
> [standby Leader](deploy-add-second-leader) configured, this is the upgrade
> order:
> 
> First, upgrade the Primary Leader Node. Then, upgrade the Secondary Leader Node.
> Lastly, upgrade the Worker Nodes. 
>
{.box .info}

### Upgrade the Leader Node

1. Commit and deploy your desired previous version. (This will be your most
   recent checkpoint.)

   Optionally `git push` to your configured remote repo.

2. Stop Cribl Stream. 

   - Optional but recommended: Back up the entire `$CRIBL_HOME` directory.

   - Optional: Check that the Worker Nodes are still functioning as expected. In
     the absence of the Leader Node, they should continue to work with their
     last deployed configurations. 

3. Uncompress the new Cribl Stream version on top of the old one.

4. Change ownership of the `cribl` directory as needed: `[sudo] chown -R cribl:cribl $CRIBL_HOME`

5. Restart Cribl Stream and log back in. 

6. Wait for all the Worker Nodes to report to the Leader, and ensure that they
   are correctly reporting the last committed configuration version. 

> ##### Upgrading from 4.1.0
>
> When upgrading a Leader Node from version 4.1.0 using the UI, you may encounter
> the following error: 
> 
> `{"status":"error","message":"Protocol \"https:\" not supported. Expected \"http:\"","error":{"code":"ERR_INVALID_PROTOCOL"}}`
> 
> To resolve this, configure the `no_proxy` directive to include the API's
> listening address on the Leader Node. To encompass all addresses, set `no_proxy`
> to `0.0.0.0`. This prevents internal traffic on the Leader Node from passing
> through the proxy server.
> 
> Here's an example `No_Proxy` configuration:
> 
> `http_proxy=http://localhost:1234 https_proxy=http://localhost:1234 no_proxy=0.0.0.0 bin/cribl start`
> 
> This extra step is due to changes in communication between the connection
> listener service and the API server via an `http` client.
>
{.box .warning}



### Upgrade the Worker Nodes

These are the same basic steps as when upgrading a [Single Instance](#single),
above:

1. Stop Cribl Stream on each Worker Node.

2. Uncompress the new version on top of the old one.

3. Change ownership of the `cribl` directory as needed: `[sudo] chown -R cribl:cribl $CRIBL_HOME`

4. Restart Cribl Stream.

### Commit and Deploy All Changes from the Leader Node

1. Ensure that newly upgraded Worker Nodes report to the Leader with their
   **new** software version. 

2. Commit and deploy the newly updated Leader configuration after
   all Workers have upgraded.

![Successful upgrade confirmation](post-worker-upgrade.1539f48148.png)

> ##### Breaking Change at v.3.2 {{< id `3.2.x-ui` >}}
>
> Because of a breaking change at v.3.2, Leaders running v.3.2.x cannot upgrade (via the UI) Workers running versions prior to 3.2.0. The upgrade will fail with errors of the form: `Error checking upgrade path` and `Cannot read property 'greaterThan' of
> undefined`.
> 
> The workaround is to upgrade these Workers [via the filesystem](upgrading#distributed) to v.3.2.0 or higher. The error does not affect upgrades of Workers running v.3.2.0+. 
>
{.box .warning}

## Upgrade and Rollback via the UI {#ui}

Cribl Stream/LogStream v.2.4.4 and higher provides streamlined upgrading options
directly through the UI:

- To upgrade a single-instance deployment, select **Settings** > **System** > **Upgrade**. 
- To upgrade the Leader Node, select **Settings** > **Global Settings** > **System** > **Upgrade**. 
- To upgrade a single Worker Group, select **Settings** > **Stream Settings** > **Group Upgrade**. 

These streamlined controls perform the whole above sequence of stopping the
Cribl server, updating the installed package, and restarting Cribl.

Cribl Stream/LogStream 3.0 (and later) also enables you to manage automatic
[backup and rollback](#back) in case an upgrade fails.

> These options will work only if all Stream/LogStream instances
> (including Worker Processes) start at v.2.4.4 or higher. For other
> version-specific limitations, please see  [Known Issues](known-issues).
> 
> Be aware that the **Checking for upgrade** status message, and its accompanying
> spinner, can take up to several minutes to resolve. Also, after you initiate an
> upgrade, it can take up to several minutes before the **View** button (described
> below) is displayed. 
>
{.box .warning}

### Upgrade the Leader Node via the UI

To upgrade the Leader in a distributed deployment, go to **Settings** > **Global Settings** > **System** > **Upgrade**. The following controls are available:

#### Package Source {#package-source}

To the right of **Package source**, the default **CDN** button downloads a
package directly from Cribl's content delivery network. 

If you select the alternative **Path** button, next click **Add Path** (below
**Custom path**) to define as many paths as you need. In the resulting table,
each row provides these additional fields:

  - **Platform‑specific package location**: Enter either a URL (HTTP) or a local
    path to the upgrade package. HTTP example:
    `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz`

  - **Package hash location**: Enter either a URL (HTTP), or a local path to the
    hash that validates the package. Supports SHA‑256 and MD5 formats. You can
    simply append `.sha256` to the contents of the
    **Platform‑specific package location** field.  
  HTTP example: `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz.sha256`

  - **X** (close box): Click to delete a row – immediately. (There is no
    confirmation prompt.)

  - **Save**/**Cancel** buttons: Click **Save** to store the specified
    locations. Clicking **Cancel** restores the **CDN** package-source
    selection.

> You can add multiple rows to this table to specify packages for
> different platforms/architectures. To obtain the latest packages from
> https://cribl.io/download, use the drop-down list to specify each platform
> (e.g., x64 versus ARM). When you stage these packages on your own servers,
> preserve the original file names. 
>
{.box .success}

#### Automatic Upgrades {#automatic-upgrades}

**Enable automatic upgrades** has a different default state depending on the
deployment type:

- In on-prem/​customer-managed deployments, **Enable automatic upgrades**
  defaults to `No`, to prevent the Leader from automatically upgrading
  out-of-date Worker Nodes. Before you toggle this to `Yes`, upgrade the Leader
  itself to Cribl Stream's most recent version.

- In [Cribl.Cloud](deploy-cloud) deployments, **Enable automatic upgrades**
  defaults to `Yes`. This enables Cribl to automatically upgrade Worker Nodes to
  Cribl Stream's newest stable version. (Cribl-managed and hybrid Workers will
  auto-upgrade as soon as they see a new Leader version.) If you toggle this to
  `No`, you will need to explicitly upgrade each Worker.

When enabled, the automatic upgrade process works like this: 

1. The Leader pulls packages, and checks their hashes.

    * The Leader must be able to connect to the path.
    * The Leader must also have privileges to download the files.
    * If the path is an HTTP URL, the Leader copies the file to a known location
      in its filesystem.
    * If the package is already hosted on the Leader Node, specify its
      filesystem path.

2. Workers pull packages and check their hashes.

    * Workers pull from the Leader through HTTP, not directly from the Leader's
      filesystem.
    * Each Worker pulls the package that is appropriate for that Worker's
      platform and architecture. 

3. Workers install the packages.



### Upgrade Worker Groups via the UI {#ui-workers}

To upgrade a Worker Group, go to **Settings** > **Stream Settings** > **Group Upgrade**. Click any row's **Upgrade** button to upgrade that group. The resulting
**Upgrade Group** modal offers two states: Basic Upgrade and Advanced Upgrade.

> Upgrading Workers from the Leader requires a Cribl Stream Standard or
> Enterprise [license](licensing). 
>
{.box .info}

#### Basic Upgrade Configuration

In this default **Upgrade Group** modal, you can simply upgrade the whole
Group, by clicking the modal's **Upgrade** button to confirm. 

Cribl Stream will check to ensure that Workers are upgraded no higher than the
Leader's version. Upgrades are performed as the user that was running
Cribl Stream on each machine.

#### Advanced Upgrade Configuration

Click the modal's gear (⚙️) button to expose these additional options:

**Quantity %**: Specify what percentage of the Group's Workers to upgrade in
this operation. If you enter a value less than the default `100`%, Cribl Stream
will perform a partial upgrade, keeping the remaining Workers active to process
data.

**Rolling upgrade**: This option upgrades Workers in batches, each sized
according to the value in the **Quantity %** field. When enabled, this slider also enables the modal's two remaining controls: 

- **Retry delay (ms)**: How many milliseconds to wait between upgrade attempts.
  Defaults to `1000` ms (1 second).

- **Retry count**: How many times to retry a failed upgrade. Defaults to `5`.

After you confirm the Worker Group upgrade, the table on the Group Upgrade page will display an additional button on this Group's row:

* **View**: Click to display the upgrade task's status in the Job Inspector modal
– select that modal's **System** tab to access details.

> When you initiate an upgrade via the UI, the new package is untarred to
> `$CRIBL_HOME/unpack.<random‑hash>.tmp`. This location inherits the permissions
> you've already assigned to `$CRIBL_HOME`. 
>
{.box .info}



### Backup and Rollback {#back}

When you initiate an upgrade through the UI, Cribl Stream first stores a backup
of your current stable deployment. If the upgrade fails, then by default, Stream
will automatically roll back to the stored backup package. You can adjust this
behavior at **Settings** > **Global Settings** > **System** >**General Settings** > **Upgrade & Share Settings**, 
using the following controls. 

> Cribl Stream can perform rollbacks only on Worker Nodes/instances
> that were running at least v.3.0.0 before the attempted upgrade. 
>
{.box .warning}

**Enable automatic rollback**: Cribl Stream will automatically roll back an
upgrade if the Cribl Stream server fails to start, or if the Worker Node fails
to connect to the Leader. (Toggle to `No` to defeat this behavior.)

**Rollback timeout (ms)**: Time to wait, after an upgrade, before checking each
Node's health to determine whether to roll back. Defaults to `30000`
milliseconds, i.e., 30 seconds.

**Rollback condition retries**: Number of times to retry the health check before
performing a rollback. Defaults to `5` attempts.

**Check interval (ms)**: Time to wait between health-check retries. Defaults to
`1000` milliseconds, i.e., 1 second.

**Backups directory**: Specify where to store backups. Defaults to
`$CRIBL_HOME/state/backups`.

**Backup persistence**: A relative time expression specifying how long to keep
backups after each upgrade. Defaults to `24h`.

## Manual Rollback {#rollback}

Using [CLI commands](cli-reference#command-syntax), it's possible to explicitly
roll back an on-prem Leader, and on-prem or hybrid Workers, to an earlier
release. This works much like an upgrade.

Explicit rollback might be necessary if an [automatic rollback](#back)
fails. Otherwise, Cribl recommends
first considering other options – an upgrade, or working with Cribl Support –
before a manual rollback. Rollback can encounter these complications:

- Your current configuration might take advantage of dependencies not supported
  in an earlier release.
- Do not manually roll back any Cribl Stream instance running in a container.
  Instead, locate, download, and launch a
  [container image](https://hub.docker.com/r/cribl/cribl/tags) hosting the
  earlier version you want.

### Rollback Outline {#rollback-outline}

We assume that you are rolling back to a previously deployed version that you
know to be stable in your environment. The broad steps are:

1. Ideally, [link](/stream/version-control#remote-setup) your deployment to a
   Git remote repo.
   [Commit and push](/stream/version-control#pushing-to-a-remote-repo) your
   Leader's configuration to that remote. The repo will provide a stable
   location from which to recover your config, if necessary.

2. Stop the Leader instance. (From `$CRIBL_HOME/bin/`, execute `./cribl stop`.)

3. Also create a local backup of your Leader's whole `$CRIBL_HOME` directory.

4. Obtain the installation package for the earlier release and platform you
   need. (See the [Rollback Example](#rollback-example).)

5. [Uncompress](deploy-single-instance#install) the earlier version to your
   original deployed directory. You can do this from the command line or
   programmatically (see the [Rollback Example](#rollback-example)).

  If installing to your existing target directory fails, try the same
  mitigations we list [above](#single) for upgrades.

6. In a [distributed deployment](#distributed), Workers must not run a higher
   version than the Leader. Repeat steps 2, 3, and 5 above on all Workers
   (substituting “Worker” for “Leader”).

### Rollback Example {#rollback-example}

Although rollback can be partially scripted, you cannot discover older installation packages programmatically – the initial steps here require human intervention:

1. Stop and back up the Leader. (Follow steps 1–3 in the [Rollback Outline](#rollback-outline).)

1. Open the **Cribl Releases** download page: https://cribl.io/download/.

1. In the **Cribl Past Releases** section, locate the older version that you want to restore.

1. Select your target platform from the adjacent drop-down menu.

1. Click the corresponding download button. The browser downloads the installation package for your target platform and saves it directly to your designated download location. The download URL for installation packages is similar to this example for v.4.1.0: `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-arm64.tgz`

1. Navigate to your designated download location to locate the downloaded installation package.

   - For `.tgz` files, double-click the installation package to extract the contents or run a command like `tar -zxvf <fileName>.tgz`

   - For `.msi` files, double-click the installation package to launch the installer

1. Repeat the preceding steps to adjust all Worker Nodes to a compatible version.



## Splunk App Package Upgrade Steps

> See [Deprecation note](deploy-splunkapp) for v.2.1.
>
{.box .warning}

Follow these steps to upgrade from v.1.7, or higher, of the Cribl App for Splunk:

1. Stop Splunk.

2. Untar/unzip the new app version on top of the old one.

   On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot open: File
   exists`. In this case, please remove the `cribl/bin/cribl` directory if it's empty, and
   untar again. If you have **custom functions** in `cribl/bin/cribl`, please move them under
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

3. Restart Splunk.

### Upgrading from Splunk App v.1.6 (or Lower)

As of v.1.7, contrary to prior versions, Cribl's Splunk App package defaults to Search Head
Mode. If you have v.1.6 or earlier deployed as a Heavy Forwarder app, upgrading requires an
extra step to restore this setting:

1. Stop Splunk.

2. Untar/unzip the new app version on top of the old one.

3. Convert to HF mode by running: `$SPLUNK_HOME/etc/apps/cribl/bin/cribld mode-hwf` 

4. Restart Splunk.
