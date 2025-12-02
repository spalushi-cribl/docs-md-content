# Installing Cribl Edge on Linux


First, download the [install package](https://www.cribl.io/download), which is the same binary as [Cribl Stream](/stream/), to your Linux machine.
 
Next, ensure that required ports are available (see [Network Ports](deploy-planning#fleet-checklist)).  

Then, you can install and run any of the following:
* [Single](#run-edge) Cribl Edge instance.
* [Cribl Edge and Cribl Stream](#collocated) on the same host.
* [Multiple installations](#multiple-edge-or-stream) of the same product, on the same host.

## Single Cribl Edge Instance {#run-edge}

These instructions explain how to run Cribl Edge locally, as a single-instance deployment on your own machine.

Un-tar the install package in a directory of choice, and rename the resulting `cribl` directory as `cribl-edge`, e.g.:

   ```shell
   cd /opt/
   tar xvzf cribl-<version>-<build>-<arch>.tgz
   mv /opt/cribl/ /opt/cribl-edge
   ```

Set the renamed directory, (e.g., `/opt/cribl-edge/`), as your `$CRIBL_HOME` directory.

> To make the `$CRIBL_HOME` env variable available on your command line, you can:
> - Assign it once, using the `export` command:  
> `export CRIBL_HOME=/opt/cribl-edge`
> - Set it as a default, by adding it to your to your terminal profile file.  
>
{.box .info}

Next, navigate to `$CRIBL_HOME/bin`. Here, you can use `./cribl` commands to:
- **Set to Edge mode**: `./cribl mode-edge`
- **Start**: `./cribl start`
- **Stop**: `./cribl stop`
- **Reload**: `./cribl reload`
- **Restart**: `./cribl restart`
- **Get status**: `./cribl status`

You can change the hostname and port, by adding `-H` (address) and `p` (port options). E.g:   
`./cribl mode-edge -H 0.0.0.0 -p 8123`

Next, go to `http://localhost:9420` and log in with default credentials (`admin:admin`). You can now start configuring Cribl Edge with [Sources](/Edge/sources) and [Destinations](/Edge/destinations), or start creating [Routes](/Edge/routes) and [Pipelines](/Edge/pipelines).

## Cribl Edge and Cribl Stream on the Same Host {#collocated}

You can run an Edge Node with a Leader that is managing Cribl Stream, or an Edge Node and a Stream Worker Node on the same host. To accommodate these scenarios, each product has a distinct service name:

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

    Run Cribl Edge as a root user: 

    ```shell
    chown -R root:root /opt/cribl-edge
    ```

2. Set the correct mode, and configure each installation as a service. The `-H` and `-p` parameters are required.
   
    For Cribl Edge:
    
    ```shell
    /opt/cribl-edge/bin/cribl mode-managed-edge -H <leader-hostname-or-IP> -p <port> [options] [args]

    /opt/cribl-edge/bin/cribl boot-start enable
    ```
    If you are setting up Cribl Edge in single-instance mode, make sure to set `mode-edge` instead of `mode‑managed‑edge` .

    For Cribl Stream:

    ```shell
    /opt/cribl/bin/cribl mode-worker -H <leader-hostname-or-IP> -p <port> [options] [args]
    
    /opt/cribl/bin/cribl boot-start enable
    ```
  
> ##### Managing IP Addresses  {{< id `ip` >}}
>
> By default, Cribl Edge's API listens on port `9420`, instead of on Cribl Stream's default `9000` port. Some things to note:
> 
> * You can set the `CRIBL_EDGE` [environment variable](environment-variables#internal) to any value to bind to `0.0.0.0`, instead of to the `127.0.0.1` address.
> * If you are starting Cribl Edge from the CLI, make sure you set the `-H` [parameter](/cli-reference#mode-managed-edge) to `0.0.0.0`.
> * If you connect your Edge Node to a Leader, the Node will automatically update the binding IP address to the one configured in the Leader's Fleet Settings. For an Edge Node to always listen on all IP addresses, you must update the Leader's **Host** value. (In the Leader's UI, go to **Fleet Settings** > **System** > **General Settings**, and change the **Host** field to `0.0.0.0`.)
>
{.box .info}

Continue with [Setting Up Leader and Edge Nodes](setting-up-leader-and-edge-nodes) for Edge, and [Setting Up Leader and Worker Nodes](/stream/deploy-distributed#setting-up-leader-and-worker-nodes) for Stream.



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

[Cribl University](https://cribl.io/university/)'s Troubleshooting Criblet on [Switching from Cribl Edge to Cribl Stream](https://university.cribl.io/#/online-courses/0d49811e-aeaf-4d4e-b51f-4c5d444de81d) demonstrates these techniques for integrating Edge with Stream. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
