# Upgrade


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

> If you're upgrading Worker Nodes directly from the command line, you don't have to worry about a version upgrade path. There are no restrictions on the versions you can upgrade from or to.
>
{.box .info}

## Considerations {#upgrade-considerations}

Here are some considerations to look at before you upgrade.

### Manual Upgrade Required for Hybrid and On-Prem Deployments

Whether your upgrade is automatic or manual depends on your deployment type:

- **For Cribl.Cloud:** Cribl Stream automatically upgrades the Leader Node in Cribl.Cloud deployments.
- **For customer-managed (hybrid) Cribl.Cloud Worker Groups, and all on-prem deployments:** Requires manual upgrades for Worker Groups and distributed instances.

Maintaining compatible versions across all Cribl Stream deployments—whether in the cloud, hybrid, or on-premise—is crucial for optimal performance and stability.

### Supported Version Differences Between Leader and Workers

Cribl strongly recommends keeping Leader Nodes and Worker Groups/instances on the same version for the best experience and to ensure access to all features.
Upgrading a Leader Node to a newer version may surface configuration options in the UI
that will not be functional until you upgrade the connected Workers to the same version.

However, this section outlines the supported version differences between Leader Nodes and Worker Groups (or distributed instances) across all deployment types—including Cribl.Cloud, hybrid, and on-premise. Following these guidelines is essential to ensure functionality, compatibility, and full support coverage:

- **Patch Versions**: Cribl supports Leaders and Worker Groups or distributed instances on different patch versions within the same minor version. For example, a Worker Group on 4.5.0 and Leader on 4.5.1.
- **Minor Versions**: Cribl supports Worker Groups or distributed instances that are one minor version behind the Leader Node. For example, a Worker Group on 4.5.1 and a Leader is on 4.6.0.

Do not upgrade Workers to a newer version than the Leader Node.

> Cribl does not support version differences greater than one minor version, which may lead to unpredictable behavior and compatibility issues. Support requests for configurations exceeding this limit will require updating to compatible versions.
>
{.box .warning}

### Requirements for Specific Version Upgrades

- **Upgrading from a pre-GA candidate:** Cribl Stream does not support direct upgrades from any pre-GA version (such as a Cribl-provided test candidate) to a GA version. To get the GA version running, you must perform a new install.

- **Upgrading your Leader to 4.0 and newer:** Before upgrading, see [Persisting Socket Connections](persisting-socket-connections) to prepare the host to keep communications open from Worker Nodes.

- **Upgrading to 3.5.4 or newer:** All Worker Nodes will need to be on the same version as the Leader. Leaders running v.3.5.4 and newer test whether Worker Nodes are running a compatible version before deploying configs that could break Worker Nodes' data flow. The Leader will prompt you to upgrade these Nodes as needed.

  > Version 3.5.4 was also a compatibility breakpoint for the Cribl HTTP [Source](sources-cribl-http) and [Destination](destinations-cribl-http), and for the Cribl TCP [Source](sources-cribl-tcp) and [Destination](destinations-cribl-tcp). When running on Cribl Stream 3.5.4 and newer, these two Sources can send data only to Nodes running v.3.5.4 and newer, and these two Destinations can receive data only from Nodes running v.3.5.4 and newer. When running on Cribl Stream 3.5.3 and older, these four integrations can similarly interoperate only with Nodes running v.3.5.3 and older.
  >
  {.box .info}

- **Upgrade to v.4.3.x using Leader-Managed Upgrades for Worker Nodes:** If a Leader is on Cribl Stream v.4.3.x, you should upgrade all Worker Nodes to v.4.3.x **before** making any config changes or committing and deploying to the Nodes. If you have already committed and deployed to Nodes that are on a version older than 4.3.0, and the Leader is on v.4.3.x, you won't be able to upgrade the Nodes. In this case, there are three workaround options:

  - Revert and redeploy the last commit, then upgrade the Worker Nodes and deploy the most up-to-date changes; **or**
  - Upgrade the Worker Nodes via command line or script; **or**
  - Downgrade the Leader to v.4.2.2, and wait for a future release that will enable you to upgrade Worker Nodes across all Leaders and 4.x.x versions.

