# Distributed Quick Start


This tutorial builds on our [Getting Started Guide](getting-started-guide) by walking you through a distributed deployment – Cribl Stream's typical deployment type for production.

> For concepts and deeper details underlying the techniques presented here, see [Distributed Deployment](deploy-distributed).
>
{.box .success}

## Requirements

To exercise these distributed features, you'll need the following prerequisites.

### Licenses and Instances

This tutorial is tiered to accommodate different Cribl Stream [license types](licensing):

- With a Cribl Stream Free, One, or Standard license, you'll install three Cribl Stream instances on one or multiple physical or virtual machines – one Leader Node, and two Worker Nodes.

- To do the **optional** section on adding and managing [multiple Worker Groups](#groups), you'll need an Enterprise or Sales Trial license/plan.

### Basic Setup 

Basic building blocks are identical to the [Getting Started Guide](getting-started-guide). Please refer to the following sections of that tutorial for details, as needed:

- [(System) Requirements](getting-started-guide#requirements)
- [Download and Install Cribl Stream](getting-started-guide#install)
- [Run Cribl Stream](getting-started-guide#running)

To set up the multiple instances you'll need, choose among the options below in [Three Instances, Pick Any Medium](#media). But first, let's lay out the division of labor in a single Worker Group.

##  Distributed Deployment (Identical Workers) {#distributed}

A distributed deployment enables Cribl Stream to scale out to handle higher data volumes, load-balancing with failover, and parallel data processing based on conditional mapping and routing. 

A single Leader Node manages multiple Worker Nodes. The Leader distributes and updates configuration on the Workers, handles version control, and monitors the Workers' health and activity metrics. The Workers do all the data processing.

Here, we'll show how this works by configuring a Leader and two Workers. This configuration is compact, and can be demonstrated without an Enterprise (or other paid) license/plan. But you can extrapolate the same technique to setting up enterprise-scale deployments of hundreds of Workers, to handle petabytes of data.

> Cribl Stream Free, One, and Standard licenses support only a single Worker Group (named `default`), so in this example, all Workers share identical configuration. With an Enterprise license/plan, you can organize Workers into multiple Groups, with varying configurations to handle scenarios like on-prem versus cloud tech stacks, or data centers in different geographic locations. For details, see [Worker Groups – What Are They and Why You Should Care](https://cribl.io/blog/worker-groups-what-are-they-and-why-you-should-care/).
>
{.box .success}



###  Three Instances, Pick Any Medium {#media}

You'll need to deploy three Cribl Stream instances – with SSH access – on one or more physical machines, virtual machines, or containers. If you haven't already provisioned this infrastructure, you have several alternatives, listed in the following subsections:

- [Docker Containers](#docker)
- [Curl](#curl)
- [Amazon Lightsail](#lightsail)
- [AWS/EC2 (CloudFormation Optional)](#ec2)
- [Kubernetes/Helm](#k8s)

Pick whichever approach will make it easiest for you to get the infrastructure launched – based on familiarity, preference, or availability.

> ##### We Don't Need No Stinkin' Permissions Errors! {{< id `user` >}}
>
> Cribl's Docker containers come with Cribl Stream preinstalled. If you select any other option, be sure to both install and run Cribl Stream as the same Linux user. (For details on creating a new user – addressing both systemd and initd distro's – see [Enabling Start on Boot](deploy-single-instance#boot-start).)
>
{.box .info}

#### Docker Containers {#docker}

You can use the Docker Compose (`docker-compose.yml`) file below to easily stand up a Cribl Stream distributed deployment of a Leader and multiple Workers on one machine. 

> **Before you use the Docker Compose file**
> 
> 1. Open the `docker-compose.yml` file in a text editor.
> 1. Replace both instances of `INSERT_TOKEN` with a unique, [secure token value](securing-auth-token).
> 1. Save the updated `docker-compose.yml` file.
>
{.box .warning}

``` {title="docker-compose.yml"}
version: '3.8'
services:
  leader:
    image: cribl/cribl:latest
    environment:
      - CRIBL_DIST_MODE=leader
      - CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@0.0.0.0:4200
      - CRIBL_VOLUME_DIR=/opt/cribl/config-volume
    ports:
      - "19000:9000"
    volumes:
      - "~/cribl-config:/opt/cribl/config-volume"
  workers:
    image: cribl/cribl:latest
    depends_on: 
      - leader
    environment:
      - CRIBL_DIST_MODE=worker
      - CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@leader:4200
    ports:
      - 9000
```

This uses a local directory, `~/cribl-config`, as the configuration store for Cribl Stream. You must create this directory (it can be empty) before you run the `docker‑compose` command. 

If you prefer to use ephemeral storage, you can delete line 8 (the `CRIBL_VOLUME_DIR` definition) and lines 11–12 (the `volumes` configuration) before running the `docker‑compose` command. But this will make it hard to stop and restart the same infrastructure, if you want to do the tutorial in chunks.

To deploy a Leader Node, plus (e.g.) two Workers already configured and wired up to the Leader, use this command:

```shell
docker-compose up -d --scale workers=2
```

To deploy a different number of Workers, just change the `workers=2` value. By default, the above command pulls the freshest stable image (tagged `cribl/cribl:latest`) from [Cribl's Docker Hub](https://hub.docker.com/r/cribl/cribl/tags). It defaults to the following URLs and ports:

- Leader URL: `http://localhost:19000`
- Worker URLs: `http://localhost:<automatically-assigned-host-ports>`

If you're running the container itself on a virtual machine, replace `localhost` with the VM's IP address. The automatic assignment of available host-OS ports to the Workers prevents port collisions. **Within** the Docker container, these ports will forward over TCP to port 9000. To see the ports assigned on the OS, enter:

`docker ps`

You should see results like these:

```shell
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                         NAMES
a3de9ea8f46f   cribl/cribl:latest   "/sbin/entrypoint.sh…"   12 seconds ago   Up 10 seconds   0.0.0.0:63411->9000/tcp                       docker_workers_1
40aa687baefc   cribl/cribl:latest   "/sbin/entrypoint.sh…"   12 seconds ago   Up 10 seconds   0.0.0.0:63410->9000/tcp                       docker_workers_2
df362a65f7d1   cribl/cribl:latest   "/sbin/entrypoint.sh…"   13 seconds ago   Up 11 seconds   0.0.0.0:19000->9000/tcp, :::19000->9000/tcp   docker_master_1
```

The **PORTS** column shows the host-OS ports on the left, forwarding to the container-internal ports on the right. You can use the `docker_workers_N` ports if you want to log directly into Workers. In the above example:

- Worker1 URL: `http://localhost:63411` 
- Worker2 URL: `http://localhost:63410`

If your Leader is crashing with two Workers, make sure you are allocating enough memory to Docker.



> Once your three instances are running, proceed to [Configure Leader Instance](#leader).
>
{.box .success}

#### Curl {#curl}

Use our [Download](https://cribl.io/download/) page's `curl` command to directly install Cribl Stream onto your chosen infrastructure. 

For x64 processors, use: `curl -Lso - $(curl https://cdn.cribl.io/dl/latest-x64) | tar zxv`

For ARM64 processors:
`curl -Lso - $(curl https://cdn.cribl.io/dl/latest-arm64) | tar zxv`

Once you've [configured](#leader) the first Cribl Stream instance as a Leader, you can [bootstrap Workers from the Leader](deploy-workers). Or you can create Workers (with tags) from the Leader, using a `curl` command of this form:

`curl 'http://<leader-ip-or-hostname>:9000/init/install-worker.sh?token=criblmaster&tag=<tag1>&tag=<tag2>'`

E.g.: `curl 'http://localhost:9000/init/install-worker.sh?token=criblmaster&tag=dev&tag=test''`

> Once your three instances are running, proceed to [Configure Leader Instance](#leader).
>
{.box .success}

#### Amazon Lightsail {#lightsail}

Amazon Lightsail provides a quick, simple way to deploy Cribl Stream to AWS instances. Amazon's [Get Started with Linux/Unix-based Instances in Amazon Lightsail](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/getting-started-with-amazon-lightsail) tutorial walks you through setting up your instances and connecting via SSH.

- The free (first month) tier is all you need for this tutorial.
- As the "platform," Cribl recommends selecting **Amazon Linux 2 (default CentOS)**.
- Lightsail doesn't support IAM roles assigned to instances, or advanced load balancing, but it's adequate for this tutorial, which is not a production deployment.

> Once your three instances are running, proceed to [Configure Leader Instance](#leader).
>
{.box .success}

#### AWS/EC2 (CloudFormation Optional) {#ec2}

You can deploy Cribl Stream's AWS EC2 instances using Cribl's CloudFormation template, see our [AWS/EC2 Quick Start Guide](https://github.com/amiracle/quick-start-cribl) on GitHub. Follow the **Cribl Stream Distributed** instructions. (You'll be responsible for the costs of your AWS infrastructure.)

If you prefer to deploy your own EC2 instances, the free tier is fine for this tutorial. Cribl recommends selecting **Amazon Linux 2 (default CentOS)** AMIs. Relevant instructions are linked below.

1. See any of these AWS Docs for EC2 deployment:

   - [Tutorial: Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
   - [Launching an Instance Using the Old Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)
   - [How Can I Create and Connect to an Amazon Linux EC2 Instance?](https://aws.amazon.com/premiumsupport/knowledge-center/create-linux-instance/)

2. See these AWS instructions for SSH access:

   - [Connect to Your Linux Instance Using an SSH Client](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html#AccessingInstancesLinuxSSHClient)

> Once your three instances are running, proceed to [Configure Leader Instance](#leader).
>
{.box .success}

#### Kubernetes/Helm {#k8s}

Use Cribl's Helm charts to deploy Kubernetes pods, then proceed to the next section: 

- [Kubernetes Leader Deployment](deploy-kubernetes-leader)
- [Kubernetes Worker Deployment](deploy-kubernetes)



### Configure Leader Instance {#leader}

Once you've set up your Leader and Worker instances via your chosen approach above, you're ready to configure these instances and their communication.

> If you used an installation option like [Docker Containers](#docker) above, it has already preconfigured all three instances for you. This section and the next [Configure Workers](#workers) section will show you how to verify, and/or modify, these preset configurations. 
> 
> If you want to jump ahead, your next required configuration step is [Add a Source](#source), further down.
>
{.box .success}

Configure the first instance in Leader mode, and open port 4200.

1. Log into the Leader. Depending on the [deployment method](#media) you chose, this will be at `http://<localhost‑or‑IP‑address>:9000` or `http://<localhost‑or‑IP‑address>:19000`. Use the default `admin`/`admin` credentials.

2. Complete the registration form.

3. From the UI's **Settings** > **Global Settings** > **System** > **Distributed Settings** > **General Settings**, select **Mode**: `Leader`.

4. Click the **Leader Settings** left tab, and make sure the **Port** is set to `4200`.  
   (This port must be open for Workers to communicate with the Leader.)

5. Click **Save** to restart the Cribl server in Leader mode.

  Keep the Leader's tab open, so that we can verify connectivity with the Workers after configuring them (in the next section).

![Distributed > Leader Settings – the whole enchilada](leader-settings.eb34f1c547.png)
{border="true"}

6. Change the value of **Auth token** from the default to a [unique, secure value](securing-auth-token). Do this **before** adding any Workers.

7. {{< id `worker-ui-access` >}} Optional but recommended: From Cribl Stream's top nav, click **Manage** > **Groups**. Then enable the **UI access** toggle for your Group(s). This way, you will be able to click through from the Leader's **Manage** > **Workers** tab to view and manage each Worker's UI, distinguished by a [purple border](#teleport-border). (This option handles authentication for you, so you don't need to manage or enter the Workers' credentials.)



### Configure Workers {#workers}

Next, configure the two other instances as Workers that report to the Leader to receive processing instructions. We'll configure one instance through the UI, and (optionally) bootstrap the other from the Leader.

#### Configure Worker via UI {#worker-ui}

1. Log into the first Worker instance. Depending on the [deployment method](#media) you chose, this will be at `http://<localhost-or-IP-address>:9000` or at: 
`http://<localhost-or-IP-address>:<automatically-assigned-host-port>`. 
Use the default `admin`/`admin` credentials.

1. Complete the registration form, if displayed.

1. From the UI's **Settings** > **Global Settings** > **Distributed Settings** > **Distributed Management** > **General Settings**, select **Mode**: `Stream: Managed Worker (managed by Leader)`.

1. Leave other settings on this tab unchanged, including **Default Group**: `default`.  
   (This is the literal name of the single Worker Group available with free licenses.)

1. On the **Leader Settings** tab, make sure the **Address** matches the domain of the Leader you [previously configured](#leader). 

1. On the same **Leader Settings** tab, display the **Auth token** value.  
   (Optionally, you can change this from the default `criblmaster`.)

1. Leave other settings unchanged, and click **Save** to restart this instance as a managed Worker.

#### Bootstrap Worker from Leader {#bootstrap}

You can (if you choose) configure the second Worker instance using exactly the same procedure you used just above. But here, we'll offer you a simplified version of Cribl Stream's [Bootstrapping Workers from Leader](deploy-workers#chain) procedure for downloading the config from the Leader to the second Worker:

First, switch to the terminal/console of the instance you've reserved for this second Worker.

Next, if you didn't change the default **Auth token** value when you [previously configured](#leader) the Leader, run this command as root user:

```shell
curl http://<leader-hostname-or-IP>:9000/init/install-worker.sh | sh -
``` 

If you cannot run as root, insert `sudo` as you pipe to the shell:

```shell
curl http://<leader-hostname-or-IP>:9000/init/install-worker.sh | sudo sh -
``` 

To instead pipe to a bash shell:

```shell
curl http://<leader-hostname-or-IP>:9000/init/install-worker.sh | [sudo] bash -
``` 

If you substituted a custom **Auth token** value on the Leader, enter:

```shell
curl http://<leader-hostname-or-IP>:9000/init/install-worker.sh?token=<your-custom-token> | [sudo] sh -
```

Or, for bash:

```shell
curl http://<leader-hostname-or-IP>:9000/init/install-worker.sh?token=<your-custom-token> | [sudo] bash -
```

> The bootstrap script will install Cribl Stream into `/opt/cribl` on the target instance.
>
{.box .success}

#### Verify Workers' Communication with the Leader

With both Workers configured, next make sure they're visible to the Leader.

1. Switch back to Cribl Stream's UI on the Leader instance you [previously configured](#leader).

2. From Cribl Stream's top nav, click **Manage**. 

3. Below that, click the **Workers** tab. 

4. On the resulting **Manage Workers** page, you should now see both Workers, mapped to the `default` **Group**.

If one or both Workers are missing, repeat the preceding [Configure Worker via UI](#worker-ui) and/or [Bootstrap Worker from Leader](#bootstrap) procedures until the Workers show up.

Otherwise, if both Workers are present, we can now configure a few more resources to get data flowing through this distributed setup.

> If you're interested in details about the communication among Cribl Stream instances, see [How Do Workers and Leader Work Together](deploy-distributed#interaction).
> 
> Once you've configured a Worker to point to the Leader Node, the Leader will assign the Worker a new, random admin password. This secures each Worker from unintended access. If you need to reconfigure the Worker later, you can either:
> - Enable the Leader's **UI access** option, as [recommended above](#worker-ui-access); or
> - Reset the password on the Worker Group – see [Set Worker Passwords](authentication#worker_pwd).
>
{.box .success}

### Add a Source {#source}

To minimize dependencies, this section walks you through enabling a Cribl Stream built-in [Datagen](sources-datagens) Source to get some (fake) events flowing into your Workers. (This is the same approach used in our single-instance [Getting Started Guide](getting-started-guide#source) tutorial, but using a different datagen.)

If you prefer to configure Cribl Stream to receive events from a real data input that you've already set up, see our [Sources](sources) topic for a link to the appropriate instructions.

1. In the Leader UI's top nav, click **Manage** > **Groups**. 

1. From the resulting Groups page, click the Group name.  
  (Working with a free license, we're implicitly configuring this Source on the `default` Group.)

1. From the top nav, select **Manage** > **Data** > **Sources**. 

1. From the **Data Sources** page's tiles or left menu, select **Datagen**.  
  (You can use the search box to jump to the **Datagen** tile.)

1. Click **Add Source** to open a **New Source** modal.

1. In the **Input ID** field, name this Source `weblog` (or any unique name).

1. In the **Data Generator File** drop-down, select `weblog.log`.  
  This generates...simulated log events for a website.

1. Keep the **Events per Second per Worker Node** at the default `10` EPS for now.

1. Click **Save**. {{< id `datagen-status` >}}  

  In the **Enabled** column, the toggle set to `Yes` indicates that your Datagen Source has started generating sample data.

1. Click **Commit** at the upper right, enter a commit message, and click **Commit & Deploy**.  
  This uses Cribl Stream's `git` integration to record the `default` Group's new configuration and deploy it to your Workers.

![Source (Datagen) configuration](datagen-weblog-config.73fb67036d.png)
{border="true"}

> If you'd like to verify that the Datagen is sending events, wait about a minute for them to start accumulating. Then, on the **Manage Sources** / **Datagen** page, click the **Live**  button beside your configured Source. On the resulting **Live Data** tab, you should see events arrive within the default 10-second capture.
>
{.box .success}

### Add a Destination

On the output side, choose any of these options:

- To configure a realistic output, but with no real dependencies and no license requirement, follow the [Simulated Splunk Destination](#splunk) instructions just below.

- For a simpler option, jump to the [Internal Destination](#devnull) instructions further down.

- To send data to a real receiver that you've already set up, see our [Destinations](destinations) topic for a link to the appropriate instructions.

#### Simulated Splunk Destination {#splunk}

Here, you'll go through the steps of configuring a typical Splunk Destination, but you'll use netcat to spoof the receiver on port 9997.

1. In the Cribl Stream Leader's UI, configure a Splunk Single Instance Destination, following [these instructions](destinations-splunk). For this simulated output:
 
   - Set the **Address** to `127.0.0.1` (i.e., localhost).
   - Do not precede this IP address with any `http://` or `https://`
   - Leave the **Port** at the default `9997`.
   - Set the **Backpressure behavior** to `Drop Events`.

![A glorious spoofed Splunk Destination config](splunk-local-config.c817af0fe8.png)
{border="true"}



2. Click **Commit** at the upper right, enter a commit message, and confirm the commit.

3. Click **Deploy** at the upper right to deploy this new configuration to your Workers.

4. In the first Worker instance's terminal/console, shell in, then enter `cd /opt/cribl/bin` to access Cribl Stream's CLI. {{< id `shell` >}} 

5. Enter `nc -h` to check whether netcat is installed on this Linux instance.

  If the command fails, follow your Linux distro's steps for installing netcat. (E.g., for Ubuntu instructions, see [Docker Notes](#docker-notes) below.)

6. Enter `./cribl nc -l -p 9997` to have netcat listen on port 9997, but simply discard data.

> ##### Docker Notes {{< id `docker-notes` >}}
>
> If you're using a container like [Docker](#docker), before shelling in at step 4 above, you'll need to first open a shell inside that container: 
> `docker exec -it <CONTAINER ID> /bin/bash`
> 
> In the above [Docker Containers](#docker) deployment example, you'd want to open the shell on the `docker_workers_1` container, whose `<CONTAINER ID>` was `a3de9ea8f46f`.
> 
> Cribl's Docker containers currently run Ubuntu 20.04.6. You can install netcat with this sequence of commands:
> 
> `apt update`  
> `apt install netcat`
>
{.box .info}

The data you'll now see displayed in the terminal will be gibberish, because of Splunk's proprietary data format.

If data isn't flowing, you might need to restart Workers. You can do this [through Cribl Stream's UI](#worker-ui). With Docker containers, use `docker-compose down`, followed by `docker-compose up`.

> You can streamline future Commit and Deploy steps by entering a **Default Commit Message**, and by [collapsing actions](version-control#collapse-actions) to a combined **Commit and Deploy** button. Both options are available at **Settings** > **Global Settings** > **System** > **Git Settings** > **General**.
>
{.box .success}

#### Internal Destination {#devnull}

As an alternative to the Splunk instructions above, you can configure Cribl Stream's built-in DevNull Destination to capture events and discard them. (This is the same Destination used in our single-instance  [Getting Started Guide](getting-started-guide#destination) tutorial.)

1. In the Leader UI's top nav, click **Manage** > **Groups**. 

1. From the resulting **Groups** tab, click the Group name.  
  (Working with a free license, we're implicitly configuring this Source on the `default` Group.)

1. From the top nav, select **Data** > **Destinations**. 

1. Select **DevNull** from the **Data Destinations** page's tiles or left menu.  
  (You can use the search box to jump to the **DevNull** tile.)

1. On the resulting **devnull** row, look for the **Live** indicator under **Enabled**. This confirms that the **DevNull** Destination is ready to accept events.

1. From the **Data Destinations** page's left nav, select the **Default** Destination at the top.

1. On the resulting **Manage Default Destination** page, verify that the **Default Output ID** drop-down points to the **devnull** Destination we just examined.

1. Click **Commit** at the upper right, enter a commit message, and confirm the commit.

1. Click **Deploy** at the upper right to deploy this new configuration to your Workers.

### Add a Pipeline

To complete this distributed deployment with realistic infrastructure, let's set up a Pipeline and Route.

If you've already done the [Getting Started Guide](getting-started-guide#pipeline), you've already created a `slicendice` Pipeline. In the following steps, skip ahead to [adding an Eval Function](#eval).

1. From the current Group's submenu, select **Processing** > **Pipelines**.

1. At the **Pipelines** pane's upper right, click **Add Pipeline**, then select **Create Pipeline**.

1. In the new Pipeline's **ID** field, enter a unique identifier (e.g., `slicendice`).

1. Optionally, enter a **Description ** of this Pipeline's purpose. 

1. Click **Save**.

![Pipeline saved](pipeline-prompt.d206559d1f.png)
{border="true"}

#### Add an Eval Function {#eval}

Now add an Eval Function to the Pipeline:

1. At the **Pipelines** pane's upper right, click **Add Function**.

2. Search for the `Eval` Function, and click its link to add it.

3. In the new Function's **Evaluate Fields** section, click **Add Field**. 

4. In the new row's **Name** column, name the field `origin`.

5. In the new row's **Value Expression** column, enter: `host+" "+source`

  This new `origin` field will concatenate the host and source fields from incoming events.

![Adding Eval Function to Pipeline](add-eval-fn.e09f682405.png)
{border="true"}

6. Click **Save** to store the Function's configuration.

7. **Commit** and **Deploy** the new Pipeline configuration.

8. Optionally, open the right pane and click **Capture Data** to verify throughput.

### Add a Route

If you've already done the [Getting Started Guide](getting-started-guide#route), you've already created a `demo` Route, attached to the `slicendice` Pipeline. In the following steps, just modify the Route to send data to the new Destination you [configured above](#add-a-destination).

1. At the **Pipelines** page's top left, click **Attach to Route**.  
  This displays the **Data Routes** page.

2. Click `Add Route`.

3. Enter a unique **Route Name**, like `demo`.

4. Leave the **Filter** field set to its `true` default, allowing it to deliver all events.

5. Set the **Pipeline** drop-down to our configured `slicendice` Pipeline.

6. Set the **Output** drop-down to the Destination you configured above. If you boldly chose the [Simulated Splunk Destination](#splunk), this will be named something like `splunk:splunk9997`.

7. You can leave the **Description** empty, and leave **Final** set to `Yes`.

8. Grab the new Route by its left handle, and drag it to the top of the Routing table, so that our new Route will process events first. You should see something like the screenshot below.

9. Click **Save** to save the new Route configuration.

10. **Commit** and **Deploy** your changes.

11. Still assuming you configured a simulated Splunk output, look at the terminal where you're running netcat. You should now see events arriving.

![Route attached to Pipeline and Destination](route-splunk-connected.bdc12a271f.png)
{border="true"}

### Managing Workers (Scaling) {#scale}

With all our infrastructure in place, let's look at how a Cribl Stream distributed deployment scales up to balance the incoming event load among multiple Workers.

1. In the Leader UI's top nav, click **Manage** > **Groups**. 

2. From the Groups page, click the Group name.  
  (Working with a free license, we're implicitly configuring this Source on the `default` Group.)

3. From the top nav, select **Manage** > **Data** > **Sources**, and find the **Datagen** Source you configured [earlier](#add-a-source).

4. Click this Datagen's row to reopen its config modal.

5. Reset the **Events per Second per Worker Node** from the default `10` EPS to a high number, like `200` EPS.

![Flooding events into Cribl Stream](high-eps.e4a3359743.png)
{border="true"}

6. Click **Save**, then **Commit** and **Deploy** this higher event load.

7. From the Leader's top nav, also select **Global Config** > **Commit** to commit the Leader's newest config version. In the resulting modal, click **Commit** again to confirm.

  This uses Cribl Stream's `git` integration to save a global configuration point for your whole deployment, which you can roll back to.

![Committing Leader's config](changes-commit-all.c2deee937c.png)

8. From the Leader's top nav, click **Monitoring**.

![Monitoring tab](monitoring-link.e60a2755ea.png)

9. On the resulting **Monitoring** page, watch as the following changes unspool over the next few minutes:


  - The **CPU Load Average (1 min)** will spike as the higher event volume floods the system.
  - The **Workers** indicator at upper left will drop to `0` as the Workers restart with the new configuration you've deployed to them, and then rebound to `2`.
  - If you use the **All Workers** drop-down at upper right to toggle between your two individual Workers, you should see that Cribl Stream is balancing roughly equal event loads among them.

![](monitoring-load.925c7b5421.png)
{border="true"}

10. To confirm both Workers' `alive` status: Click  **Manage**, then click the **Workers** tab. {{< id `workers-page` >}}

![Workers tab](workers-link.3944f8b317.png)

11. To wrap up, repeat this procedure's first six steps to set the Datagen's rate back down to `10` EPS, and then save, commit, and deploy all changes.

### What Have We Done?

This completes your setup of a basic distributed deployment, using a free license, and configuring a single Worker Group of two identically configured Worker Processes. (You can extrapolate these same techniques you've just mastered to spin up virtually any number of Workers in the same way.)

To see how you can set up multiple Worker Groups – with separate configurations optimized for separate data flows – continue to the next section, noting its licensing prerequisites. 

If you're deferring or skipping that option, jump ahead to [Cleaning Up](#cleaning-up).



## Add and Manage Multiple Worker Groups (Optional) {#groups}

To add and manage more than one Worker Group – everything in this optional section – you'll need an Enterprise or Sales Trial license for your on-prem Cribl Stream deployment, or an Enterprise (or equivalent) plan for your Cribl.Cloud Organization. For details on adding multiple Worker Groups on Cribl.Cloud, see [Cribl.Cloud Worker Groups](deploy-distributed#cloud-groups).

> See [Licensing](licensing) for how to acquire and install one of the above license types. Install the license on your Leader instance, and then commit this as a Leader config change (top nav's **Global Config** dialog), before you proceed.
>
{.box .success}

Here, we'll build on the infrastructure we've created so far to:

- Configure Mapping Rules.
- Verify how Cribl Stream balances large data volumes among Worker Processes.
- Add a second Worker Group, data Source, and Destination.
- Add a second Pipeline and attach it to its own Route.
- Reconfigure Mapping Rules to send each Source's data through a separate Group.

To keep this Quick Start tutorial focused on techniques, rather than on configuring lots of infrastructure, we'll assign just one Worker to each Worker Group – one of the two Workers we launched above. But in production, you'll be able to apply the same principles to setting up any number of Worker Groups, with any number of Workers.

### Multiple-Group Setup

With an Enterprise or Sales Trial license/plan, Cribl Stream's UI adds some extra features. 

The **Manage** > **Groups** page, shown here, now supports multiple Groups. (For now, you still see only the single `default` Group – but we'll change that a few sections down.)

![UI for multiple Worker Groups](st-manage-ui-access.6e7074c299.png)
{border="true"}

> On Cribl.Cloud, the **Groups** page adds extra columns. You'll see each Group's Worker type (hybrid versus Cloud/Cribl-managed) and, for Cloud Groups, the anticipated ingress rate and Provisioned status. For details, see [Cribl.Cloud Worker Groups](deploy-distributed#cloud-groups).
>
{.box .cloud}

Note that if you've [enabled Worker UI access](#worker-ui-access), you can click directly through to each of your Workers. This feature will come in handy just below. To try it out: 

1. Either click the link in the desired Group's **Workers** column, or click the submenu's **Workers** tab.
1. Either way, from the resulting **Workers** tab, click the desired Worker's link in the **GUID** column.
1. A purple header indicates that you're viewing a Worker's UI.
1. To return to the Leader's UI, just click the top nav's **Manage** option.)

![Select a Worker's GUID link to tunnel to that Worker](st-worker-ui-access.a3a4c1d544.png)
{border="true"}
{{< id `teleport-border` >}}

![Leader's remote view of Worker's UI](st-worker-teleported.5afc9115d4.png)
{border="true"}

#### Check/Restart Workers

For the remaining steps, we want to make sure both our Workers (configured earlier) are up. Cribl Stream's top header should indicate `2 WORKERS`. You can verify that they're `alive` by clicking the top nav's **Manage** > **Workers** tabs, as shown [earlier](#workers-page). If so, proceed to [Map Groups by Config](#map).

If either or both Workers are down, restart them:

1. Make sure you've [enabled Worker UI access](#worker-ui-access). (It's time!)

1. To click through to a dormant Worker's UI, use either of the left-nav options covered just above: either the **Workers** page, or the **Manage** > **Groups** > `default` submenu of individual Worker IDs.

1. When you see that Worker's UI (purple header), click **Restart** at the upper right. 

1. Confirm your choice, and wait for a **Server has been restarted** message to appear for a few seconds.

1. Click the top nav's **Manage** option to return to the Leader's UI.

1. If the other Worker is down, repeat the above steps to restart it as well.

### Map Groups by Config {#map}

With both Workers confirmed up, let's look at how Cribl Stream has automatically mapped all these existing workers to the `default` Group.

1. From the Leader's top nav, click **Manage** > **Mappings**.

2. You now see a default Mapping Ruleset, also literally named `default`. Click it.

  > A Cribl Stream Leader can have multiple Mapping Rulesets configured, but only one can be active at a time.
  {.box .success}

3. This `default` Ruleset contains one initial Rule, literally named `Default Mappings`. Expand its accordion as shown below.

  (The `default` Ruleset and Rule naming are separate from the `default` Group's naming. All of these out-of-the-box starting configurations have been named...literally.)

![Default Mapping Rule](mappings-default.0d076420c6.png)
{border="true"}

Below the **Rule Name**, a Mapping Rule has two functional fields:

- **Filter**: `!cribl.group` – this value expression specifies "Not already assigned to a Worker Group."
- **Group**: `default` – this value specifies "Assign to the `default` Group."

So this is a catch-all rule. By following it, the Leader has assigned all (both) registered Workers to the `default` Group. 

Let's make a more-specific rule, mapping a specific Worker (by hostname) to this Group, which receives events from our `weblog` Datagen Source. 



### Add Mapping Rules

1. Click **Add Rule** at the upper right. Then configure the new Rule as shown below:

   - **Rule Name**: `weblog` – for simplicity.
   - **Filter**: `platform=="<this‑worker's‑platform>"` – get this value from the right Preview pane. Look for the `platform` field's value. In the example shown below, we got `"darwin"`.
   - **Group**: `default` – this is the only option available.

![A specific Mapping Rule by Filter condition](mappings-2nd-rule.9adf983d1e.png)
{border="true"}

2. Click **Save** to add this Rule.

3. Confirm the warning that changes will take effect immediately.

4. From the top nav, select **Global Config** > **Commit** to commit the Leader's new config.

> For more Mapping Rules/Rulesets details and examples, see [Distributed Deployment](deploy-distributed#mapping).
>
{.box .success}

Next, we'll add a second Worker Group; add a second Source (relaying Cribl Stream's internal metrics); and then add another Mapping Rule, to map our second Worker to the new Group.


### Add Another Worker Group

1. From the Leader's top nav, click **Manage** > **Groups**.

2. On the resulting Groups page, click **New Group**.

3. Name the new Group `CriblMetrics` to match its purpose.

4. Toggle **UI access** to `Yes`.

![Creating a new Group](group-new-create.3da85be372.png)

4. Save the Group.

5. Click **Deploy** on the new Group's row, and confirm your choice. The new Group should deploy immediately.

![Second Groups saved, ready to Deploy](group-2nd-saved.2ab50bac7a.png)
{border="true"}

6. From the top nav, select **Global Config** > **Commit** to commit the new config on the Leader.



### Add Another Source

On the new Group, we'll now enable a **Cribl Internal: Metrics** Source, representing a second data type.

1. From the Leader's top nav, click **Manage** > **Groups** > `CriblMetrics`.

2. From the resulting submenu, click **Data** > **Sources**. 

3. From the **Data Sources** page's tiles or left menu, select **Cribl Internal**.

4. On the **Manage Sources** / **Cribl Internal** page, toggle **Enable** to `Yes` in the `CriblMetrics` row.

5. Confirm that you want to enable `CriblMetrics`.

![CriblMetrics Source enabled](source-cribl-internal-metrics.cc1d4e0f2c.png)
{border="true"}

6. From the top nav, select **Global Config** > **Commit** to commit the new config on the Leader and Group.

7. Click **Deploy** at the upper right to deploy this new configuration to your Workers.


### Map Workers to Groups {#map-groups}

Now let's map this new group to the CriblMetrics Source's incoming events:

1. From the Leader's top nav, click **Manage** > **Groups**.

2. From the Groups page, click the **Mappings** tab.

3. Click the `default` Mapping Ruleset to open it.

4. Click **Add Rule** at the upper right. Then configure the new Rule as shown below:
  - **Rule Name**: `CriblMetrics` – for simplicity.
  - **Filter**: `hostname=="<this‑worker's‑hostname>"` – get this value from the right Preview pane. In the second Worker down, look for the `hostname` field's value. In the example shown below, we got `"2ca68fec7de0"`.
  - **Group**: `CriblMetrics` – this is the Group we want to map.

5. Move the `Default Mappings` rule to the bottom of the Ruleset, reflecting its catch-all function.

6. Click **Save** to store the new configuration.

7. Confirm the warning that changes will take effect immediately.

![Adding a Rule to map the CriblMetrics Worker to its own Group](metrics-rule-config.78b3f7472d.png)
{border="true"}

8. From the top nav, select **Global Config** > **Commit** to commit the Leader's new config.

We now have two data Sources and two Worker Groups – one each for (Web) logs versus (Cribl Internal) metrics –  along with two Mapping Rules to map data accordingly. To confirm the Workers' assignment to the two Groups, click the top nav's **Manage** tab, then select **Groups** from the submenu:

![Manage Groups page confirms Workers' mapping and assignment](group-2nd-saved.2ab50bac7a.png)
{border="true"}

To confirm further details about the Workers, click the **Workers** tab, and on the resulting **Workers** page, click anywhere on the Worker Node's row to reveal more details:

![Worker Node Details](st-workers-status-details.0f5518f8e8.png)
{border="true"}

### Configure Metrics Output

With incoming metrics now mapped to our second Worker Group, we next need to configure this Group's output. Here, we'll rely on a metrics-oriented Pipeline and a Destination that ship with Cribl Stream, and create a new Route to connect everything up.

### Examine the Metrics Pipeline

1. From the Leader's top nav, click **Manage** > **Groups** > `CriblMetrics`.

1. From the resulting submenu, select **Processing** > **Pipelines**. 

1. On the **Pipelines** page, find the `cribl_metrics_rollup` Pipeline, and click it to expand it.

1. Expand this Pipeline's Functions (including Comments) to see its configuration. It's preconfigured with a   Rollup Metrics Function to aggregate metrics to a 30-second time window. Next is an Eval Function that filters for Cribl (Cribl Stream) internal metrics and tags them on outgoing events with a new field.

![cribl_metrics_rollup Pipeline, shipped with Cribl Stream](metrics-rollup-pipeline.fd3ca7072d.png)
{scale="80%" border="true"}

### Add Another Route

We'll connect this existing Pipeline to a new Route:

1. At the **Pipelines** page's top left, click **Attach to Route**.  
  This displays the **Data Routes** page.

1. Click `Add Route`.

1. Enter a unique **Route Name**, like `metrics`.

1. Leave the **Filter** field set to its `true` default, allowing it to deliver all events.

1. Make sure the **Pipeline** drop-down is set to `cribl_metrics_rollup`.

1. As the **Output** (Destination), select our old friend `devnull:devnull`.  

    This is Cribl Stream's preconfigured Destination that simulates a downstream service while simply dropping events.

1. You can leave the **Description** empty, and leave **Final** set to `Yes`.

1. Grab the new Route by its handle, and drag it to the top of the Routing table, so that our new Route will process events first. You should see something like the screenshot below.

1. Click **Save** to save the new Route configuration.

1. **Commit** and **Deploy** your changes.

![Cribl metrics Route, Pipeline, and Destination wired up](metrics-route.fdf96b914e.png)
{border="true"}

### Verify the Multi-Group Deployment

From the sparkline on the Route you just configured (see the screenshot above), you can already see that metrics data is flowing all the way "out" of Cribl Stream – simulated here by the DevNull Destination. 

To verify the whole configuration you've created, click the top nav's **Monitoring** tab. On the **Monitoring** page, toggle the **All Groups** drop-down (upper right), toggle between the two Worker Groups to see the division of labor:

- Group `default` (the out-of-the-box Group we configured first) handles the `weblog.log` data.
- Group `CriblMetrics` handles the metrics data. 

![Monitoring individual Worker Groups' throughput](monitoring-groups.c5167749ff.png)
{border="true"}



### All the Distributed Things – Review

Before a final section where you can [tear down](#cleaning-up) your infrastructure, here's a recap of the simple (but expandable) distributed model we've created, with some ideas for expanding it:

- A distributed deployment enables Cribl Stream to scale out to higher data volumes. This load-balancing occurs even with a single Worker Group, in which all Workers share the same configuration.

- By adding multiple Worker Groups, you can partition Worker Nodes (and their data flows) by different configurations. In this demonstration, we simply mapped Workers to Groups by the Workers' `hostname`. But you can map by a range of arbitrary criteria to meet your production needs.

- E.g.: Different Groups can be managed by different teams. You can filter on DNS rules to send relevant data to the relevant team's Group.

- Different Groups can also maintain configurations for different regions' data privacy or data retention requirements.

- You can also Map workers arbitrarily using Tags and other options.

![Setting arbitrary Tags on a managed Worker](tags-on-workers.03c010a965.png)
{border="true"}

## More About Worker Groups, Nodes, and Processes – Definitions {#worker-details}

Cribl Stream refers to "Workers" at several levels. Now that you've been initiated into building distributed deployments, here's a guide to the fine points of distinguishing these levels:

- A **Worker Group** holds one or multiple **Worker Nodes**. 

- Each Worker Node functions like an independent Cribl Stream single-instance deployment. Worker Nodes don't coordinate directly with each other – they're coordinated only through communication to/from the Leader Node.

- Each Worker Node contains a configured number of **Worker Processes**. Unlike the above grouping abstractions, this is the level that actually processes data. To load-balance among Worker Processes, Cribl Stream's API Process round-robins incoming connections to them.

> When deploying on AWS/EKS, Worker Groups should not span Availability Zones. If you have EBS persistent volumes, and a node fails, its replacement won't be able to access the peer volume across AZs.
>
{.box .warning}

## Cleaning Up {#cleaning-up}

If and when you choose to shut down the infrastructure you've configured for this demonstration:

- Navigate to Cribl Stream's `default` Group > **Data** > **Sources** > **Datagen**, and switch off the toggle beside `weblog.log`.

- If you configured a second Worker Group: Navigate to Cribl Stream's `CriblMetrics` Group > **Data** > **Sources** > **Cribl Internal**, and disable the toggle beside  **CriblMetrics**.
- If you have `netcat` running on a Worker's terminal/console, `^C` it.

 - There's no need (or way) to switch off the **DevNull** Destination – it just is.

- If desired, **Commit** and **Deploy** these changes.

- If you're running the Cribl Stream server(s) on cloud instances that will (ultimately) incur charges, you're now free to shut down those cloud resources.

## Next Steps

Interested in guided walk-throughs of more-advanced Cribl Stream features? We suggest that you next check out these further resources.

- [Distributed Deployment](deploy-distributed): All the details behind the deployment approach you just mastered in this tutorial.

- [Cribl Stream Sandbox](https://cribl.io/try-cribl): Work through general and specific scenarios in a free, hosted environment, with terminal access and real data inputs and outputs.

- [Use Cases](usecase-ingest-time-fields) documentation: Bring your own services to build solutions to specific challenges.

- [Cribl Concept: Pipelines](https://vimeo.com/442794849) – Video showing how to build and use Pipelines at multiple Cribl Stream stages.

- [Cribl Concept: Routing](https://vimeo.com/441168083) – Video about using Routes to send different data through different paths.
