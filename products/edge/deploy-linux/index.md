# Installing Cribl Edge on Linux


Install and deploy Cribl Edge Nodes on-prem on Linux.

---

You can install Cribl Edge in a [supported Linux environment](#minimum-requirements)
either [connected to a Leader Node](#run-edge), [alongside Cribl Stream](#collocated),
or [in multiple instances](#multiple-edge-or-stream) on one host.

## Minimum Requirements



The following requirements are the minimum for installing and running an Edge Node on Linux.

> Below is a summary of requirements for installing Cribl Edge.
> See [Requirements](requirements) for more detailed information.
{.box .warning}

### OS Requirements

Cribl Edge Nodes can be installed on any common Linux distribution that fulfils the following criteria:

- 64-bit kernel >= 3.10
- glibc >= 2.17
- OpenSSL >= 3.0 (if FIPS mode is enabled)

### Application Requirements

Minimum application requirements for Edge Nodes are:

- ~1Ghz processor
- 512MB RAM
- 5GB free disk space per volume for operational purposes

## Download Cribl Edge

1. First, download the [install package](https://www.cribl.io/download), which is the same binary as [Cribl Stream](/stream/), to your Linux machine.
 
2. Next, ensure that required ports are available (see [Ports](ports)).  

3. Then, you can install and run any of the following:

   - [Connected to a Leader Node](#run-edge).
   - [Cribl Edge and Cribl Stream](#collocated) on the same host.
   - [Multiple installations](#multiple-edge-or-stream) of the same product, on the same host.

## Install Cribl Edge {#run-edge}

These instructions explain how to run Cribl Edge locally on your own machine
and connect it to a Leader Node.

1. Un-tar the install package in a directory of choice, and rename the resulting `cribl` directory to `cribl-edge`, for example:

   ```shell
   cd /opt/ 
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   mv /opt/cribl/ /opt/cribl-edge
   ```
> To prevent issues with Cribl file operations, `/opt/cribl` and all its subdirectories must reside on the same device. Mounting separate devices within `/opt/cribl` is not recommended. For external storage needs, such as lookups or Persistent Queues (PQs), create a completely separate directory outside of `/opt/cribl`. While `/opt/cribl` is the default installation path, Cribl can be installed to other locations if necessary.
{.box .warning}

2. Set the renamed directory, (for example, `/opt/cribl-edge/`), as your `$CRIBL_HOME` directory.

> To make the [`$CRIBL_HOME`](environment-variables#cribl_home) environment variable available on your command line, you can:
> - Assign it once, using the `export` command:  
> `export CRIBL_HOME=/opt/cribl-edge`
> - Set it as a default, by adding it to your to your terminal profile file.  
>
{.box .info}

3. Change the ownership of the installation to run Cribl Edge as a non-privileged user: 

   ```shell
   chown -R cribl:cribl /opt/cribl-edge
   ```

4. Start the Cribl service:

   ```shell
   /opt/cribl-edge/bin/cribl start
   ```

5. Set the installation mode to `mode-managed-edge` to connect the Node to the designated Leader:

   ```shell
   /opt/cribl-edge/bin/cribl mode-managed-edge /
      -H <leader-hostname-or-IP> /
      -p <port> /
      -u <token> /
     [-g <fleet>]
   ```
   
   `<token>` must match the Leader token, to ensure a connection with the Leader.
   Get the Leader token from **Settings** > **Global** > **System** > **Distributed Settings** > **Leader Settings** > **Auth token**.

   The new Node will be assigned to a [Fleet](fleet-management) on the Leader, either `default_fleet`,
   or a different one according to [Fleet mappings](mapping-edge-nodes).

   You can manually point to a specific Fleet by providing its name as `-g <fleet>`,
   but Fleet mappings will override this choice.

   > If you only need a Single-instance deployment of Cribl Edge (without a Leader),
   > use `mode-edge` instead of `mode-managed-edge`.
   > In that case you don't need to provide the Leader token.
   {.box .info}

6. Configure the installation as a service and restart it:

   ```shell
   /opt/cribl-edge/bin/cribl boot-start enable -m systemd -u cribl
   sudo systemctl restart cribl-edge.service
   ```

   > To find out more about the `boot-start` command, see [Enabling Start on Boot](deploy-boot-start).
   {.box .info}

7. Finally, go to `http://localhost:9420` (or another port you defined) and log in with default credentials (`admin:admin`). You can now start configuring Cribl Edge with [Sources](sources) and [Destinations](destinations), or start creating [Routes](routes) and [Pipelines](pipelines).

> For more ways to configure Leader and Edge Nodes, see [Set Up Leader and Edge Nodes](setting-up-leader-and-edge-nodes).
{.box .info}

## Cribl Edge and Cribl Stream on the Same Host {#collocated}

You can run an Edge Node with a Leader that is managing Cribl Stream, or an Edge Node and a Stream Worker Node on the same host. To accommodate these scenarios, each product has a distinct service name:

* `cribl-edge.service` for Cribl Edge.
* `cribl.service` for Cribl Stream.

Here, Cribl recommends un-tarring the download package twice, into two separate directories. This setup frees you to update and run each product individually. You could choose, e.g., `/opt/cribl-edge` and `/opt/cribl`:

   ```shell
   cd /opt
   tar zxvf /tmp/cribl-<version>-<build>-<arch>.tgz
   mv cribl cribl-edge
   tar zxvf /tmp/cribl-<version>-<build>-<arch>.tgz
   ```

The installation and configuration sequence will be the same for each product: 

1. Change the ownership for both installations. 

   Run Cribl Stream as a non-privileged user:  

   ```shell
   chown -R cribl:cribl /opt/cribl
   ``` 

   Run Cribl Edge as a non-privileged user: 

   ```shell
   chown -R cribl:cribl /opt/cribl-edge
   ```

> ##### Do NOT Run Cribl Edge as Root!
>
> To listen on low ports 1-1024, Cribl Edge needs privileged access. You can enable this on systemd by adding this configuration key to your `override.conf` file:
> 
> ```shell
> [Service]
> AmbientCapabilities=CAP_NET_BIND_SERVICE 
> ```
> 
> If you want to add extra capabilities, such as reading certain resources (e.g., `/var/log/*`), add `CAP_DAC_READ_SEARCH` in a space-separated format as follows:
> 
> ```shell
> [Service]
> AmbientCapabilities=CAP_NET_BIND_SERVICE CAP_DAC_READ_SEARCH
> ```
> 
> Alternatively, you can use [ACLs to allow Cribl Edge to read files](usecase-edge-acls).
> 
> For details, see [Running Edge as an Unprivileged User](deploy-runtime-user).
>
{.box .warning}

2. Set the correct mode, and configure each installation as a service. The `-H` and `-p` parameters are required.
   
   For Cribl Edge:
  
   ```shell
   /opt/cribl-edge/bin/cribl mode-managed-edge -H <leader-hostname-or-IP> -p <port> [options] [args]

   /opt/cribl-edge/bin/cribl boot-start enable

   sudo systemctl restart cribl-edge.service
   ```

   <br>For Cribl Stream:

   ```shell
   /opt/cribl/bin/cribl mode-worker -H <leader-hostname-or-IP> -p <port> [options] [args]
   
   /opt/cribl/bin/cribl boot-start enable

   sudo systemctl restart cribl.service
   ```
  
> ##### Managing IP Addresses  {#ip}
>
> By default, Cribl Edge's API listens on port `9420`, instead of on Cribl Stream's default `9000` port. Some things to note:
> 
> * You can set the `CRIBL_EDGE` [environment variable](environment-variables#single-instance-deployment-variables) to any value to bind to `0.0.0.0`, instead of to the `127.0.0.1` address.
> * If you are starting Cribl Edge from the CLI, make sure you set the `-H` [parameter](/cli-mode-managed-edge) to `0.0.0.0`.
> * If you connect your Edge Node to a Leader, the Node will automatically update the binding IP address to the one configured in the Leader's Fleet Settings. For an Edge Node to always listen on all IP addresses, you must update the Leader's **Host** value. (In the Leader's UI, go to **Fleet Settings** > **System** > **General Settings**, and change the **Host** field to `0.0.0.0`.)
> * You can disable listening on the port by toggling **Listen on port** to off in **Fleet Settings** > **General Settings** > **Advanced**.
>
{.box .info}

Continue with [Setting Up Leader and Edge Nodes](setting-up-leader-and-edge-nodes) for Edge, and [Setting Up Leader and Worker Nodes](/stream/setting-up-leader-and-worker-nodes) for Stream.


## Multiple Installations of Same Product on Same Host {#multiple-edge-or-stream}

There are situations where it makes sense to run multiple Cribl Edge or Cribl Stream installations on the same host. For example, suppose two departments want to collect the data at the edge – and each wants to process the data differently, and to deploy its own Helm chart as a daemonset.

To support this: After un-tarring the installation package, copy or move it into a separate directory for each installation. For example, if you're creating two Cribl Edge installations:

   ```shell
   cd /opt
   tar zxvf /tmp/cribl-<version>-<build>-<arch>.tgz
   cp -r cribl/ cribl-edge-01/
   mv cribl/ cribl-edge-02/
   ```

The installation and configuration procedure is the same as described for [collocated Cribl Edge and Cribl Stream](#collocated) above, except that you must:
* Omit steps pertaining to the product you are **not** installing.
* Repeat steps pertaining to the product you are **are** installing – once for each instance of the product.

> Ensure that each instance of the product runs on its own dedicated port. Either: 
> * Specify different ports when you run the `mode-managed-edge` or `mode-worker` command; or, 
> * On the host, set the `CRIBL_AUTO_PORTS` environment variable to `1`.
>
{.box .warning}

## Troubleshooting Resources {#troubleshooting}

> [Cribl University](https://cribl.io/university/)'s Troubleshooting Criblet on [Switching from Cribl Edge to Cribl Stream](https://university.cribl.io/#/online-courses/0d49811e-aeaf-4d4e-b51f-4c5d444de81d) demonstrates these techniques for integrating Edge with Stream. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
{.box .course}
