# Running in a Docker Container


As a best practice, we recommend using the UI to concatenate and copy/paste the [bootstrap script](edge-nodes-add-update#bootstrap) for adding a Docker node. 

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

> With a Leader on Cribl.Cloud, encryption is enabled by default. Set the hybrid worker's `CRIBL_DIST_LEADER_URL` [environment variable](environment-variables#cribl_dist_leader_url) to begin with the `tls://` protocol. For example:  
> 
> ```shell
> CRIBL_DIST_LEADER_URL=tls://<token>@logstream-<tenant>.cribl.cloud:4200
> ```
>
{.box .cloud}

Once that’s running, the UI displays on `http://localhost:9420/`

The ` -v /var/run/appscope:/var/run/appscope \` line exposes the Unix domain socket for the AppScope Source so that it's accessible to the `scope` application on the host, and in other containers that mount that same directory. See [Running AppScope With Cribl Edge in a Docker Container](https://appscope.dev/docs/edgescope-docker) in the AppScope documentation.

You can now start configuring Cribl Edge with [Sources](/Edge/sources) and [Destinations](/Edge/destinations), or start creating [Routes](/Edge/routes) and [Pipelines](/Edge/pipelines).


## Running as a Non-Root User

You can run the Cribl image as a non-root user within a container using Docker's `--user` option or the user setting in Compose. You can specify any desired `UID` (User ID).

To ensure correct functionality, you must set the `CRIBL_VOLUME_DIR` environment variable to a location the non-root user can write to. This is because the software requires write permissions to directories like `/opt/cribl/state`, `/opt/cribl/log`, and `/opt/cribl/local`. Failure to set `CRIBL_VOLUME_DIR` will result in a warning and potential functional issues.

If you mount a persistent volume, setting `CRIBL_VOLUME_DIR` to the mount point is strongly recommended for optimal performance and reliability. While `/tmp/cribl_data` might be used, a dedicated persistent volume ensures data integrity and prevents potential data loss or corruption.

We recommend specifying `CRIBL_VOLUME_DIR` whenever running as a non-root user to avoid unexpected behavior. 

See example below: 

```shell
docker run -d \
 -u 999:999
 -e CRIBL_DIST_MODE=managed-edge \
 -e CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@leaderhere:4200?group=fleet_name
 -e "CRIBL_EDGE=1" 
 -e CRIBL_VOLUME_DIR=/tmp/
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 -p 9420:9420
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:latest
```


## Overriding Default Ports {#ports-override}

Default ports can be overridden in the following [configuration files](/Edge/configuration-files): 

* Edge UI port (`9420`): Default definitions for `host`, `port`, and other settings are set in `$CRIBL_HOME/default/cribl/cribl.yml`, and can be overridden by defining alternatives in `$CRIBL_HOME/local/cribl/cribl.yml`.

* Data Ports, for example HTTP In (`10080`), TCPJSON in (`10420`) have default definitions for `host`, `port` and other settings defined in `$CRIBL_HOME/default/cribl/inputs.yml`, and can be overridden by defining alternatives in `$CRIBL_HOME/local/cribl/inputs.yml`.

> In the case of an API port conflict, the process will retry binding for 10 minutes before exiting.
>
{.box .info}

## Updating the Docker Image and the Packaged OS

For details, see the following topics:
- [Updating the Docker Image](upgrading-containers#docker-updates)
- [Updating the Packaged OS](upgrading-containers#os-updates)

## Troubleshooting {#troubleshooting}

When you deploy containers via [Kubernetes](/stream/deploy-kubernetes) in `managed-edge` mode, this Node will – after applying the first config bundle from the Leader – listen on the Fleet's configured **API Server Settings** > **Host**.

> If you have Fleets running Edge Nodes with version 4.8.2 and older,
> you can encounter failing health checks that require specific API server configuration.
> See [Common Challenges on the Edge](edge-common-challenges#failing-health-checks) for more information.
{.box .info}

> [Cribl University](https://cribl.io/university/) offers a course titled [How to Install & Configure Edge Using Docker](https://university.cribl.io/#/online-courses/30ec0b12-22bb-4f91-9c1b-9fe0b4091fb3) that illustrates the installation steps. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Cribl Edge courses](https://university.cribl.io/#/catalog/f397da65-f17d-4c43-b147-5fff8b8c5876).
>
{.box .info}