# Running in a Docker Container


As a best practice, we recommend using the UI to concatenate and copy/paste the [bootstrap script](managing-edge-nodes#add-node) for adding a Docker node. 

Or, for a single-instance deployment, start the container with the Docker command below:

```shell
docker run -d -e CRIBL_EDGE=1 \
 -p 9420:9420 \
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 --privileged \
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:latest
```

For a distributed deployment, edit the Docker command below to replace `INSERT_TOKEN` with a unique, [secure token value](securing-auth-token). Then start the container with the revised command.

```shell
docker run -d \
 --privileged \
 -e CRIBL_DIST_MODE=managed-edge \
 -e CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@leaderhere:4200?group=fleet_name
 -e "CRIBL_EDGE=1" 
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 -p 9420:9420
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:latest
```

> With a Leader on Cribl.Cloud, encryption is enabled by default. Set the hybrid worker's `CRIBL_DIST_LEADER_URL` [environment variable](/stream/deploy-distributed#environment-variables) to begin with the `tls://` protocol. For example:  
> 
> ```shell
> CRIBL_DIST_LEADER_URL=tls://<token>@logstream-<tenant>.cribl.cloud:4200
> ```
>
{.box .cloud}

Once that’s running, the UI displays on `http://localhost:9420/`

The ` -v /var/run/appscope:/var/run/appscope \` line exposes the Unix domain socket for the AppScope Source so that it's accessible to the `scope` application on the host, and in other containers that mount that same directory. See [Running AppScope With Cribl Edge in a Docker Container](https://appscope.dev/docs/edgescope-docker) in the AppScope documentation.

You can now start configuring Cribl Edge with [Sources](/Edge/sources) and [Destinations](/Edge/destinations), or start creating [Routes](/Edge/routes) and [Pipelines](/Edge/pipelines).

## Overriding Default Ports {#ports-override}

Default ports can be overridden in the following [configuration files](/Edge/configuration-files): 

* Edge UI port (`9420`): Default definitions for `host`, `port`, and other settings are set in `$CRIBL_HOME/default/cribl/cribl.yml`, and can be overridden by defining alternatives in `$CRIBL_HOME/local/cribl/cribl.yml`.

* Data Ports, for example HTTP In (`10080`), TCPJSON in (`10420`) have default definitions for `host`, `port` and other settings defined in `$CRIBL_HOME/default/cribl/inputs.yml`, and can be overridden by defining alternatives in `$CRIBL_HOME/local/cribl/inputs.yml`.

> In the case of an API port conflict, the process will retry binding for 10 minutes before exiting.
>
{.box .info}

## Updating the Docker Image

Cribl recommends that you always use our latest stable container image wherever possible. This will provide bug fixes and security patches for any vulnerabilities that Cribl has discovered when scanning the base image OS, dependencies, and our own software.

You can explicitly pull the latest stable image with this CLI command:

```shell
docker pull cribl/cribl:latest
```

### Updating the Packaged OS

Cribl strongly recommends that you monitor and patch vulnerabilities in the [packaged OS](#defaults). The base OS might have been updated with fixes for new vulnerabilities discovered after Cribl published its container images. This is especially important if you choose to keep an earlier Cribl image in production.

You can update the base OS image by updating the package, as shown in the following `Dockerfile`. This example assumes you're updating Cribl's `latest` image:

```yaml title='Dockerfile'
FROM cribl/cribl:latest
RUN apt-get update && \
    apt-get -y upgrade dpkg
```

> Cribl Edge supports both Docker and `containerd` runtimes. 
>
{.box .info}

## Troubleshooting {#troubleshooting}

When you deploy containers via [Kubernetes](/stream/deploy-kubernetes) in `managed-edge` mode, this Node will – after applying the first config bundle from the Leader – listen on the Fleet's configured **API Server Settings** > **Host**.

If you encounter spurious health-check failures, change that **Host**  setting to `0.0.0.0` (rather than the default `127.0.0.1`, which can block health checks).

> [Cribl University](https://cribl.io/university/) offers a course titled [How to Install & Configure Edge Using Docker](https://university.cribl.io/#/online-courses/30ec0b12-22bb-4f91-9c1b-9fe0b4091fb3) that illustrates the installation steps. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Cribl Edge courses](https://university.cribl.io/#/catalog/f397da65-f17d-4c43-b147-5fff8b8c5876).
>
{.box .info}