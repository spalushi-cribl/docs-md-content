# Manual Rollback


Learn how to rollback back an upgrade manually using a command line interface (CLI).

---

## Manual Rollback {#rollback}

Using [CLI commands](cli-reference), it's possible to explicitly roll back an
on-prem Leader, and Nodes, to an earlier release. This works much like an
upgrade.

Explicit rollback might be necessary if an [automatic rollback](upgrading-ui-rollbacks#backup-and-rollback)
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

### Rollback Outline {#rollback-outline}

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

5. [Uncompress](deploy-linux#install) the earlier version to your
   original deployed directory. You can do this from the command line or
   programmatically (see the [Rollback Example](#rollback-example)).

  If installing to your existing target directory fails, try the same
  mitigations we list [above](#single) for upgrades.

6. In a [distributed deployment](#distributed), Nodes must not run a higher
   version than the Leader. Repeat steps 2, 3, and 5 above on all Nodes,
   substituting “Node” for “Leader.” (On Nodes where you've integrated
   Cribl Edge with systemd, stop the Cribl server with: `sudo systemctl stop
   cribl-edge`.)

#### Rollback Example {#rollback-example}

While rollbacks can be partially scripted, discovering earlier installation
packages cannot be automated – the initial steps here are manual:

1. Stop and back up the Leader. (Follow steps 1–3 in the [Rollback Outline](#rollback-outline).)

1. Open the **Cribl Releases** download page: https://cribl.io/download/.

1. In the **Cribl Past Releases** section, locate the older version that you want to restore.

1. Use the adjacent drop-down to select your target platform.

1. Right-click (or `Ctrl`-click) the corresponding **Download** button, and copy
   its URL to your clipboard. In this example, we've copied:
`https://cdn.cribl.io/dl/4.13.0/cribl-4.13.0-46ac1714-linux-x64.tgz`

1. Swap that URL into an untar command like the following. Run this command from
   the parent (typically `/opt/`) of your existing `$CRIBL_HOME` directory:

   ```shell
   sudo curl -Lso - https://cdn.cribl.io/dl/4.13.0/cribl-4.13.0-46ac1714-linux-x64.tgz | tar zxv
   ```

1. Repeat the preceding steps to adjust all Nodes to a compatible version.
