# Environment Variables


You can use the following environment variables to configure your Cribl Stream installation. 

| Name | Purpose |
| --- | --- |
| `CRIBL_TMP_DIR` | Defines the root of a temporary directory. See [Usage Notes](#cribl_tmp_dir) below. |
| `CRIBL_VOLUME_DIR` | Sets a directory that persists modified data between different containers or ephemeral instances. When set, this environment variable overrides [`CRIBL_HOME`](#cribl_home). It also creates predefined subdirectories in the specified directory. If that directory already contains subdirectories with those names, they will be overwritten. |
| `CRIBL_BOOTSTRAP`     | Quickstart a Cribl instance by configuring this variable.|
| `CRIBL_BOOTSTRAP_HOST`   | Host name for connecting to the Leader Node when setting up a new Worker Node. |
| `CRIBL_EDGE` | (Cribl Edge only) When set to any value, runs this command at container start: `cribl mode‑edge ‑H 0.0.0.0`. This launches the instance as an Edge Node, listening on a host at `0.0.0.0`. |
| `CRIBL_EDGE_FS_ROOT` | (Cribl Edge running in a container only) Location of the host OS filesystem when mounting in a container. Defaults to `/hostfs`. |
| `CRIBL_K8S_POD` | (Cribl Edge on Kubernetes only) Sets the name of the Kubernetes Pod in which Cribl Edge is deployed. |
|`CRIBL_K8S_TLS_REJECT_UNAUTHORIZED`| (Cribl Edge on Kubernetes only) Set to `0` to disable certificates validation when connecting to the Kubernetes APIs. When you disable this environment variable, all Kubernetes features (including Metadata, Metrics, Logs, and AppScope metadata) will tolerate invalid TLS certificates (such as, expired, self-signed, and so forth) when connecting to the Kubernetes APIs.|
| `CRIBL_SERVICEACCOUNT_PATH` | (Cribl Edge on Kubernetes only) Path to the `ServiceAccount` to use to query the Kubernetes API. Defaults to  `/var/run/secrets/kubernetes.io/serviceaccount`. |

## Distributed Deployment Variables

You can use the following environment variables to configure your distributed Cribl Stream instance. 

| Name | Purpose |
| --- | --- |
| `CRIBL_DIST_LEADER_URL` |URL of the Leader Node. Example: `CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`. See [Usage Notes](#cribl_dist_leader_url) below.| 
| `CRIBL_DIST_MODE` |`worker` or  `master`. Defaults to `worker`, if (and only if) `CRIBL_DIST_LEADER_URL` is present.|
| `CRIBL_DIST_WORKER_PROXY`     | Communicate to the Leader Node via a SOCKS proxy. See [Usage Notes](#cribl_dist_worker_proxy) below.|

## Usage Notes {#format}

This section explains how to use certain complex environment variables.

### `CRIBL_DIST_LEADER_URL`
  
Use this format:

`<tls|tcp>://<authToken>@host:port?group=defaultGroup&tag=tag1&tag=tag2&tls.<tls_settings>`

> To generate a random authentication token, leave `<authToken>` unchanged.
> You can define it to add your own token instead, but make sure it's secure enough.
>
{ .box .info }

Here are the components:

* `group` – The preferred Worker Group assignment.
* `resiliency` – The preferred Leader failover mode. 
* `volume`– The location of the NFS directory to support Leader failover. 
* `tag` – A list of tags that you can use to assign ([Stream](/stream/deploy-distributed#mapping), [Edge](/edge/fleets#mapping)) the Worker to a Worker Group.
* `disableSNIRouting` – Whether Server Name Indicator (SNI) routing is enabled on the Worker Node. Boolean; defaults to `false`.
  > Do not edit this advanced setting without supervision by Cribl Support.
  > Changing this setting can affect the scalability of your system because it
  > determines how Cribl Stream routes connections between Leader Nodes and Worker Nodes.
  > See the [Known Issue](known-issues#2024-04-25-–-v46-–-in-environments-that-use-a-proxy-or-a-tls-breaking-security-appliance-the-worker--edge-node-to-leader-communication-is-disrupted-cribl-244512024-04-25--v46--in-environments-that-use-a-proxy-the-worker--edge-node-to-leader-communication-is-disrupted-cribl-24451) that resulted in this setting for more details.
  {.box .warning} 
* `tls.privKeyPath` – Private Key Path.
* `tls.passphrase` – Key Passphrase.
* `tls.caPath` – CA Certificate Path.
* `tls.certPath` – Certificate Path.
* `tls.rejectUnauthorized` – Validate Client Certs. Boolean; defaults to `false`.
* `tls.requestCert` – Authenticate Client (mutual auth). Boolean; defaults to `false`.
* `tls.commonNameRegex` – Regex matching peer certificate > subject > common names allowed to connect. Used only if `tls.requestCert` is set to `true`.

> `CRIBL_DIST_LEADER_URL` was previously called `CRIBL_DIST_MASTER_URL`.
> This name is still in use in certain contexts, for example in Helm charts.
>
{ .box .info }

### `CRIBL_TMP_DIR`

Sources use this variable to construct temporary directories in which to stage downloaded Parquet data. If `CRIBL_TMP_DIR` is not set (the default), Cribl applications create subdirectories within your operating system's default temporary directory:
* For Cribl Stream: `<OS_default_temporary_directory>/stream/`.
* For Cribl Edge: `<OS_default_temporary_directory>/edge/`.

For example, on Linux, Stream's default staging directory would be `/tmp/stream/`. 

If you explicitly set this `CRIBL_TMP_DIR` environment variable, its value replaces this OS-specific default parent directory.

### `CRIBL_DIST_WORKER_PROXY`

Use the format `<socks4|socks5>://<username>:<password>@<host>:<port>`. Only `<host>:<port>` are required. 
  
The default protocol is `socks5://`, but you can specify `socks4://proxyhost:port` if needed. 
  
To authenticate on a SOCKS4 proxy with username and password, use this format: `username:password@proxyhost:port`. The `proxyhost` can be a `hostname`, `ip4`, or `ip6`.

### `CRIBL_BOOTSTRAP_HOST`

`CRIBL_BOOTSTRAP_HOST` overrides the [Add/Bootstrap New Worker](/stream/deploy-workers#add-bootstrap) script generator's default hostname.

For example, if you set `CRIBL_BOOTSTRAP=myhost`, then `myhost` will appear in the script modal's **Leader hostname/IP** field, instead of the URL used in the browser.

### `CRIBL_BOOTSTRAP`

`CRIBL_BOOTSTRAP` enables specifying a URL, an absolute disk file path, or a YAML string, in order to bootstrap a configuration to the `$CRIBL_HOME/local` directory. Cribl Stream applies this configuration only upon its first startup.

For any method, Cribl Stream expects each targeted config file to be YAML-formatted. Each file's top-level keys should be the paths to config files inside the `$CRIBL_HOME/local/...` subdirectory.

Below is an example of a bootstrap file. Its output, when Cribl Stream starts, would be to create three files inside the `$CRIBL_HOME/local/cribl` path: `inputs.yml`, `outputs.yml`, and `pipelines/route.yml`.

```yaml
cribl/inputs.yml:
  inputs:
    <id>:
      <config>
cribl/outputs.yml:
  outputs:
    <id>:
      <config>
cribl/pipelines/route.yml:
  id: default
  groups: {}
  comments: []
  routes:
    ...
```

For details about each file's syntax, see [Config Files](configuration-files) and its child topics.
    
## Adding Fallback Leaders

You can configure [fallback Leader Nodes](deploy-add-second-leader) to support high availability and failover.
Use the following environment variables.

| Name | Purpose |
| --- | --- |
| `CRIBL_DIST_MASTER_RESILIENCY=failover` | Sets the Leader's `Resiliency` to `Failover` mode.|
| `CRIBL_DIST_MASTER_FAILOVER_VOLUME=/tmp/shared` | Sets the location of the NFS directory to support Leader failover.|
| `CRIBL_DIST_MASTER_FAILOVER_MISSED_HB_LIMIT` | Determines how many Lease refresh periods elapse before the standby Nodes attempt to promote themselves to primary. Cribl recommends setting this to `3`.|
| `CRIBL_DIST_MASTER_FAILOVER_PERIOD`| Determines how often the primary Leader refreshes its hold on the Lease file. Cribl recommends setting this to `5s`.|
| `CRIBL_INSTANCE_HOME` | In `Failover` mode, this variable points to the Leader Node's `root` directory, as opposed to the shared volume. It is used to access `$CRIBL_INSTANCE_HOME/local/_system/instance.yml`  (`C:\ProgramData\Cribl\local\_system.yml` for Cribl Edge on Windows). |

## GitOps Variables

Cribl Stream provides the following environment variables to facilitate using [GitOps](/stream/gitops) in Cribl Stream.

You can use these variables to override the defaults in the UI
if you are not using systemd to manage the `cribl` service.

### Bootstrap Variables

| Name                                 | Purpose                |
| ------------------------------------ | ---------------------- |
| `CRIBL_GIT_REMOTE`                   | Location of the remote repo to track. Can contain username and password for HTTPS auth. |
| `GIT_SSH` / `GIT_SSH_COMMAND`        | See [Git's documentation](https://git-scm.com/docs/git#Documentation/git.txt-codeGITSSHcode).
| `CRIBL_GIT_BRANCH`                   | Git ref (branch, tag, commit) to track/check out. |
| `CRIBL_GIT_AUTH`                     | One of: `none`, `basic`, or `ssh`. |
| `CRIBL_GIT_USER`                     | Used for `basic` auth. |
| `CRIBL_GIT_PASSWORD`                 | Used for `basic` auth. |
| `CRIBL_GIT_OPS`                      | Controls which GitOps workflow to use. One of: `push` to enable the GitOps push workflow, or `none` to disable GitOps. |
| `CRIBL_INTERACTIVE`                  | Controls whether git commands called by Cribl CLI at startup are interactive.|
| `CRIBL_GIT_SSH_KEY`                  | Content of the SSH key used to access git remote. |
| `CRIBL_GIT_STRICT_HOST_KEY_CHECKING` | Controls whether to check the host key strictly. |

## Additional Environment Variables

Cribl Stream uses the following variables for multiple purposes.

| Name | Purpose |
| --- | --- |
| `CRIBL_SPOOL_DIR` | Specifies the base path where events from various Sources and Destinations are spooled. Defaults to `$CRIBL_HOME/state/spool` or `$CRIBL_VOLUME_DIR/state/spool`. | 

### `CRIBL_HOME`

`CRIBL_HOME` is a special internal environment variable that indicates the location of the Cribl binary (`bin` directory).
You do not set it manually, but it is referred to in multiple places and commands.
