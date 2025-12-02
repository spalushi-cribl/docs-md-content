# Getting Started with Cribl Edge


This quick start guide shows you how to get Cribl Edge running.

You can either start with a free Cribl.Cloud instance,
or deploy the Cribl Edge agent on a physical or virtual machine.
We’ll cover both options below, so feel free to jump to the relevant section in this article:

- [Download and Install](#install) Cribl Edge.
- [Quick Start with a Cloud Instance](#cloud).

## Check the Minimum Deployment Requirements {#requirements}

Before you proceed, make sure that the hosts for all Cribl Edge agents – and for any Leader that you install on your own infrastructure – meet the [System Requirements](deploy-planning#requirements) that we list for Linux and Windows.

## Download and Install Cribl Edge {#install}

To install Cribl Edge, select the Linux or Windows tab below and follow the corresponding steps.

{{% tabs "linux" "linux" "Linux" "windows" "Windows" %}}
{{% tab-item "linux" %}}

> To avoid permissions errors, install and run Edge as the same Linux user.
> For details on creating a new user, see [Enabling Start on Boot](deploy-boot-start).
{.box .info}

1. Navigate to the [Cribl Downloads](https://cribl.io/download/) page.
1. Select either `x64` or `ARM` Cribl Suite from the drop-down.
   You can also copy and paste one of the provided `curl` scripts into a command-line interface.
1. Open a command-line interface and navigate to the `/opt/` directory:
   
   ```
   cd /opt/
   ```

1. Un-tar the resulting `.tgz` file in the `/opt/` directory.

   ```
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   ```

   For example:

   ```
   tar xvzf cribl-4.3.1-12f82b6a-linux-x64.tgz
   ```

   Edge is now installed in a `cribl` subdirectory, by default: `/opt/cribl/`.

1. Move it to the `cribl-edge` directory with:

   ```
   mv /opt/cribl/ /opt/cribl-edge
   ```

1. In your terminal, go to `/opt/cribl-edge/bin` and switch your installation to Edge mode:

   ```
   ./cribl mode-edge
   ```

{{% /tab-item %}}
{{% tab-item "windows" %}}

1. From Cribl's [Download page](https://www.cribl.io/download), set the **Software** drop-down to `Cribl Edge for Windows`.  
1. Click **Download Now** to get the `.msi` file.
1. Enter the following at your command prompt for a silent install:  

  ```shell
  msiexec /i cribl-<version>-<build>-<arch>.msi /qn 
   ```

> You can also install Cribl Edge from the `.msi` file using the [Setup Wizard](deploy-windows#ui).
{.box .info}

{{% /tab-item %}}
{{% /tabs %}}

### Run Cribl Edge {#running} 

Now you can start, stop, and verify the Cribl Edge server using these `./cribl` CLI commands:

- **Start**: `./cribl start`
- **Stop**: `./cribl stop`
- **Get status**: `./cribl status`

> For other available commands, see [CLI Reference](cli-reference).
{.box .success}

1. In your browser, open `http://127.0.0.1:9420` and log in with default credentials (`admin`, `admin`). 

1. Register your copy of Cribl Edge when prompted.

   After registering, you'll be prompted to change the default password.

## Quick Start with a Cloud Instance {#cloud}

As mentioned above, the Cribl Edge Leader Node can reside in a free Cribl.Cloud account.
You’ll get a ready-to-use Cribl Leader that includes Cribl Edge functionality. For a detailed comparison, see [Cribl.Cloud vs. Self-Hosted](cloud-vs-self-hosted).

To get a Cribl.Cloud account:

1. Go to [https://cribl.cloud/signup/](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink) and register an account.
1. On the **Create Organization** page, optionally enter an **Organization Name** (a friendly alias for the randomly generated ID that Cribl will assign to your Organization).
1. Select an AWS Region to host your Cribl.Cloud Leader. Cribl currently supports the following Regions:
   - `US West (Oregon)`
   - `US East (Virginia)`
   - `US East (Ohio)`
   - `Europe (Frankfurt)`
   - `Europe (London)`
   - `Asia Pacific (Sydney)`
   - `Canada (Central)`

> For details on Cribl.Cloud Organizations and on managing users, refer to Cribl.Cloud documentation:
> 
> - [Registering a Cribl.Cloud Portal](cloud-initial-setup#register).
> - [Invite Members](members#invite-members).
{.box .success} 

## Add Edge Nodes

You now have a Cribl Leader running with Cribl Edge capabilities enabled,
but still need to add Edge Nodes. To do this:

1. From Cribl Edges’s top nav, select **Manage** > **Edge Nodes**.
1. On the resulting **Edge Nodes** tab, click **Add/Update Edge Node** at the upper right and then select **Linux** > **Add**
1. Copy the resulting script to your clipboard and close the modal.
1. Paste the script onto your Edge Node’s command line and execute it.

> To add an Edge Node on a Windows or using Docker or Kubernetes, refer to [Add and Update Edge Nodes](edge-nodes-add-update#bootstrap).
{.box .success}

## What's Next?

Now that you have Cribl Edge running, you can try out the following resources:

- Follow a [tutorial](getting-started-tutorial) to set up your first Sources, and learn how to forward data to Destinations.
- Explore Cribl Edge on [Linux](explore-edge) or [Windows](explore-edge-win) to learn more about the various capabilities of Cribl Edge by taking a tour through its UI.
