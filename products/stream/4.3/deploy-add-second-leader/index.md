# Leader High Availability/Failover


To handle unexpected outages in your on-prem distributed deployment, Cribl Stream 3.5 and above supports configuring a second Leader for failover. This way, if the primary Leader goes down, Collectors and Collector-based Sources can continue ingesting data without interruption. 

> For license tiers that support configuring backup Leaders, see [Cribl Pricing](https://cribl.io/pricing/stream/).
>
{.box .info}

## How It Works 

When you configure a second Leader, there will be only one active Leader Node at a time. The second Leader Node will be used only for failover. For this architecture to work, you must  configure all failover Leaders' volumes to point at the same Network File System (NFS) volume/shared drive. 

If the primary Leader Node goes down: 
- Cribl Stream will recover by switching to the standby Leader Node.
- The new Leader Node will have the same configs, state, and metrics as the previous Leader Node.
- The Worker Nodes connect to the new Leader.

![Leader High Availability/Failover Design](leaderha_summary.55ac71c8f8.png)
{border="true"}

## Required Configuration {#requirements}

Before adding a second Leader, ensure that you have the configuration outlined in this section.

### Auth Tokens

Make sure that both Leaders have matching auth tokens. If you configure a custom **Auth token**, match its value on the opposite Leader.
- In the UI, check and match these values at each Leader's **Settings** > **Global Settings** > **Distributed Settings** > **Leader Settings** > **Auth token**.
- Or, from the filesystem, check and match all Leaders' [instance.yml](#yaml) > `master` section > `authToken` values.

(If tokens don't match, Worker Nodes will fail to authenticate to the alternate Leader when it becomes active.)

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

The Leader Node will access large numbers of files whenever you use the UI or deploy configurations to Cribl Stream Worker Nodes. When this happens, NFS's default behavior is to synchronize access time updates for those files, often across multiple availability zones and/or regions. To avoid the problematic latency that this can introduce, Cribl recommends that you add one of the following NFS mount options:

1. `relatime`: Update the access time only if it is more than 24 hours ago, or if the file is being created or modified. This allows you to track general file usage without introducing significant latency in Cribl Stream. To do the same for folders, add the `reldiratime` option.
1. `noatime`: Never update the access time. (Cribl Stream does not need access times to be updated to operate correctly.) This is the most performant option – but you will be unable to see which files are being accessed. To do the same for folders, add the `nodiratime` option.

### Load Balancers

- Configure all Leaders behind a load balancer. 
- Expose ports `9000` and `4200` via the load balancer. 
- Load balancers must support health checks over HTTP/HTTPS via the `/health` endpoint.

Load balancers that support such health checks include:
- Amazon Web Services (AWS) [Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) (NLB). 
- [HAProxy](https://docs.haproxy.org/).
- [NGINX Plus](https://docs.nginx.com/nginx/).

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
- Ensure that the user running Cribl Stream has read/write access to the mount point.  
- Configure the EFS **Throughput mode** to `Enhanced` > `Elastic`. 
- For details on NFS mount options, see [Recommended NFS mount options](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs-nfs-mount-settings.html).

For best performance, place your Leader Nodes in the same geographic region as the NFS storage. If the Leader and NFS are distant from each other, you might run into the following issues:
- Latency in UI and/or API access. 
- Missing metrics between Leader restarts.
- Slower performance on data Collectors.

Set the primary Leader's **Resiliency** drop-down to `Failover`.

## Configuring Additional Leader Nodes {#configure}

You can configure additional Leader Nodes in the following ways. These configuration options are similar to configuring the primary [Leader Node](/stream/deploy-distributed#config-leader):
- [Using the UI](#ui)
- [Updating the YAML config file](#yaml)
- [Using the Command Line](#cli)
- [Using Environment Variables](#env-vars) 

> Remember, the `$CRIBL_VOLUME_DIR` environment variable overrides `$CRIBL_HOME`.
>
{.box .success}

### Using the UI {#ui}

1. In **Settings** > **Global Settings** > **Distributed Settings** > **General Settings**, select **Mode**: `Leader`. 

1. Next, on the **Leader Settings** left tab, select **Resiliency**: `Failover`. This exposes several additional fields.

1. In the **Failover volume** field, enter the NFS directory to support Leader failover (e.g.,`/mnt/cribl` or `/opt/cribl-ha)`. Specify an NFS directory outside of `$CRIBL_HOME`. A valid solution is to use `CRIBL_DIST_MASTER_FAILOVER_VOLUME=<shared_dir>`. See [Using Environment Variables](#env-vars) for more information.

1. Optionally, adjust the **Lease refresh period** from its default `5s`. This setting determines how often the primary Leader refreshes its hold on the Lease file.

1. Optionally, adjust the  **Missed refresh limit** from its default `3`. This setting determines how many Lease refresh periods elapse before standby Nodes attempt to promote themselves to primary.

1. Click **Save** to restart.


> In Cribl Stream 4.0.3 and later, when you save the **Resiliency**: `Failover` setting, further **Distributed Settings** changes via the UI will be locked on both the primary and backup Leader. (This prevents errors in [bootstrapping Workers](/stream/deploy-workers) due to incomplete token synchronization between the two leaders.) However, you can still update each Leader's distributed settings by modifying its configuration files, as covered in the very next section.
>
{.box .warning}

### Using the YAML Config File {#yaml}

In `$CRIBL_HOME/local/_system/instance.yml`, under the `distributed` section: 

1. Set `resiliency` to `failover`.
1. Specify a volume for the NFS disk to automatically add  to the Leader Failover cluster.

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

### Using the Command Line {#cli}

You can configure another Leader Node using a CLI command of this form:

`./cribl mode-master -r failover -v /tmp/shared`

For all options, see the [CLI Reference](/stream/cli-reference#mode-master).

### Using Environment Variables {#env-vars}

You can configure additional Leader Nodes via the following environment variables: 

 - `CRIBL_DIST_MASTER_RESILIENCY=failover`: Sets the Leader's `Resiliency` to `Failover` mode.

 - `CRIBL_DIST_MASTER_FAILOVER_VOLUME=<shared_dir>`: Sets the location (e.g., `/mnt/cribl`) of the NFS directory to support Leader failover.

 - `CRIBL_DIST_MASTER_FAILOVER_MISSED_HB_LIMIT`: Determines how many Lease refresh periods elapse before the standby Nodes attempt to promote themselves to primary. Cribl recommends setting this to `3`.

 - `CRIBL_DIST_MASTER_FAILOVER_PERIOD`: Determines how often the primary Leader refreshes its hold on the Lease file. Cribl recommends setting this to `5s`.

For further variables, see [Environment Variables](/stream/deploy-distributed#environment-variables). 

## Monitoring the Leader Nodes

To view the status of your Leader Nodes, select **Monitoring** > **System** > **Leaders**. 

![Monitoring Leader Nodes](se-monitoring-leaders.ecce85b7d7.png)
{border="true"}

## Upgrading

When upgrading:
- Stop both Leaders. 
- Upgrade ([Stream](/stream/upgrading/), [Edge](/edge/upgrading)) the Primary Leader, Second Leader, and then each Worker Node, respectively. 

## Disabling the Second Leader 

Cribl recommends that you maintain a second Leader to ensure continuity in your on-prem distributed environment. Should you decide to disable it, contact support to assist you.

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
