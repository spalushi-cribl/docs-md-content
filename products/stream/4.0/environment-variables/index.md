# Environment Variables


This is a consolidated list of environment variables available to configure Cribl Stream instances.

---

## Distributed Deployment 

You can use the following environment variables to configure your distributed Cribl Stream instance. 

| **Name** | **Purpose** |
| --- | --- |
| `CRIBL_DIST_LEADER_URL` |URL of the Leader Node. Example: `CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`. See [Formatting Notes](#format) below.| 
| `CRIBL_DIST_MODE` |`worker` or  `master`. Defaults to `worker`, if (and only if) `CRIBL_DIST_LEADER_URL` is present.|
| `CRIBL_HOME` |  Auto setup on startup. Defaults to parent of `bin` directory. |
| `CRIBL_CONF_DIR` |  Auto setup on startup. Defaults to parent of `bin` directory.|

| `CRIBL_TMP_DIR` | Defines the root of a temporary directory. See [Formatting Notes](#format) below. |
| `CRIBL_VOLUME_DIR` | Sets a directory that persists modified data between different containers or ephemeral instances. |
| `CRIBL_DIST_WORKER_PROXY`     | Communicate to the Leader Node via a SOCKS proxy.See [Formatting Notes](#format) below.|
| `CRIBL_BOOTSTRAP`     | Quickstart a Cribl instance by configuring this variable.|
| `CRIBL_BOOTSTRAP_HOST`   | Host name for connecting to the Leader Node when setting up a new Worker Node. This variable is not related to `CRIBL_BOOTSTRAP`.|
| `CRIBL_USERNAME` | Used to log in or out of Cribl.|
| `CRIBL_PASSWORD` | Used to log in or out of Cribl.|
| `CRIBL_HOST` | The Host URL for authentication. Example: `CRIBL_HOST=<url> CRIBL_USERNAME=<username> CRIBL_PASSWORD=<password> $CRIBL_HOME/bin/cribl auth login`|


### Usage Notes {#format}

This section explains how to use certain complex environment variables.

#### `CRIBL_DIST_LEADER_URL`
  
Use this format:

`<tls|tcp>://<authToken>@host:port?group=defaultGroup&tag=tag1&tag=tag2&tls.<tls_settings>`
  
Here are the components:

* `group` – The preferred Worker Group assignment.
* `resiliency` – The preferred Leader failover mode. 
* `volume`– The location of the NFS directory to support Leader failover. 
* `tag` – A list of tags that you can use to assign ([Stream](/stream/deploy-distributed#mapping), [Edge](/edge/fleets#mapping)) the Worker to a Worker Group.
* `tls.privKeyPath` – Private Key Path.
* `tls.passphrase` – Key Passphrase.
* `tls.caPath` – CA Certificate Path.
* `tls.certPath` – Certificate Path.
* `tls.rejectUnauthorized` – Validate Client Certs. Boolean, defaults to `false`.
* `tls.requestCert` – Authenticate Client (mutual auth). Boolean, defaults to `false`.
* `tls.commonNameRegex` – Regex matching peer certificate > subject > common names allowed to connect. Used only if `tls.requestCert` is set to `true`.
  
> `CRIBL_DIST_LEADER_URL` was previously called `CRIBL_DIST_MASTER_URL`.
> This name is still in use in certain contexts, for example in Helm charts.
>
{ .box .info }

#### `CRIBL_TMP_DIR`

Sources use this variable to construct temporary directories in which to stage downloaded Parquet data. If `CRIBL_TMP_DIR` is not set (the default), Cribl applications create subdirectories within your operating system's default temporary directory:
* For Cribl Stream: `<OS_default_temporary_directory>/stream/`.
* For Cribl Edge: `<OS_default_temporary_directory>/edge/`.

For example, on Linux, Stream's default staging directory would be `/tmp/stream/`. 

If you explicitly set this `CRIBL_TMP_DIR` environment variable, its value replaces this OS-specific default parent directory.

#### `CRIBL_DIST_WORKER_PROXY`

Use the format `<socks4|socks5>://<username>:<password>@<host>:<port>`. Only `<host>:<port>` are required. 
  
The default protocol is `socks5://`, but you can specify `socks4://proxyhost:port` if needed. 
  
To authenticate on a SOCKS4 proxy with username and password, use this format: `username:password@proxyhost:port`. The `proxyhost` can be a `hostname`, `ip4`, or `ip6`.

#### `CRIBL_BOOTSTRAP_HOST`

`CRIBL_BOOTSTRAP_HOST` overrides the [Add/Bootstrap New Worker](/stream/deploy-workers#add-bootstrap) script generator's default hostname.

For example, if you set `CRIBL_BOOTSTRAP=myhost`, then `myhost` will appear in the script modal's **Leader hostname/IP** field, instead of the URL used in the browser.

#### `CRIBL_BOOTSTRAP`

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
    
## Adding a Second Leader Node

You can configure a [second Leader Node](deploy-add-second-leader) via the following environment variables. 

| **Name** | **Purpose** |
| --- | --- |
| `CRIBL_DIST_MASTER_RESILIENCY=failover` | Sets the Leader's `Resiliency` to `Failover` mode.|
| `CRIBL_DIST_MASTER_FAILOVER_VOLUME=/tmp/shared` | Sets the location of the NFS directory to support Leader failover.|
| `CRIBL_DIST_MASTER_FAILOVER_MISSED_HB_LIMIT` | Determines how many Lease refresh periods elapse before the standby Nodes attempt to promote themselves to primary. Cribl recommends setting this to `3`.|
| `CRIBL_DIST_MASTER_FAILOVER_PERIOD`| Determines how often the primary Leader refreshes its hold on the Lease file. Cribl recommends setting this to `5s`.|
| `CRIBL_INSTANCE_HOME` | In `Failover` mode, this variable points to the Leader Node's `root` directory, as opposed to the shared volume. It is used to access `$CRIBL_INSTANCE_HOME/local/_system/instance.yml`  (`C:\ProgramData\Cribl\local\_system.yml` for Cribl Edge on Windows). Outside of `Failover` mode, it's the same value as  `CRIBL_CONF_DIR`.|


## GitOps 

Cribl Stream provides the following environment variables to facilitate GitOps.

### Bootstrap Variables

| **Name** | **Purpose** |
| --- | --- |
| `CRIBL_GIT_REMOTE` | Location of the remote repo to track. Can contain username and password for HTTPS auth. |
| `GIT_SSH / GIT_SSH_COMMAND` | See [Git's documentation](https://git-scm.com/docs/git#Documentation/git.txt-codeGITSSHcode).
| `CRIBL_GIT_BRANCH` | Git ref (branch, tag, commit) to track/check out. |
| `CRIBL_GIT_AUTH` | One of: `none`, `basic`, or `ssh`. |
| `CRIBL_GIT_USER` | Used for `basic` auth. |
| `CRIBL_GIT_PASSWORD` | Used for `basic` auth. |
| `CRIBL_GIT_OPS` | One of: `push` to enable the GitOps push workflow, or `none` to disable GitOps. |
| `CRIBL_GIT_SSH_KEY` | Content of the SSH key used to access git remote. |
| `CRIBL_GIT_STRICT_HOST_KEY_CHECKING` | Boolean flag sets whether to check the host key strictly.|
| `CRIBL_INTERACTIVE`  | Controls whether git commands called by Cribl CLI at startup are interactive.|


## Internal Environment Variables {#internal}

Cribl Stream uses the following variables internally. 

| **Name** | **Purpose** |
| --- | --- |
| `CRIBL_WORKER_ID` | Passed to Worker processes. |
| `CRIBL_GROUP_ID` | Passed to ConfigHelper processes to identify Worker Groups. |
| `CRIBL_ROLE` | Controls the behavior of a Cribl subprocess, e.g., `LEADER`, `WORKER`, `CONFIG_HELPER`. |
| `CRIBL_SERVICE` |Set to 1 when using **systemd** to start Cribl at boot time. |
| `CRIBL_SERVICE_NAME`  | Set to `cribl` when using **systemd** to start Cribl at boot time. |
| `CRIBL_SERVICEACCOUNT_PATH` | Path to the `ServiceAccount` to use to query the Kubernetes API. Defaults to  `/var/run/secrets/kubernetes.io/serviceaccount`. |
| `CRIBL_K8S_FOOTGUN` | Set to `true` to enable resource-intensive, potentially risky modes of the [Kubernetes Metrics](/edge/sources-kubernetes-metrics#cluster) and [Kubernetes Logs](/edge/sources-kubernetes-logs#cluster) Sources.|
| `CRIBL_K8S_POD` | Sets the name of the Kubernetes Pod in which Cribl Edge is deployed. |
| `CRIBL_AUTO_PORTS` | When set to `true`, allows the Cribl process to listen to the first open port, if the designated API port is taken. |
|`CRIBL_K8S_TLS_REJECT_UNAUTHORIZED`| Set to `0` to disable certification validation when connecting to the Kubernetes APIs. When you disable this environment variable, all Kubernetes features (including Metadata, Metrics, Logs, and AppScope metadata) will tolerate invalid TLS certificates (i.e., expired, self-signed, etc.) when connecting to the Kubernetes APIs.|
| `CRIBL_EDGE_FS_ROOT` | Location of the host OS' filesystem when mounting in a container. Defaults to `/hostfs`. |
| `CRIBL_EDGE` | When set to any value, runs this command at container start: `cribl mode‑edge ‑H 0.0.0.0`. This launches the instance as an Edge Node, listening on a Host at `0.0.0.0`. |

