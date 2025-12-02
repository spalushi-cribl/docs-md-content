# Single-Instance/​Basic Deployment


Getting started with Cribl Stream on a single instance

---

For small-volume or light processing environments – or for test or evaluation use cases – a single instance of Cribl Stream might be sufficient to serve all inputs, event processing, and outputs. This page outlines how to implement a single-instance deployment.

## Architecture

![](sources-and-destinations-1a.da1ff6acc5.png)

## Install on Linux {#install}

> For system requirements, see [Worker Node and Single-Instance Deployment Requirements](requirements#workers-single-reqs). To launch Cribl Stream in compliance with secure Federal Information Processing Standards (FIPS), see [FIPS Mode](fips-mode).
>
{.box .info}


* Install the package on your instance of choice. Download it [here](https://www.cribl.io/download).
* Ensure that required ports are available (see [Network Ports](ports)).
* Un-tar in a directory of choice, e.g., in the `/opt/` directory:
`tar xvzf cribl-<version>-<build>-<arch>.tgz`

> To prevent issues with Cribl file operations, `/opt/cribl` and all its subdirectories must reside on the same device. Mounting separate devices within `/opt/cribl` is not recommended. For external storage needs, such as lookups or Persistent Queues (PQs), create a completely separate directory outside of `/opt/cribl`. While `/opt/cribl` is the default installation path, Cribl can be installed to other locations if necessary.
{.box .warning}

### Install Cribl Stream and Cribl Edge on the Same Host

You can run an Edge Node with a Leader that is managing Cribl Stream, or an Edge Node and a Worker Node on the same host. For details, see [Installing Cribl Edge and Cribl Stream on the Same Host](/edge/deploy-single-instance#collocated).

Next, refer to the [Run Cribl Stream](run-stream) topic for details on managing Cribl Stream.