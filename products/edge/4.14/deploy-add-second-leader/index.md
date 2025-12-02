# Leader High Availability/Failover


To handle unexpected outages in on-prem Distributed deployments, Cribl Edge supports configuring standby Leaders for failover. In this High Availability (HA) scenario, if the primary Leader goes down, Collectors and Collector-based Sources can continue ingesting data without interruption.

> For license tiers that support configuring backup Leaders, see [Cribl Pricing](https://cribl.io/pricing/stream/).
>
{.box .info}

## How It Works

There is only ever one active Leader Node at one time. A standby Leader will become active only in the event of failover. In the configuration for all Leaders, you must specify the same **failover volume** – a shared Network File System (NFS) volume.



During the transition to a High Availability Leader setup, Cribl Edge automates the data migration process. By configuring the `failover: volume: /path/nfs` setting in the YAML configuration (or UI), all necessary files are automatically copied to the specified failover directory.

If the primary Leader Node goes down:
- Cribl Edge will recover by switching to a standby Leader.
- The new Leader will have the same configs, state, and metrics as the previous Leader Node.
- The Edge Nodes connect to the new Leader.

> In versions older than Cribl Edge 4.7, it was possible for the primary and standby Leaders to be configured differently, potentially causing issues, especially around authentication and authorization.
>
{.box .info}

![Leader High Availability/Failover Design](leaderha_summary.55ac71c8f8.png)
{border="false"}

## Required Configuration {#requirements}

Before adding a standby Leader, ensure that you have the configuration outlined in this section.

### Auth Tokens

All Leaders must have matching auth tokens. If you configure a custom **Auth token**, make sure that all Leaders have that same token.

- In Cribl Edge, check and match these values at each Leader's **Settings** > **Global** > **Distributed Settings** > **Leader Settings** > **Auth token**.
- Or, from the filesystem, check and match all Leaders' [instance.yml](#yaml) > `master` section > `authToken` values.

See [How to Secure the Auth Token for the Leader Node](securing-auth-token) for information on changing your auth token and allowed characters.



### NFS

- On all Leader Nodes, use the latest version of the NFS client. **NFSv4 is required.**
- Ensure that the NFS volume has at least 100 GB available disk space.
- Ensure that the NFS volume's IOPS (Input/Output Operations per Second) is ≥ 200. (Lower IOPS values can cause excessive latency.)
- Ensure that ping/latency between the Leader Nodes and NFS is < 50 ms.

> You can validate the NFS latency using a tool like `ioping`. Navigate to the NFS mount, and enter the following command:
>
> ```shell
> ioping .
> ```
>
> For details on this particular option, see the [ioping docs](https://github.com/koct9i/ioping/).
>
{.box .info}

### NFS Mount Options

The Leader Node will access large numbers of files whenever you use the UI or deploy configurations to Cribl Edge Edge Nodes. When this happens, NFS's default behavior is to synchronize access time updates for those files, often across multiple availability zones and/or regions. To avoid the problematic latency that this can introduce, Cribl recommends that you add one of the following NFS mount options:

1. `relatime`: Update the access time only if it is more than 24 hours ago, or if the file is being created or modified. This allows you to track general file usage without introducing significant latency in Cribl Edge. To do the same for folders, add the `reldiratime` option.
1. `noatime`: Never update the access time. (Cribl Edge does not need access times to be updated to operate correctly.) This is the most performant option – but you will be unable to see which files are being accessed. To do the same for folders, add the `nodiratime` option.

### Load Balancers {#loadbalancers}

Configure all Leaders behind a load balancer.

- [Port `4200`](ports#leader) must be exposed via a network load balancer.
- [Port `9000`](ports#leader) can be exposed via an application load balancer or network load balancer.

#### Active‑Active (Proxy) Mode for HA Leaders {#ha-proxy}

The HA Leaders use an Active-Active (Proxy) configuration to simplify load balancer (LB) routing. Both Leaders can receive UI/API traffic on port `9000`, with the standby Leader automatically proxying any incoming requests to the elected primary. This architecture maintains crucial single-writer semantics, as only the primary Leader owns the state.

For proxy mode to work, the standby must be able to reach the primary over port `9000` to forward UI/API traffic. The control‑plane connections on port `4200` are also proxied. Ensure firewall rules allow Leader‑to‑Leader communication on these paths. 

#### Load Balancer Health Checks {#load-balancer-health-checks}

Health checks over HTTP/HTTPS via the [`/health` endpoint](#load-balancer-health-checks) are only supported on port `9000`. Load balancers that support such health checks include:

- Amazon Web Services (AWS) [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) (NLB). Suitable for TCP, UDP, and TLS traffic.
- AWS [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) (ALB). Application-aware, suitable for HTTP/HTTPS traffic.
- [HAProxy](https://docs.haproxy.org/).
- [NGINX Plus](https://docs.nginx.com/nginx/).

To ensure reliable load balancing for your Cribl Leader Nodes, configure health checks against the `/health` endpoint. For optimal performance and to avoid potential throttling, adhere to the following guidelines: 

- **Polling frequency**: Set your load balancer to check the health endpoint every `60` seconds. This interval aligns with Cribl.Cloud's internal monitoring and prevents excessive API calls.
- **Avoid overly frequent checks**: Polling more often than every `60` seconds (for example, every `5` seconds) can overload the Leader's API process.
- **Load balancer routing**: Configure your load balancer to direct traffic exclusively to Leader Nodes that return a `200` status code from the health check.

For detailed information on the `/health` endpoint query and response formats, refer to the [Query the Health Endpoint](/cribl-as-code/query-health-endpoint) documentation.

> ##### AWS Network Load Balancers
>
> If you need to access the same target through a Network Load Balancer, use an IP-based target group and deactivate client IP preservation. For details, see:
> - [Why can an instance in a target group not reach itself via NLB?](https://repost.aws/questions/QU_9Dcea7NR3W3MzaZKVkcBA/why-can-an-instance-in-a-target-group-not-reach-itself-via-nlb)
> - [Why can't a target behind my Network Load Balancer connect to its own Network Load Balancer?](https://repost.aws/knowledge-center/target-connection-fails-load-balancer)
>
{.box .warning}

#### Source-Level Health Checks

For many HTTP-based Sources, you can enable a Source-level health check endpoint in the **Advanced Settings** tab. Load balancers can send periodic test requests to these endpoints, and a `200 OK` response indicates that the Source is healthy.

Frequent requests to Source-level health check endpoints can trigger throttling settings. In such cases, Cribl will return a `503 Service Unavailable` response, which can be misinterpreted as a service failure. A `503` response may indicate that the health checks are running too frequently instead of an actual service failure.

## Recommended Configuration

Use the latest NFS client across all Leaders. If you are on AWS, we recommend the following:
- Use Amazon's Elastic File System (AWS EFS) for your NFS storage.
- Ensure that the user running Cribl Edge has read/write access to the mount point.
- Configure the EFS **Throughput mode** to `Enhanced` > `Elastic`.
- For details on NFS mount options, see [Recommended NFS mount options](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-nfs-mount-settings.html).

For best performance, place your Leader Nodes in the same geographic region as the NFS storage. If the Leader and NFS are distant from each other, you might run into the following issues:
- Latency in UI and/or API access.
- Missing metrics between Leader restarts.
- Slower performance on data Collectors.

Set the primary Leader's **Resiliency** drop-down to `Failover`.

## Configure Additional Leader Nodes {#configure}

You can configure additional Leader Nodes in the following ways. These configuration options are similar to configuring the primary [Leader Node](stream/setting-up-leader-and-worker-nodes):
- [Using the UI](#ui)
- [Updating the YAML config file](#yaml)
- [Using the Command Line](#cli)
- [Using Environment Variables](#env-vars)

> Remember, the `$CRIBL_VOLUME_DIR` environment variable overrides `$CRIBL_HOME`.
>
{.box .success}

### How Cribl Edge Manages Leader Settings {#leader-settings}

When you first configure a Leader for failover, Cribl Edge will create a new [`leader.yml`](leaderyml) file in the local `$CRIBL_HOME/local/cribl` directory and will upload it to the failover volume. Configuration stored in the `leader.yml` file on the failover volume will take precedence over what is stored in the local `instance.yml` file.

The `leader.yml` file replicates most of the content of the local `instance.yml`, but leaves out the failover configuration.

While running in failover mode, when you change **Settings > Distributed Settings** via the UI, Cribl Edge applies those changes to `leader.yml` in the failover directory.

### Use the UI {#ui}

1. In **Settings** > **Global** > **Distributed Settings** > **General Settings**, select **Mode**: `Leader`.

2. Next, on the **Leader Settings** left tab, select **Resiliency**: `Failover`. This exposes several additional fields.

3. In the **Failover volume** field, enter the NFS directory to support Leader failover. This directory must be outside of `$CRIBL_HOME`. One valid solution is to use `CRIBL_DIST_MASTER_FAILOVER_VOLUME=<shared_dir>`. See [Using Environment Variables](#env-vars) for more information.

4. Optionally, adjust the **Lease refresh period** from its default `5s`. This setting determines how often the primary Leader tries to refresh its hold on the Lease file.

5. Optionally, adjust the  **Missed refresh limit** from its default `3`. This setting determines how many Lease refresh periods elapse before standby Nodes attempt to promote themselves to primary.

6. Select **Save** to restart.

> In Cribl Edge 4.0.3 and newer, when you save the **Resiliency**: `Failover` setting, further **Distributed Settings** changes via the UI will lock for both the primary and backup Leader. (This prevents errors in [bootstrapping Workers](/stream/deploy-workers) due to incomplete token synchronization between the two leaders.) However, you can still update each Leader's distributed settings by modifying its configuration files, as covered in the very next section.
>
{.box .warning}

### Use the YAML Config File {#yaml}

In `$CRIBL_HOME/local/_system/instance.yml`, under the `distributed` section:

1. Set `resiliency` to `failover`.
1. Specify a volume for the NFS disk to automatically add to the Leader Failover cluster and trigger automated data migration.


```yaml {title="$CRIBL_HOME/local/_system/instance.yml"}
distributed:
  mode: master
  master:
    host: <IP or 0.0.0.0>
    port: 4200
    resiliency: failover
    failover:
      volume: /path/to/nfs
```

> Note that `instance.yml` configs are local, not on the shared NFS volume.
>
{.box .info}

### Use the Command Line {#cli}

You can configure another Leader Node using a CLI command of this form:

`./cribl mode-master -r failover -v /tmp/shared`

For all options, see the [CLI Reference](/stream/cli-mode-master).

### Use Environment Variables {#env-vars}

You can configure additional Leader Nodes using the following environment variables listed in [Environment Variables Reference: Adding Fallback Leaders](environment-variables#fallback-leaders).

You can also configure Leader Nodes using the `distributed.master.resiliency` and `distributed.master.failover` configuration options as shown in the [example instance.yml file](instanceyml).

## Monitor the Leader Nodes

To view the status of your Leader Nodes, select **Monitoring** > **System** > **Leaders**.

![Monitoring Leader Nodes](se-monitoring-leaders.ecce85b7d7.png)
{border="true"}

## Upgrade

> Upgrading through the UI is not supported for distributed environments with a second Leader configured
> for high availability/failover. Instead, [use the command line (CLI) to upgrade](upgrading#cli).
>
{.box .warning}

Follow this upgrade order:

1. Stop the standby Leader first.
1. Upgrade the standby Leader.
2. Start the standby Leader.
3. Stop the primary Leader; the upgraded standby will take over automatically through the HA failover mechanism.
4. Upgrade the now-stopped former primary Leader for your [Stream](/stream/upgrading/) or [Edge](/edge/upgrading/) deployment.
5. Start the former primary Leader. It will rejoin and assume the standby role (since the other Leader is currently active).
6. Upgrade Edge Nodes in rolling batches; do not upgrade them to a version newer than the Leader.

## Disable a Standby Leader

Cribl recommends that you maintain a standby Leader to ensure continuity in your on-prem distributed environment. Should you decide to disable it, contact support to assist you.

## Addressing HA Migration Timeouts {#timeouts}

When enabling High Availability (HA) mode on a production system with a lot of data, the process of moving that data can sometimes take longer than expected. If the system runs out of time during this move, it might only copy part of the data, which can cause problems when the system restarts. 

To mitigate the risk of incomplete data transfers, temporarily increase the system's transfer time prior to enabling HA. You can do this by adjusting a setting called TimeoutSec in the system's configuration. Specifically, locate the **Service** section and add or modify the `TimeoutSec` directive. For example, set it to `600` seconds (`10` minutes):

```
[Service]
TimeoutSec=600
```

A successful data transfer will be confirmed by specific messages in the system's log, such as:

`{"time":"2025-02-17T22:01:46.295Z","cid":"api","channel":"ResiliencyConfigs","level":"info","message":"failover migration is complete"}`

 A failed migration due to timeout might show logs ending with a system shutdown, like this:

`{"time":"2025-02-06T15:29:15.416Z","cid":"api","channel":"ShutdownMgr","level":"info","message":"Starting shutdown ...","reason":"Got SIGTERM","timeout":0}`

>  After the HA setup is complete, revert the TimeoutSec setting to its original value. Leaving the timeout extended indefinitely can lead to prolonged leader downtime if a problem occurs during a future failover. Make sure to document the original setting before making changes to facilitate easy reversion.
>
{.box .warning}

