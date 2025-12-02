# Install Cribl Edge on MacOS


Install and deploy Cribl Edge Nodes on-prem on macOS.

---

> ##### Preview Feature {#preview}
>
> This Preview feature is still being developed. We do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance, and related documentation could be incomplete.
>
> Please continue to submit feedback through normal Cribl [support channels](/support/),
> but assistance might be limited while the feature remains in Preview.
> 
{.box .info}

You can install Cribl Edge in a supported macOS environment to collect logs and track local files.

## Supported MacOS Versions {#supported-versions}

Cribl Edge supports the following macOS versions:

- 26.x (Tahoe)
- 15.x (Sequoia)
- 14.x (Sonoma).

You can install Cribl Edge using the same universal binary on both Intel and Apple silicon processors.

## Before You Install Cribl Edge on MacOS

1. Set a compatible browser as your default browser: Microsoft Edge, Firefox, Safari, or Chrome.
   (We support the five most-recent versions of these browsers.
   Cribl Edge is not compatible with Internet Explorer.)
1. Ensure that the required ports are available (see [Ports](ports)).
1. Go to the Cribl [Download page](https://www.cribl.io/download) and set the **Software** drop-down to `Cribl Edge for macOS`.
1. Select **Download now** to get the `.pkg` installer.

You can also concatenate and copy/paste the [bootstrap script](edge-nodes-add-update#bootstrap) in your command prompt to add macOS Nodes.

## Install Cribl Edge

From the command line, you can install  and configure Cribl Edge
to act either as a Single-instance deployment, or as a managed Node.

1. To install Cribl Edge from the package you downloaded, enter the following at your command prompt: 

   ```shell
   sudo installer -pkg <package-file>.pkg -target /
   ```

   This will install Cribl Edge to `/opt/cribl/`. 

   > To prevent issues with Cribl file operations, `/opt/cribl` and all its subdirectories must reside on the same device.
   > Mounting separate devices within `/opt/cribl` is not recommended.
   > For external storage needs, such as lookups or persistent queues (PQs),
   > create a completely separate directory outside of `/opt/cribl`.
   {.box .warning}

1. Set `/opt/cribl` as your `$CRIBL_HOME` directory.

   > To make the [`$CRIBL_HOME`](environment-variables#cribl_home) environment variable available on your command line, you can:
   > 
   > - Assign it once, using the `export` command: `export CRIBL_HOME=/opt/cribl`
   > - Set it as a default, by adding it to your to your terminal profile file.  
   >
   {.box .info}

1. Set the installation mode to `mode-managed-edge` to connect the Node to the designated Leader:

   ```shell
   sudo /opt/cribl/bin/cribl mode-managed-edge /
      -H <leader-hostname-or-IP> /
      -p <port> /
      -u <token> /
     [-g <fleet>]
   ```
   
   - `<token>` must match the Leader token, to ensure a connection with the Leader.
   Get the Leader token from **Settings** > **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Auth token**.

   - The new Node will be assigned to a [Fleet](fleet-management) on the Leader, either `default_fleet`,
   or a different one according to [Fleet mappings](mapping-edge-nodes).

   - You can manually point to a specific Fleet by providing its name as `-g <fleet>`,
   but Fleet mappings will override this choice.

   > If you only need a Single-instance deployment of Cribl Edge (without a Leader),
   > do not change the mode. Cribl Edge will run in `mode-edge` automatically.
   {.box .info}

1. Restart the Cribl service:

   ```shell
   sudo /opt/cribl/bin/cribl restart
   ```

1. You can now log in to your Leader and locate the newly installed Cribl Edge Node
   and start configuring [Sources](sources) and [Destinations](destinations), or creating [Routes](routes) and [Pipelines](pipelines).
   
   > If you installed a single instance of Cribl Edge without a Leader,
   > go to `http://localhost:9420` (or another port you defined) and log in with default credentials (`admin:admin`).
   {.box .info}


> For more ways to configure Leader and Edge Nodes, see [Set Up Leader and Edge Nodes](setting-up-leader-and-edge-nodes).
{.box .info}
