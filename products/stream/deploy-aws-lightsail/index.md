# AWS Deployment via Lightsail


This page outlines how to deploy a Cribl Stream Master Node, and one Worker Node, to two AWS instances using AWS' Lightsail cloud configuration manager.

## Install and Start the Cribl Stream Server

1. Log into AWS, and navigate to the [Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/instances) service to set up instances.

1. For a Master Node and one Worker Node, click **Create Instance** to create instances with these settings: <br/>
  • **Linux/Unix** 
  • **OS Only** 
  • **Amazon Linux 2** (default CentOS), or your choice of Linux OS <br/>
  For production, you'd size these as 4+ GB (2 cores) each. But if you're simply evaluating Cribl Stream, you can use the **$3.50/One Month Free** tier.

1. Under **Identify your instance**, change the default naming scheme (`Amazon_Linux-`) to a unique primitive. E.g., `GoatStreams-` or whatever naming scheme you like. (Lightsail will append the digits.)

1. Use the spin box to request `2` instances. Create them in your default Region, and wait a few seconds for them to spin up.

1. Click into the first instance, which will host your Cribl Stream Master Node. Click its **Connect using SSH** button to open an SSH session in a new browser tab.

1. In the SSH terminal, execute the following commands. (This particular example references the LogStream 2.3.3 installer, but substitute the current version's URL, which you can get from [Cribl's download page](https://cribl.io/download/).)

```shell
sudo yum install git

cd /tmp; wget -c https://cdn.cribl.io/dl/2.3.3/cribl-2.3.3-fc2c6ae8-linux-x64.tgz

sudo tar zxf /tmp/cribl*tgz -C /opt/

sudo useradd cribl; sudo chown -R cribl: /opt/cribl

sudo /opt/cribl/bin/cribl boot-start enable -u cribl

sudo systemctl start cribl
```



7. In the Lightsail UI, click **Home**, then click into the #2 Worker instance.

> You might find it easier to duplicate browser tabs, to keep both the Cribl Stream #1 and #2 instances open for further configuration without having to traverse through the **Home** link.
>
{.box .success}

8. Click the 2nd instance's **Connect using SSH** button. In its console, execute these commands – identical to the Master's, except that here we omit the initial `git` installation:

```shell
cd /tmp; wget -c https://cdn.cribl.io/dl/2.3.2/cribl-2.3.2-cb3fcf2e-linux-x64.tgz

sudo tar zxf /tmp/cribl*tgz -C /opt/

sudo useradd cribl; sudo chown -R cribl: /opt/cribl

sudo /opt/cribl/bin/cribl boot-start enable -u cribl

sudo systemctl start cribl
```



You now have a Cribl Stream Master Node and Worker Node running. Next, configure the AWS instances to enable the Nodes to communicate. with each other.

## Open Ports and Launch the Cribl Stream UI

We'll start by opening port `9000` on the Worker Node's instance, then do the same on the Master's instance.

1. In Lightsail's UI, select the second instance's **Networking** tab. In its **Firewall** section, click **Add Rule**.

1. Define a **Custom** app to listen on port `9000`, without restrictions.

1. Configure the same **Custom**/port `9000` app on the first instance.

1. Optionally, repeat the previous two step to add rules enabling both instances to listen on port `8000`, Splunk's port.

1. Copy instance #1's **Public IP** address to the clipboard.

1. Paste it into a new browser tab, then append the `:9000` port.

1. Log into the Cribl Stream UI with `admin`, `admin`. Register Cribl Stream, and ignore its password-change prompts.

1. Return to the Lightsail UI, and copy instance #2's **Public IP** address to the clipboard.

1. Open another browser tab. Paste in instance #2's **Internet IP** address, and again append the `:9000` port. 

1. Log into this second Cribl Stream UI with `admin`, `admin`. Again register Cribl Stream, and ignore password-change prompts.

## Configure the two Cribl Stream Nodes as Master and Worker

1. In the Master instance's UI, set the **Distributed Settings > General Settings > Mode** to `Master`. Also, under **Distributed Settings > Git Settings > General**, enter a **Default Commit Message**.

1. In the Master instance's UI, optionally enable **Distributed Settings > Master Settings > Worker UI access**. Also, you *might* want to unlock Enterprise features by pasting in the NFR license. **Save** everything, and let the Master instance restart.

1. In the Lightsail UI, go to instance #1 (Master) and copy its **Private IP** address.

1. In the Worker instance's UI, set the **Distributed Settings > General Settings > Mode** to `Worker`.

1. Paste the Master's **Private IP** address into the Worker's **Distributed Settings > Master Settings > Address** field. **Save** everything, and let the Worker restart.

1. In the Master's UI, navigate to the `default` Worker Group. Change its **System Settings > Worker Processes** from default `-2` to `-1`. **Save**.

## Commit and Deploy the Configuration

1. In the Master's UI, click the **Workers** tab. Notice that the `default` Group has no **Config Version** yet. To remedy this:

1. Commit the configuration changes.

1. Click the **Default Group** tab, then click the **Deploy default** [Group] button. Confirm and Save.

1. On the **Workers** tab, verify that **Config Version** is now populated, and that the **Status** column now shows the Worker Node shutting down and restarting.



## Clean Up

1. To close an SSH tab, just type `exit` at its command prompt.

2. Lightsail charges you for infrastructure, not usage. To destroy instances you no longer need, go to Lightsail's **Instances** tab, select each instance, and select **Delete** from its context menu.
