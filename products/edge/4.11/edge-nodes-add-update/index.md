# Add and Update Edge Nodes


Learn how to add and update Edge Nodes in Cribl Edge

---

Cribl Edge provides you with the ability to add and update Edge Nodes from the
UI. To access this function, select **Edge Nodes** in the sidebar. On the
**Edge Nodes** page, select **Add/Update Edge Node**.

![Options to add or update an Edge Node](edge-node-add-update.png)
{border="true" scale="75%"}

Here, you can update an existing Node with new configurations or add a new Node
using the bootstrap script.

You can generate a bootstrap script for Edge Nodes deployed in the following
environments:

- Docker: For details, see [Running in a Docker Container](deploy-running-docker).
- Kubernetes: For details, see [Deploying via Kubernetes](deploy-running-kubernetes).
- Linux: For details, see [Installing Cribl Edge on Linux](deploy-single-instance).
- Windows: For details, see [Installing Cribl Edge on Windows](deploy-windows#add-or-update-a-windows-node).

> The **Update** option in the UI adjusts the Leader connection details
> for the currently installed software. To upgrade the Edge Node – that is,
> install a newer version of the software – see [Upgrading](upgrading). 
>
{.box .info}

## Add a New Edge Node with a Bootstrap Script {#bootstrap}

Using the Cribl Edge UI, you can add Edge Nodes to your existing deployment.
Simply change the environment-specific fields, then copy and paste the resulting
bootstrap script into a CLI. The script updates in real-time as you change the
fields. The result is a new Edge Node configured with the options you chose.

### Host Communication Prerequisites
To enable the Leader to manage the Edge Nodes, you must enable ongoing outbound
communication to the Leader's port 4200 on all Edge Nodes hosts.

While the bootstrap script runs, firewalls on each Nodes's host must also allow
outbound communication on the following ports:

- Port 443 to `https://cdn.cribl.io`.
- Port 443 to a Cribl.Cloud Leader.
- Port 9000 to an on-prem Leader.

### How To Add an Edge Node
> The details below differ slightly depending on which deployment option you
> select. This tutorial assumes a **Linux deployment**.
>
{.box .info} 

1. From the Cribl Edge sidebar, select **Edge Nodes**.

1. On the **Edge Nodes** page, select **Add/Update Edge Node**.

1. Fill and modify the deployment option fields:

    - The **Install package location** defaults to `Cribl CDN`. If desired,
      change this to `Download URL`. 
    - As needed, correct the target **Fleet**, as well as the **Leader hostname/IP**
     (URI).
    - As needed, correct the **User** and **User Group** to run Cribl as.
      Defaults to `cribl`.
    - As needed, correct the **Installation directory**. Defaults to
      `/opt/cribl`. 

1. Optionally, add **Tags** that you can use for filtering and grouping in
   Cribl Edge. Use a tab or hard return between (arbitrary) tag names.

1. Copy the resulting script to your clipboard.

1. Click either **Done** or **X** to close the modal.

1. To add the Edge Node, paste the script onto the command line of your Edge
   Node and execute it.

If your script successfully added the Edge Node, you will see it in the
**Edge Nodes** list.

#### Select the Update Script Type for Linux Nodes

When you update a Linux Node, you must select a **Script type** from the
drop-down. Choose between environment variable or CLI to add/update an Edge
Node.  

With **Environment variable** selected:

1. Hover over the **Script** field and select the copy icon. 
1. To allow Edge to update or add Edge Nodes, configure the environment variable
  with a restart command.

  - You can add the restart command to the environment variable in the Linux
    systemd service unit file, so you don't have to insert the environment
    variable each time you restart the server:
  
      1. In the CLI, enter `systemctl edit cribl-edge` on the Edge Node.
      1. Add `ENVIRONMENT="CRIBL_DIST_LEADER_URL=tcp://criblmaster@masterHostname:4200"`
     to the opened file and save it.
      1. Reload the service with `sudo systemctl restart cribl-edge`.
  
  - Or, you can use use `./cribl restart`, but you will have to insert the
    environment variable each time you restart the server:

    ```
    CRIBL_DIST_LEADER_URL=tcp://criblmaster@masterHostname:4200 ./cribl restart
    ```

With **CLI** selected:

1. Hover over the **Script** field and select the copy
  icon. 
1. Paste the copied script into a CLI and run it. 

### Concatenate the Bootstrap Script

Cribl Edge admins can use the UI to concatenate and copy/paste the bootstrap
script, combining several of the steps above. Instead of manually editing the fields
in the UI, you can change them in the script. You can use an adjacent option to
grab a script that updates an Edge Node's Fleet assignment.

### Edge Node Updates Using Fleet Version Settings {#fleets-target}

The script you generate using the **Add/Update Edge Node** window ensures accurate 
updates by using the specified [**Target Software Version**](upgrading-ui-nodes#upgrading-edge-nodes)
for your Fleet, enhancing the reliability and consistency of updates across all
supported environments: Docker, Kubernetes, Linux, and Windows. 

For Docker and Kubernetes:

- If you don’t specify a target version, the command uses the Leader version’s
  image and version flag respectively, by default.

- If you specify a target version, the command parses the Fleet version and maps
  it to the appropriate CDN URL or version flag.

For Linux environments:

- The existing script behavior from the Leader manages the updates unless you
  specify a target version, in which case the script uses that version.

For Windows: 

- The default behavior uses the Leader Version from the CDN, while specifying a
  target version directs the command to parse the Fleet version and map it for
  MSI download via the CDN. 

This mechanism ensures a more consistent and reliable deployment process,
maintaining version control and improving update accuracy across all
environments.

### Troubleshooting the Display {#trouble}

If you see unexpected results in the **Edge Nodes** list, keep in mind that:

- Edge Nodes that don't maintain their connection to the Leader will eventually disappear from the Edge Nodes page.
- For a newly created Fleet, the **Config Version** column can show an
  indefinitely spinning progress spinner for that Fleet. This happens because
  the Edge Nodes are polling for a config bundle that hasn't been deployed.
  To resolve this, click the **Deploy** option to force a deploy.
- For details on collocating multiple Cribl products, see [Cribl Edge and Cribl Stream on the Same Host](deploy-single-instance#collocated).
- For details on overriding default ports, see [Overriding Default Ports](deploy-running-docker#ports-override).