- **Upgrading from v.4.1 and older:** Before upgrading, encrypt TLS certificate private key files when you add or modify them. See the next section about [Safeguard Unencrypted Private Key Files for Rollback](#safeguarding) for information about backing up your keys from older versions.

### Safeguard Unencrypted Private Key Files for Rollback {#safeguarding}

Before upgrading from a version older than 4.1, make a backup copy of all unencrypted
TLS certificate private key files. Having access to the unencrypted files is
essential if you later find that you need to roll back to your previous version.

To safeguard your unencrypted private keys, make a full backup of all Cribl
config files. Files in the `auth/certs` directory are particularly important,
such as those in:

- `groups/default/local/cribl/auth/certs/`
- `groups/<groupname>/local/cribl/auth/certs/`
- `cribl/local/cribl/auth/certs/`

Take appropriate precautions to prevent unauthorized access to these unencrypted
private key files. If you need to roll back to a version older than 4.1, see
[Restore Unencrypted Private Keys](securing-tls#restoring).

## Network Connectivity Requirements

For a smooth upgrade process, Cribl Workers require reliable and timely network access to specific resources during the upgrade. Check the following:

- **Preferred Connectivity:** Workers attempt to download upgrade packages from the Cribl content delivery network (CDN) for efficient updates. A stable and responsive connection to `cdn.cribl.io` is a must for successful upgrades.
- **Alternative Connectivity (if CDN access is restricted):** If accessing `cdn.cribl.io` is not possible due to security restrictions or other reasons, Workers can fall back to downloading the upgrade package directly from the Leader.

### Network Performance Considerations

When accessing the CDN at `cdn.cribl.io`, a slow or unreliable network connection will cause the upgrade to fail due to significant delays. The upgrade package size can be around 80 MB.

If you are having problems downloading the package from the CDN, check the following:

- **Ensure adequate network bandwidth and stability.** Test your network connection for reliability before upgrading.
- **Prevent RPC timeouts.** Cribl Stream versions 4.11.1 and newer have a higher RPC timeout threshold. Upgrade your Worker Nodes to v.4.11.1 or newer to help prevent future RPC timeouts.
- **Ensure access to the CDN (to download packages) is not blocked by firewall for upgrades.** See the following section about [Considerations for Firewalls/Web Proxies](#firewalls) for more information.

### Considerations for Firewalls/Web Proxies {#firewalls}

For optimal upgrade performance, configure firewalls to allow access to `cdn.cribl.io`:

- If your organization restricts access to `cdn.cribl.io`, configure firewalls to immediately deny connection requests. This allows Workers to promptly fall back to the Leader for downloading the upgrade package and avoid timeouts.
- You may encounter the "unsafe legacy renegotiation disabled" error when the Cribl Stream Leader attempts to connect to the CDN, preventing Worker Node upgrades. This error occurs if your firewall or proxy (like PAN firewalls) doesn't support the [TLS Renegotiation Indication Extension (RFC 5746)](https://datatracker.ietf.org/doc/html/rfc5746).
- Ensure your firewall or proxy is compliant with RFC 5746. Contact your firewall/proxy vendor for update instructions.

### Disable SNI Routing for Firewalls/Web Proxies {#sni}

If you're experiencing issues with proxies or firewalls between your Cribl Stream Worker Nodes and Leader Node, you can disable Server Name Indication (SNI) routing for specific Groups.

To disable SNI routing for a Group: Navigate to **Worker Group Settings** > **General Settings** > **SNI** > **Disable SNI Routing** option. **Commit** and **Deploy** and then proceed with the upgrade. You must enable this setting for every Group that is behind a web proxy or firewall.

## Upgrade a Standalone/Single-Instance {#single}

To upgrade a single-instance deployment from the UI, select **Settings** > **System** > **Upgrade**.

To upgrade a single-instance deployment using the command line:

1. Stop Cribl Stream.

2. Uncompress the new version on top of the old one.

   On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot
   open: File exists`. In this case, please remove the `cribl/bin/cribl`
   directory if it's empty, and untar again. If you have **custom functions** in
   `cribl/bin/cribl`, please move them under
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

3. Restart Cribl Stream.

## Upgrade a Distributed Deployment  {#distributed}

You can upgrade Leader and Worker Nodes with the [command line interface](#cli) (CLI) or the [user interface](#ui) (UI).

### Upgrade and Automatic Rollback via the UI {#ui}

When you choose to upgrade via the UI, Cribl Stream stops its own server, updates the installed package, and restarts Stream for you.

> Upgrading through the UI is not supported for distributed environments with an [additional Leader](deploy-add-second-leader) configured for high availability/failover. Instead, [use the command line interface (CLI) to upgrade](#cli).
>
{.box .warning}

Here's how to access the Upgrade UI in Stream:

1. First, select **Settings** in the sidebar.
1. Select **Upgrade** depending on your deployment type:

    - In a single-instance deployment, select **System**, then **Upgrade**.
    - To upgrade the Leader Node, select **Global**, then **System** and **Upgrade**.
    - To upgrade a single Worker Group, select **Stream** then **Group Upgrade**.

You can also enable an automatic [backup and rollback](#backup-and-rollback) in case an upgrade
fails.

> These options will work only if all Stream instances
> (including Worker Processes) start at v.2.4.4 or higher. For other
> version-specific limitations, please see  [Known Issues](known-issues).
>
{.box .warning}

#### Upgrade the Leader via the UI

To upgrade the Leader in a distributed deployment, go to **Settings** > **Global** > **System** > **Upgrade**.

When upgrading, you can choose to either download upgrade packages from Cribl's
content delivery network (CDN), or from a filesystem location that you specify
in the form of a path.

##### Select a Package Source

Here, you'll select between **CDN** and **Path** for your upgrade (in an
on-prem, distributed deployment):

|Package Source|Description|When You Select This Package Source|
|------------|-----------|-----------|
|<div style="width: 150px">CDN</div>  |Downloads an installation package directly from Cribl's content delivery network.|With **CDN** selected, you can see the currently installed version and the version available on the CDN. The Leader will always upgrade to the current CDN version when you select **Upgrade**.|
|Path        |You designate the path to the installation package in the **Path** field.|You must designate the correct paths for your Leader and Worker Groups, both version and architecture. For example, a Worker Group that contains ARM64 Worker Nodes will require an ARM64 installation package.|

Use the buttons to choose the desired package source. Then, the UI will display
settings and information appropriate for the package source you chose, as
described in the respective [Configure Stream Settings for CDN Upgrade](#configure-stream-settings-for-cdn-upgrade) and
[Configure Stream Settings for Path Upgrade](#configure-stream-settings-for-path-upgrade) sections below.

##### Configure Stream Settings for CDN Upgrade

![Upgrade Stream using CDN](stream-upgrade-4-6.png)
{scale="60%" border="true"}

If you select **CDN** as your upgrade method, you will see the following options:

- **Current CDN version**: This lists the latest version of Cribl Stream available on the CDN.
- **Leader**: This shows the currently installed version of Cribl Stream on the Leader. If a newer version
  is available, you will be able to use the **Upgrade to** button.
- **Stream Worker Groups**: Provides a quick link to the Group Upgrade page
  if you want to upgrade Worker Groups individually.

  > In Cribl.Cloud, this area will only show hybrid Worker Nodes.
  {.box .cloud}

  To automatically upgrade Worker Nodes to the Leader version, enable **Upgrade Workers automatically**.
  See [Automatic Upgrades](#automatic-upgrades) for more details.

##### Configure Stream Settings for Path Upgrade

![Upgrade Stream using Path](stream-upgrade-path-4-6.png)
{scale="60%" border="true"}

With **Path** as your upgrade method, you'll see the following settings and
information in the **Package Source** table:

- **Platform-Specific Package Location**: Enter or paste the path to the Cribl
  installation package. This can be either of the following:

    * An HTTP URL, for example `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz`.
    * A local filesystem path, for example `myfolder/directory/cribl-package.tgz`. In a distributed deployment, the Leader must have access to the local filesystem location. The Leader retrieves the file and distributes config bundles to Worker Nodes and Edge Nodes.

- **Package Hash Location**: Enter either of the following:

    * An HTTP URL, for example `https://cdn.cribl.io/dl/4.1.0/cribl-4.1.0-6979aea9-linux-x64.tgz.sha256`.
    * A local filesystem path to the hash that validates the package.

    Supports SHA‑256 and MD5 formats. You can simply append `.sha256` to the
    contents of the **Platform‑specific package location** field.

  >When Stream is running in [FIPS mode](fips-mode), MD5 is not an acceptable hashing algorithm.
  {.box .info}

- **Leader**: Indicates the version the Leader will be upgraded to when an
  upgrade is available and you select the **Upgrade to** button.
- **Stream**: The version in this row will be used to upgrade the Stream Worker Group,
  because it matches the Leader's version.
- **Edge**: The version in this row will be used to [upgrade Edge Fleets](/edge/upgrading-ui-nodes/)
  with a matching target version.
- **Version**: Displays the package version in this row.
- **Platform**: Displays the platform architecture for the package in this row.

Select **X** to immediately delete a row – there is no confirmation prompt.

> You can add multiple rows to this table to specify packages for different
> versions and platforms/architectures. To obtain the latest packages from
> https://cribl.io/download, use the drop-down list to specify each platform
> (for example, x64 versus ARM). When you stage these packages on your own servers,
> preserve the original file names.
>
{.box .success}

**Leader** area: Shows you the currently installed Leader version and whether an
upgrade is available. Check the [backup and rollback](#backup-and-rollback)
settings before you upgrade.

**Stream Worker Groups**: Provides a quick link to the Group Upgrade page
if you want to upgrade Worker Groups individually.

To automatically upgrade Worker Nodes to the Leader version, enable [**Upgrade Workers automatically**](#automatic-upgrades).

The **Edge Fleets** area offers **Upgrade Options** for Edge. See
[Configure Edge Settings for Path Upgrade](/edge/upgrading-ui-leader/#configure-edge-settings-for-path-upgrade)
for more information.

##### Missing Package Links

When you see this warning, it means there are Worker Groups that can't be
upgraded due to a missing package path.

To add package links:

1. Select the **Copy** icon to add the link to your clipboard.
1. Paste the copied package link into a new **Platform-specific package location** field.
1. Add the appropriate path directory to the beginning of the installation
   package path (where your Cribl upgrade packages are located).

   For example: `myfolder/directory/cribl-package.tgz`

1. Select **Save** to add the upgrade package path and resolve the warning.

##### Automatic Upgrades

You can enable **Upgrade Workers automatically** in **Settings** > **Global** to check
for Worker Nodes running an earlier version than the Leader, and automatically upgrade
them to match the Leader.

**Enable automatic upgrades** has a different default state depending on the
deployment type:

- In on-prem deployments, toggle **Enable automatic upgrades** off (default) to prevent the Leader from automatically upgrading out-of-date Worker Nodes. Before toggling on, upgrade the Leader itself to Cribl Stream's most recent version.

- In [Cribl.Cloud](deploy-cloud) deployments, toggle **Enable automatic upgrades** on. This enables Cribl to automatically upgrade Worker Nodes to
  Cribl Stream's newest stable version. (Cribl-managed/Cribl.Cloud and customer-managed/hybrid Worker Nodes will
  auto-upgrade as soon as they see a new Leader version.) If you toggle **Enable automatic upgrades** off, you will need to explicitly upgrade each Worker.

When enabled, the automatic Worker upgrade process works like this:

1. The Leader pulls packages, and checks their hashes.

    * The Leader must be able to connect to the path.
    * The Leader must also have privileges to download the files.
    * If the path is an HTTP URL, the Leader copies the file to a known location
      in its filesystem.
    * If the package is already hosted on the Leader Node, specify its
      filesystem path.

2. Worker Nodes pull packages and check their hashes.

    * Worker Nodes pull from Cribl’s content delivery network (CDN) and fall back to the Leader through HTTP, not directly from the Leader's
      filesystem.
    * Each Worker pulls the package that is appropriate for that Worker's
      platform and architecture.

3. Worker Nodes install the packages.

#### Upgrade Worker Groups via the UI {#ui-workers}

To upgrade a Worker Group:

1. Go to **Settings**, then **Stream**.
1. Select **Group Upgrade**.
1. Select the **Upgrade** button in a row to upgrade that  Worker Group.

The **Upgrade Group** modal offers two states: Basic Upgrade and
Advanced Upgrade.

> Upgrading Worker Nodes from the Leader Node requires a Cribl Stream Standard or
> Enterprise [license](licensing).
>
{.box .info}

##### Basic Upgrade Configuration

In this default **Upgrade Group** modal, you can simply upgrade the whole
Group, by selecting the modal's **Upgrade** button to confirm.

When one or more Worker Nodes can be upgraded in the Group, the **Upgrade** button will be enabled.

By selecting the **Upgrade** button, you can initiate the upgrade job with a task for each Worker that can be upgraded.

Cribl Stream will check to ensure that Worker Nodes are upgraded no higher than the
Leader's version. Upgrades are performed as the user that was running
Cribl Stream on each machine.

The **Worker Nodes** column will display a warning icon with a tooltip if the Group contains Worker Nodes that can't be upgraded for one or more of the following reasons:

- Worker Nodes are already running the current version.
- Worker Nodes running v.2.4.4 or older are too old to upgrade.

When none of the Worker Nodes in the Group can be upgraded, the **Upgrade** button will be disabled.

##### Advanced Upgrade Configuration

Select the modal's gear (⚙️) button to expose these additional options:

**Quantity %**: Specify what percentage of the Group's Worker Nodes to upgrade in
this operation. If you enter a value less than the default `100`%, Cribl Stream
will perform a partial upgrade, keeping the remaining Worker Nodes active to process
data.

**Rolling upgrade**: This option upgrades Worker Nodes in batches, each sized
according to the value in the **Quantity %** field. When enabled, this toggle also enables the modal's two remaining controls:

- **Retry delay (ms)**: How many milliseconds to wait between upgrade attempts.
  Defaults to `1000` ms (1 second).

- **Retry count**: How many times to retry a failed upgrade. Defaults to `5`.

After you confirm the Worker Group upgrade, the table on the Group Upgrade page will display an additional button on this Group's row:

* **View**: Select to display the upgrade task's status in the Job Inspector modal
– select that modal's **System** tab to access details.

> When you initiate an upgrade via the UI, the new package is untarred to
> `$CRIBL_HOME/unpack.<random‑hash>.tmp`. This location inherits the permissions
> you've already assigned to `$CRIBL_HOME`.
>
{.box .info}

#### Backup and Rollback

When you initiate an upgrade through the UI, Cribl Stream first stores a backup
of your current stable deployment. If the upgrade fails, then by default, Stream
will automatically roll back to the stored backup package. You can adjust this
behavior in **Settings**.

Go to **Settings** > **Global**, then **System** and **General Settings**, then **Upgrade & Share Settings**.

Use the following controls to adjust backup and rollback behavior.

> Cribl Stream can perform rollbacks only on Worker Nodes/instances
> that were running at least v.3.0.0 before the attempted upgrade.
>
{.box .warning}

**Enable automatic rollback**: Cribl Stream will automatically roll back an
upgrade if the Cribl Stream server fails to start, or if the Worker Node fails
to connect to the Leader. (Toggle off to defeat this behavior.)

**Rollback timeout (ms)**: Time to wait, after an upgrade, before checking each
Node's health to determine whether to roll back. Defaults to `30000`
milliseconds (30 seconds).

**Rollback condition retries**: Number of times to retry the health check before
performing a rollback. Defaults to `5` attempts.

**Check interval (ms)**: Time to wait between health-check retries. Defaults to
`1000` milliseconds (1 second).

**Backups directory**: Specify where to store backups. Defaults to
`$CRIBL_HOME/state/backups`.

**Backup persistence**: A relative time expression specifying how long to keep
backups after each upgrade. Defaults to `24h`.

### Upgrade and Rollback via the CLI {#cli}

For a Distributed Deployment, this is the general upgrade order:

First, upgrade the Leader Node. Then, upgrade the Worker Nodes. Lastly, commit and deploy the changes on the Leader.

> For distributed environments with a [second (standby) Leader](deploy-add-second-leader) configured for high availability/failover, this is the upgrade order:
>
> 1. Stop the standby Leader for your [Stream](/stream/upgrading/) or [Edge](/edge/upgrading/) deployment.
> 1. Upgrade the standby Leader.
> 1. Start the standby Leader.
> 1. Stop the primary Leader. The standby will take over as primary.
> 1. Upgrade the former primary Leader.
> 1. Start the former primary Leader, which will become the new standby Leader.
>
{.box .info}

#### Upgrade the Leader Node via the CLI

1. Commit and deploy your desired previous version. (This will be your most
   recent checkpoint.)

   Optionally `git push` to your configured remote repo.

2. [Stop](cli-stop) Cribl Stream.

   - Optional but recommended: Back up the entire `$CRIBL_HOME` directory.

   - Optional: Check that the Worker Nodes are still functioning as expected. In
     the absence of the Leader Node, they should continue to work with their
     last deployed configurations.

3. Uncompress the new Cribl Stream version on top of the old one.

4. Change ownership of the `cribl` directory as needed: `[sudo] chown -R cribl:cribl $CRIBL_HOME`

5. Restart Cribl Stream and log back in.

6. Wait for all the Worker Nodes to report to the Leader, and ensure that they
   are correctly reporting the last committed configuration version.

> ##### Upgrade from 4.1.0
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

#### Upgrade the Worker Nodes via the CLI

These are the same basic steps as when upgrading a [single instance deployment](#single),
above:

1. [Stop](cli-stop) Cribl Stream on each Worker Node.

2. Uncompress the new version on top of the old one.

3. Change ownership of the `cribl` directory as needed: `[sudo] chown -R cribl:cribl $CRIBL_HOME`

4. Restart Cribl Stream.

#### Commit and Deploy All Changes from the Leader Node

1. Ensure that newly upgraded Worker Nodes report to the Leader with their
   **new** software version.

2. Commit and deploy the newly updated Leader configuration after all Worker Nodes have upgraded.

![Successful upgrade confirmation](post-worker-upgrade.50e60340f0.png)
{border="true"}

> ##### Breaking Change at v.3.2 {{< id `3.2.x-ui` >}}
>
> Because of a breaking change at v.3.2, Leaders running v.3.2.x cannot upgrade (via the UI) Worker Nodes running versions older than 3.2.0. The upgrade will fail with errors of the form: `Error checking upgrade path` and `Cannot read property 'greaterThan' of
> undefined`.
>
> The workaround is to upgrade these Worker Nodes [via the filesystem](upgrading#distributed) to v.3.2.0 or higher. The error does not affect upgrades of Worker Nodes running v.3.2.0+.
>
{.box .warning}

#### Manual Rollback {#rollback}

Using [CLI commands](cli-reference), it's possible to explicitly
roll back an on-prem Leader, and on-prem or hybrid Worker Nodes, to an earlier
release. This works much like an upgrade.

Explicit rollback might be necessary if an [automatic rollback](#backup-and-rollback)
fails. Otherwise, Cribl recommends
first considering other options – an upgrade, or working with Cribl Support –
before a manual rollback. Rollback can encounter these complications:

- Your current configuration might take advantage of dependencies not supported
  in an earlier release.
- Do not manually roll back any Cribl Stream instance running in a container.
  Instead, locate, download, and launch a
  [container image](https://hub.docker.com/r/cribl/cribl/tags) hosting the
  earlier version you want.

##### Rollback Outline {#rollback-outline}

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
   need (see the [Rollback Example](#rollback-example)).

5. [Uncompress](deploy-single-instance#install) the earlier version to your
   original deployed directory. You can do this from the command line or
   programmatically (see the [Rollback Example](#rollback-example)).

   If installing to your existing target directory fails, try the same
   mitigations we list [above](#single) for upgrades.

6. In a [distributed deployment](#distributed), Worker Nodes must not run a higher
   version than the Leader. Repeat steps 2, 3, and 5 above on all Worker Nodes
   (substituting “Worker” for “Leader”).

##### Rollback Example {#rollback-example}

Although rollback can be partially scripted, you cannot discover older installation packages
programmatically – the initial steps here require human intervention:

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

Follow these steps to upgrade from v.1.7, or newer, of the Cribl App for Splunk:

1. Stop Splunk.

2. Untar/unzip the new app version on top of the old one.

   On some Linux systems, `tar` might complain with: `cribl/bin/cribl: Cannot open: File
   exists`. In this case, please remove the `cribl/bin/cribl` directory if it's empty, and
   untar again. If you have **custom functions** in `cribl/bin/cribl`, please move them under
   `$CRIBL_HOME/local/cribl/functions/` before untarring again.

3. Restart Splunk.

### Upgrade from Splunk App v.1.6 (or Lower)

As of v.1.7, contrary to prior versions, Cribl's Splunk App package defaults to Search Head
Mode. If you have v.1.6 or earlier deployed as a Heavy Forwarder app, upgrading requires an
extra step to restore this setting:

1. Stop Splunk.

2. Untar/unzip the new app version on top of the old one.

3. Convert to HF mode by running: `$SPLUNK_HOME/etc/apps/cribl/bin/cribld mode-hwf`

4. Restart Splunk.
