# Docker Deployment


You can use the following `docker-compose.yml` to stand up a Cribl Stream [distributed deployment](deploy-distributed) of a Leader and one or more Workers:

``` {title="docker-compose.yml"}
version: '3.8'
services:
  leader:
    image: cribl/cribl:latest
    environment:
      - CRIBL_DIST_MODE=leader
      - CRIBL_DIST_LEADER_URL=tcp://criblmaster@0.0.0.0:4200
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
      - CRIBL_DIST_LEADER_URL=tcp://criblmaster@leader:4200
    ports:
      - 9000
```

This uses a local directory, `~/cribl-config`, as a persistent configuration store for Cribl Stream. As a best practice, create this directory on your host OS' filesystem before you run the `docker-compose` command. 

If you prefer to use ephemeral storage, simply delete line 8 (the `CRIBL_VOLUME_DIR` definition) and lines 11–12 (the `volumes` configuration) before running the `docker‑compose` command. 

> Cribl recommends deploying the Leader on stable, highly available infrastructure, because of its role in coordinating all Worker instances.
>
{.box .info}

## Deploying Hybrid Workers

With a Leader on Cribl.Cloud, encryption is enabled by default. Set the hybrid Worker's `CRIBL_DIST_LEADER_URL` [environment variable](/stream/deploy-distributed#environment-variables) to begin with the `tls://` protocol. For example:  

```shell
CRIBL_DIST_LEADER_URL=tls://<token>@main-<Organization-name>.cribl.cloud:4200
```

## Selecting the Number of Workers

To deploy a Leader Node, plus (e.g.) two Workers already configured and wired up to the Leader, use this command:

```shell
docker-compose up -d --scale workers=2
```

To deploy a different number of Workers, just change the `workers=2` value. 

If you are deploying the Leader and Workers on the same machine or VM, and the Leader is crashing with two workers, make sure you are allocating enough memory to Docker.

## Default Image, OS, and Port Assignments

By default, the above command pulls the freshest stable image (tagged `cribl/cribl:latest`) from [Cribl's Docker Hub](https://hub.docker.com/r/cribl/cribl/tags). Our Docker images are currently built on Ubuntu 20.04.6.

Launching Docker containers with the above `docker-compose.yml` file will default to the following URLs and ports:

- Leader URL: `http://localhost:19000` 
- Worker URLs: `http://localhost:<automatically-assigned-host-ports>`

With virtual machines, the VMs' IP addresses would replace `localhost`. The automatic assignment of available host-OS ports to the Workers prevents port collisions. **Within** the Docker container, these ports will forward over TCP to port 9000. To see the ports assigned on the OS, enter:

`docker ps`

You should see results like these:

```shell
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS          PORTS                                         NAMES
a3de9ea8f46f   cribl/cribl:latest   "/sbin/entrypoint.sh…"   12 seconds ago   Up 10 seconds   0.0.0.0:63411->9000/tcp                       docker_workers_1
40aa687baefc   cribl/cribl:latest   "/sbin/entrypoint.sh…"   12 seconds ago   Up 10 seconds   0.0.0.0:63410->9000/tcp                       docker_workers_2
df362a65f7d1   cribl/cribl:latest   "/sbin/entrypoint.sh…"   13 seconds ago   Up 11 seconds   0.0.0.0:19000->9000/tcp, :::19000->9000/tcp   docker_master_1
```

The host-OS ports are shown on the left, forwarding to the container-internal ports on the right. You can use the `docker_workers_N` ports if you want to log directly into Workers. In the above example:

- Worker1 URL: `http://localhost:63411`
- Worker2 URL: `http://localhost:63410`

## Adding Port, Forwarding, and Protocol Assignments

To specify additional ports in a distributed deployment, add them to the `docker-compose.yml` file's `workers` > `ports` section:

``` {title="docker-compose.yml (modified)"}
version: '3.8'
services:
  [...]
  workers:
    image: cribl/cribl:latest
    depends_on: 
      - leader
    environment:
      - CRIBL_DIST_MODE=worker
      - CRIBL_DIST_LEADER_URL=tcp://criblmaster@leader:4200
    ports:
      - 9000
      - <Additional ports go here>
```

You can specify port forwarding, and TCP versus UDP protocol, in the same keys:

``` {title="Specifying protocols when adding ports"}
    ports:
      - 9000
      - "10060/tcp"
      - "10070/tcp"
```



## Updating the Docker Image

Cribl recommends that you always use our latest stable container image wherever possible. This will provide bug fixes and security patches for any vulnerabilities that Cribl has discovered when scanning the base image OS, dependencies, and our own software.

Note that the sample `docker-compose.yml` provided above automatically specifies the latest stable image at:

```yaml
image: cribl/cribl:latest
```

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