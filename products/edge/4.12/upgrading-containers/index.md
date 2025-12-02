# Upgrading Containers


Learn how to upgrade Edge Nodes on Containers.

---

## Upgrading Cribl Edge Containers {#containers-status}

If you're asking, "Why didn't my Kubernetes Node upgrade?" or perhaps, "Why
_did_ my Docker Node upgrade?", then you're in the right place. 

Cribl Edge handles upgrades for containerized Nodes (4.5.0 and newer) differently than standalone
or on-prem Nodes. Leader Nodes don't upgrade Edge Nodes that are running in a
container (such as containerd, Docker, or Kubernetes), because Edge Nodes revert
back to their Helm chart image after a Pod restarts. Instead, upgrade
containerized Nodes manually.

Upgrades for containerized Nodes also work a little differently in 4.5.0 and
newer compared to 4.4 and older. Consider these upgrade scenarios:

|Scenario                                                                                                                                    | Do the Nodes Upgrade? | Logic Explanation                                                                                                                                                                                                                                                         |
|--------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A Fleet contains  **Kubernetes / containerized**  Nodes running version v.4.5.0. The Fleet has a set  **Target Software Version**  of 4.5.2. | No                    | <ul><li>The Nodes are running in a container. Containerized (Docker, Kubernetes, etc) Nodes will not  be upgraded automatically.</li>  <li>Nodes on version 4.5.0 and newer inform the Leader that they are containerized Nodes.</li></ul>                                |
| A Fleet contains  **Kubernetes / containerized**  Nodes running v.4.4. The Fleet has a set  **Target Software Version**  of 4.5.0.   | Yes                   | <ul><li>Nodes on v.4.4 and older do not have the ability to inform the Leader that they're containerized.</li></ul>                                                                                                                                              |


## Specific Considerations for Upgrading Containers {#container-upgrades}

When upgrading, you must consider the initial install method for Edge Nodes relevant to containerized deployments.

### Considerations for Kubernetes (Helm Charts) 

**Recommended method**: If you initially deployed these Edge Nodes using Helm Charts, it's recommended to maintain consistency and upgrade them using the same method.

**Process**: Update the Helm Chart version to the desired Cribl Edge version in your Helm repository.

Deploy the updated Helm Chart to your Kubernetes cluster. Kubernetes will manage the upgrade process and ensure no downtime due to restarts.

> 
> Upgrading Edge Nodes manually within containers is highly discouraged. It can interfere with Kubernetes' resource management and lead to unexpected behavior.
>
{.box .warning}

For details on managing your Kubernetes Deployment, see [Deploying via Kubernetes](deploy-running-kubernetes).

### Considerations for Docker Containers {#docker}

Cribl Edge upgrades won't be applied if your Node Info indicates `env.CRIBL_INSTALL_TYPE` is set to `CONTAINER`. This is because upgrades are designed for installations on physical or virtual machines, and containerized deployments manage updates differently.

## Upgrading Kubernetes Edge Nodes {#k8-upgrade}

[Edge Nodes deployed in Kubernetes](deploy-running-kubernetes) can't be upgraded
via the Leader. Instead, use our [Helm charts](https://github.com/criblio/helm-charts)
to upgrade the Edge Nodes deployed in your Kubernetes cluster: 
  
 ```
 helm upgrade --install
 ```

For details on common challenges, see [edge-via-kubernetes](edge-common-challenges#edge-via-kubernetes).

## Updating the Docker Image {#docker-updates}

Cribl recommends that you always use our latest stable container image wherever possible. This will provide bug fixes and security patches for any vulnerabilities that Cribl has discovered when scanning the base image OS, dependencies, and our own software.

You can explicitly pull the latest stable image with this CLI command:

```shell
docker pull cribl/cribl:latest
```

### Updating the Packaged OS {#os-updates}

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

