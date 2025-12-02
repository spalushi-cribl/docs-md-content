# Set Up Leader and Worker Nodes


In a [Distributed deployment](deploy-distributed), each Cribl Stream instance runs in a defined mode:
either as a Leader Node, governing the whole deployment, or as a Worker Node managed by the Leader.

> Before setting up your Leader and Worker Nodes, be sure to review [Deployment Planning](deploy-planning).
>
{.box .info}

You can select the mode in which any instance operates, using:

- [Environment Variables](#environment-variables)
- [UI Settings](#ui-settings)
- [UI Access to Workers (Teleporting)](#teleporting)
- [Add Tags to Workers in Teleport View](#add-tags-to-workers-in-teleport-view)
- [CLI](#cli)
- [`instance.yml`](#instanceyml)

> Internally, the mode for a Leader Node is called `master`, which corresponds to an earlier, outdated name for the mode.
> That is why you will see `master` in commands, configuration keys and environment variable names.
{.box .success}

## Set Mode with Environment Variables {#environment-variables}

You can configure the mode for Leader and Worker Nodes via [environment variables](environment-variables#distributed-deployment-variables).

[`CRIBL_DIST_LEADER_URL`](environment-variables#cribl_dist_leader_url) defines the Leader Node URL for the deployment.
If `CRIBL_DIST_LEADER_URL` is set, the mode for the instance defaults to `worker`.
Set `CRIBL_DIST_LEADER_URL` in the following way:

```
export CRIBL_DIST_LEADER_URL=tcp://${CRIBL_DIST_TOKEN:-criblmaster}@masterHostname:4200 ./cribl start
```

Alternatively (or if `CRIBL_DIST_LEADER_URL` is not set), you can define the mode for the current instance
by using the `CRIBL_DIST_MODE` variable, like this:

```
export CRIBL_DIST_MODE=leader
```

Environment variables take priority over other ways of setting up Leader and Worker Nodes.
If any `CRIBL_DIST_*` variable is defined in the system, you won't be able to configure Distributed mode settings via UI.

## Set Mode with UI Settings {#ui-settings}

You can configure the mode for each instance in a Distributed deployment via UI.

1. In the sidebar, select **Settings**, then **Global**.
1. Select **Distributed Settings**, then **General Settings**.
1. In **Mode**, choose the mode you want the instance to run as:
   - **Leader**.
   - **Stream: Managed Worker (managed by Leader)**.
1. Select **Leader Settings**, and configure the required Leader settings:
   - **Address** and **Port** for Leader Node.
   - **Address** (for example: `criblleader.mycompany.com`) for Worker.

     > If you need to customize the Leader's port (from its default `4200`), you can do so via the CLI's [`mode‑worker` command](cli-reference#mode-worker).
     {.box .info}

1. Optionally, customize the remaining settings.

> If you configure [Leader High Availability/Failover](deploy-add-second-leader),
> further **Distributed Settings** changes will be locked for each Leader Node.
> You can still update distributed settings by modifying configuration files, as outlined on the page linked above.
{.box .warning}

### Enable UI Access to Workers (Teleporting) {#teleporting}

Remote UI access to Workers (also known as teleporting) enables you to click through from the Leader's **Workers** page
to an authenticated view of each Worker's UI.

> Any changes that you make on a Worker Node via remote access
> (with the exception of [adding tags](#adding-tags-to-workers-in-teleport-view))
> will not be propagated to the Leader Node.
{.box .warning}

To enable (remote) UI access to Workers from the Leader Node:

1. In the sidebar, select **Worker Groups**.
2. In the row of the Worker Group you want to enable teleporting for, enable **UI Access**.

  ![Worker UI access setting](st-manage-ui-access-4.8.png)
  {border="true"}

  > On Cribl.Cloud, the **Worker Groups** page adds extra columns.
  > You'll see each Group's Worker type (customer-managed / hybrid versus Cribl-managed / Cribl.Cloud) and, for Cloud Groups,
  > the anticipated ingress rate and Provisioned status.
  > For details, see [Managing Cribl.Cloud Worker Groups](cloud-workers).
  {.box .cloud}

To access a Worker Node via teleporting:

1. In the sidebar, select **Workers**.
1. Select the link for any Worker Node you want to inspect.

   A purple border that appears around the UI indicates that you are viewing the Worker Node remotely.
  
   Select **Restart Stream** on the border to restart the Worker,
   or select the **X** close button to return to the **Workers** page.

![Authenticated view of a Worker](worker-UI-access-4.9.png)
{scale="90%"}

An alternative way of allowing UI access to Workers
is enabling the `workerRemoteAccess` configuration key in [`groups.yml`](groupsyml).

### Add Tags to Workers in Teleport View {#tags}

You can add tags to a Worker Node you've accessed by teleporting.
Tags help the Leader Node map a Worker Node to a Worker Group.
Note that changing the tags on a Worker can cause the Leader to map the Worker into a new Group.

1. [Teleport](#teleporting) into a Worker Node.
1. On the **Workers** submenu, select **Worker Settings**, then **Distributed Settings**.
1. Enter tags for this Worker Node and select **Save**.

> In Cribl.Cloud, you can only add tags to _hybrid_ Worker Nodes.
> You must accept the confirmation modal, as changing the tags on a Worker
> can affect the Worker Group that the Leader maps it into.
{.box .cloud}

## Set Mode with CLI {#cli}

You can configure the mode for an instance in a Distributed Deployment by using commands:
[`mode-master`](cli-reference#mode-master) and [`mode-worker`](cli-reference#mode-worker).

For example, to configure a Worker Node, use command of this form:

```shell
./cribl mode-worker -H <master-hostname-or-IP> -p <port> [options] [args]
```

`-H` and `-p` are required parameters. For other options, see the [CLI Reference for mode-worker](cli-reference#mode-worker).

Here is an example command:

```shell
./cribl mode-worker -H 192.0.2.1 -p 4200 -u myAuthToken
```

Cribl Stream will need to restart after this command is issued.

## Set Mode in `instance.yml` {#instanceyml}

You can configure each instance to be either Leader or Worker in [`$CRIBL_HOME/local/_system/instance.yml`](instanceyml),
under the `distributed` section, by setting the `mode` value:

{{% tabs "leader" "leader" "Leader" "worker" "Worker" %}}
{{% tab-item "leader" %}}

```yaml {title="$CRIBL_HOME/local/_system/instance.yml"}
distributed:
  mode: master
  master:
    host: <IP or 0.0.0.0>
    port: 4200
    tls:
      disabled: true
    ipWhitelistRegex: /.&ast;/
    authToken: <auth token>
    enabledWorkerRemoteAccess: false
    compression: none
    connectionTimeout: 5000
    writeTimeout: 10000
```

{{% /tab-item %}}
{{% tab-item "worker" %}}

```yaml {title="$CRIBL_HOME/local/_system/instance.yml"}
distributed:
  mode: worker
  envRegex: /^CRIBL_/
  master:
    host: <master address>
    port: 4200
    authToken: <token here>
    compression: none
    tls:
      disabled: true
    connectionTimeout: 5000
    writeTimeout: 10000
  tags:
       - tag1
       - tag2
       - tag42
  group: teamsters
```

{{% /tab-item %}}
{{% /tabs %}}

## Leader/Worker Mode and Auth Tokens {#tokens}

When you configure an instance to work as the Leader, switching from a Single-instance to a Distributed mode,
Cribl Stream generates a [random auth token](securing-auth-token).

See [How to Secure the Auth Token for the Leader Node](securing-auth-token) for information on
changing your auth token and allowed characters.

> ##### Troubleshooting Resources {#troubleshooting}
>
> [Cribl University](https://cribl.io/university/) offers a Troubleshooting Criblet
> on [Attaching Workers to Leaders](https://university.cribl.io/#/online-courses/32a4a5d5-cdfb-4e4a-87ed-f93082e7a15e).
> (To follow the direct course link, first log into your Cribl University account - it's free!)
{.box .info}

