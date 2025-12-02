# Run Cribl Edge in a Docker Container


Install Cribl Edge in a container using a Docker image.

---

You can easily deploy and run Cribl Edge from a container image into a Docker
container, making it simple to integrate Cribl Edge into your existing
containerized environments.

In this article, you'll learn how to pull the Cribl Edge Docker image, configure
it, and manage its lifecycle within your containerized environment.

## Add a Docker Node from the Cribl Edge UI

As a best practice, we recommend using the UI to create and copy/paste the
bootstrap script for adding a Docker node. See our documentation on how to 
[Add a New Edge Node with a Bootstrap Script](edge-nodes-add-update#bootstrap).

## Start the Container with a Docker Command

For a Single-instance deployment, start the container with the Docker command below
(replace `4.13.2` in the last line with the version of Cribl Edge you want to install):

```shell
docker run -d
 -e CRIBL_EDGE=1 \
 -p 9420:9420 \
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 --privileged \
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:4.13.2
```

For a Distributed deployment, edit the Docker command below to replace
`INSERT_TOKEN` with a unique, [secure token value](securing-auth-token). Then
start the container with the revised command.

```shell
docker run -d \
 -e CRIBL_DIST_MODE=managed-edge \
 -e CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@leaderhere:4200?group=fleet_name \
 -e CRIBL_EDGE=1 \
 -p 9420:9420 \
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 --privileged \
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:4.13.2
```

> With a Leader on Cribl.Cloud, encryption is enabled by default. Set the hybrid
> worker's `CRIBL_DIST_LEADER_URL`
> [environment variable](environment-variables#cribl_dist_master_url) to begin
> with the `tls://` protocol. For example:  
> 
> ```shell
> CRIBL_DIST_LEADER_URL=tls://<token>@logstream-<tenant>.cribl.cloud:4200
> ```
>
{.box .cloud}

Once that’s running, the UI displays on `http://localhost:9420/`

The ` -v /var/run/appscope:/var/run/appscope \` line exposes the Unix domain
socket for the AppScope Source so that it's accessible to the `scope`
application on the host, and in other containers that mount that same directory.
See [Running AppScope With Cribl Edge in a Docker
Container](https://appscope.dev/docs/edgescope-docker) in the AppScope
documentation.

You can now start configuring Cribl Edge with [Sources](sources) and
[Destinations](destinations), or start creating [Routes](routes) and
[Pipelines](pipelines).

> When a Node in a Kubernetes Fleet restarts, it might receive a new GUID,
> which can cause duplicate entries to appear on the **Edge Nodes** page.
> To resolve this issue, navigate to **General Settings > Fleet Configuration**
> and set **Time to keep disconnected Nodes** to 0.
{.box .info}

[Snippet not found: content/edge/4.14/snippets/_identify-docker-nodes.md]

## Run Cribl Edge as a Non-Root User

You can run the Cribl image as a non-root user within a container using Docker's
`--user` option or the user setting in Compose. You can specify any desired
`UID` (User ID).

To ensure correct functionality, you must set the `CRIBL_VOLUME_DIR` environment
variable to a location the non-root user can write to. This is because the
software requires write permissions to directories like `/opt/cribl/state`,
`/opt/cribl/log`, and `/opt/cribl/local`. Failure to set `CRIBL_VOLUME_DIR` will
result in a warning and potential functional issues.

If you mount a persistent volume, setting `CRIBL_VOLUME_DIR` to the mount point
is strongly recommended for optimal performance and reliability. While
`/tmp/cribl_data` might be used, a dedicated persistent volume ensures data
integrity and prevents potential data loss or corruption.

We recommend specifying `CRIBL_VOLUME_DIR` whenever running as a non-root user
to avoid unexpected behavior. 

See example below: 

```shell
docker run -d \
 -u 999:999
 -e CRIBL_DIST_MODE=managed-edge \
 -e CRIBL_DIST_LEADER_URL=tcp://INSERT_TOKEN@leaderhere:4200?group=fleet_name \
 -e CRIBL_EDGE=1 \
 -p 9420:9420 \
 -e CRIBL_VOLUME_DIR=/tmp/ \
 -v /var/run/appscope:/var/run/appscope \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /:/hostfs:ro \
 --restart unless-stopped \
 --name cribl-edge \
 cribl/cribl:4.13.2
```

> To prevent issues with Cribl file operations, `/opt/cribl` and all its subdirectories must reside on the same device. Mounting separate devices within `/opt/cribl` is not recommended. For external storage needs, such as lookups or Persistent Queues (PQs), create a completely separate directory outside of `/opt/cribl`. While `/opt/cribl` is the default installation path, Cribl can be installed to other locations if necessary.
{.box .warning}

## Override Default Ports {#ports-override}

You can override default ports in the following [configuration files](/Edge/configuration-files): 

* Edge UI port (`9420`): Default definitions for `host`, `port`, and other
  settings are set in `$CRIBL_HOME/default/cribl/cribl.yml`, and can be
  overridden by defining alternatives in `$CRIBL_HOME/local/cribl/cribl.yml`.

* Data Ports, for example HTTP In (`10080`), TCPJSON in (`10420`) have default
  definitions for `host`, `port` and other settings defined in
  `$CRIBL_HOME/default/cribl/inputs.yml`, and can be overridden by defining
  alternatives in `$CRIBL_HOME/local/cribl/inputs.yml`.

> In the case of an API port conflict, the process will retry binding for 10
> minutes before exiting.
>
{.box .info}

## Update the Docker Image and the Packaged OS

For details, see the following topics:
- [Updating the Docker Image](upgrading-containers#docker-updates)
- [Updating the Packaged OS](upgrading-containers#os-updates)

## Shut Down Docker Containers

To shut down Docker containers that are running Cribl Edge, use `docker stop` instead
of `docker kill`. This allows Docker to shut down the container cleanly. When you
run `docker stop` for your Cribl Edge containers, the following happens:

- The container runs `cribl stop` and waits for all `cribl` processes to exit before
  shutting down.
- Docker waits up to 10 seconds by default. If this is not enough time for your
  processes to exit, you can increase the default time using `docker stop -t 30 <container>`.
  Replace the number `30` in this example with the amount of time you need, in seconds.

## Troubleshooting {#troubleshooting}

- When you deploy containers via [Kubernetes](/stream/deploy-kubernetes) in
  `managed-edge` mode, this Node will – after applying the first config bundle
  from the Leader – listen on the Fleet's configured **API Server Settings** >
  **Host**.

- If you have Fleets running Edge Nodes with version 4.8.2 and older,
  you can encounter failing health checks that require specific API server configuration.
  See [Common Challenges on the Edge](edge-common-challenges#failing-health-checks) for more information.