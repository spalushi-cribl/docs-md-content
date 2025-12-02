# Configure Fleets and Deploy Changes


Learn how to configure Fleets and Subfleets, then commit and deploy the changes.

---

Cribl Edge provides you with the ability to configure Fleet and Subfleets. At
the Fleet and Subfleet level, you can add, update, and remove:

- Sources
- Destinations
- Routes
- Pipelines
- Packs

To access these configuration options:

- Click the **Options (•••)** menu on a Fleet/Subfleet, then click
  **Configure**. This takes you to **Collect**, where you can use
  [QuickConnect](quickconnect) to quickly set up data collection Sources and
  hook them up to Destinations.
- Click a Fleet **Name**, then click **Collect** or use the **More** drop-down.

When you make changes to a Fleet, you must commit and deploy the changes in order
for the Edge Nodes to receive the updated configs. 

## Example Configuration Deployment Workflow

Here's a typical workflow for deploying Edge configurations:

1. Make configuration changes on a Fleet.
2. Save your changes.
3. Commit (and optionally push).
4. Locate your desired Fleet and click **Deploy**.

Once you save and commit your configuration changes, you can deploy the updated
configs to the Edge Nodes. You deploy new configurations at the Fleet level.

### Deploy Subfleets

To make new configurations visible and available to Subfleets, you must first
**Commit** the changes within parent Fleets' configurations.

When you commit changes to a parent Fleet, you have the option to
**Deploy Subfleets** at the same time.

When you **Deploy Subfleets**, the Subfleets will receive the new inherited
configuration. For guidelines on deploying to a large number of Edge Nodes, see
[Leader Scalability](deploy-planning#scaling).

![Deploy Subfleets](ed-deploy-fleets-4.9.png)
{border="true"}

Edge Nodes that belong to the Fleet/Subfleet will start **pulling** updated
configurations on their next check-in with the Leader.

> ##### Can't log in to the Edge Node as Admin User? 
> When an Edge Node pulls its first configs, the admin password will be
> randomized, unless specifically changed. This means that users won't be able
> to log in on the Edge Node with default credentials. For details, see
> [User Authentication](/stream/authentication#worker_pwd).
{.box .warning}

## Configuration Files

On the Leader, a Fleet's configuration resides in this directory: 

`$CRIBL_HOME/groups/<FleetName>/local/edge/`

Where each Fleet gets its own directory that contains a collection of `.yml`
config files. 

The managed Edge Node pulls configs and extracts them under: 

`$CRIBL_HOME/local/edge/`.

> Some configuration changes will require restarts, while many others will
> require only reloads. For details, see
> [Configurations and Restart](configuration-files#restart). Upon restart,
> individual Edge Nodes might temporarily disappear from the Leader's **Nodes**
> tab, before reappearing.
{.box .info}

## Notable Environment Variables for a Fleet {#environment-variables}

* `CRIBL_DIST_LEADER_URL` – URL of the Leader Node. Format: `<tls|tcp>://<authToken>@host:port?group=default_fleet&tag=tag1&tag=tag2&tls.<tls-settings below>`. 
 
  Example: `CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`

  Parameters:

   * `group` – The preferred Fleet assignment.
   * `resiliency` – The preferred Leader failover mode. 
   * `volume`– The location of the NFS directory to support Leader failover. 
   * `tag` – A list of tags that you can use to [assign](#mapping) the Edge Node to a Fleet.
   * `tls.privKeyPath` – Private Key Path.
   * `tls.passphrase` – Key Passphrase.
   * `tls.caPath` – CA Certificate Path.
   * `tls.certPath` – Certificate Path.
   * `tls.rejectUnauthorized` – Validate Client Certs. Boolean, defaults to `false`.
   * `tls.requestCert` – Authenticate Client (mutual auth). Boolean, defaults to `false`.
   * `tls.commonNameRegex` – Regex matching peer certificate > subject > common names allowed to connect. Used only if `tls.requestCert` is set to `true`.

* `CRIBL_DIST_MODE` – Set to one of: `managed-edge` (managed Node), `leader` (Leader instance), or `edge` (single instance). 
* `CRIBL_HOME` – Auto setup on startup. Defaults to parent of `bin` directory.
* `CRIBL_CONF_DIR` – Auto setup on startup. Defaults to parent of `bin` directory.
* `CRIBL_DIST_LEADER_BUNDLE_URL` – AWS S3 bucket (format: `s3://${bucket}`) for [remote bundle storage](managing-edge-nodes#remote-bundle).
* `CRIBL_TMP_DIR` – Defines the root of a temporary directory. {{< id `cribl-tmp-dir` >}}
  
   Sources use this directory to stage downloaded Parquet data. The format of derived temporary directories is: `$CRIBL_TMP_DIR || os.tmpdir())/cribl_temp[/<componentPath>/<PID>]`  
   ... where: `<componentPath>` = `<inputId>`; `<PID>` = Worker Process ID; and `os.tmpdir()` defaults to `/tmp` on Linux. Override that default using this `CRIBL_TMP_DIR` environment variable.

* `CRIBL_VOLUME_DIR` – Sets a directory that persists modified data between different containers or ephemeral instances.
* `CRIBL_DIST_WORKER_PROXY` - Communicate to the Leader Node via a SOCKS proxy. See [`CRIBL_DIST_WORKER_PROXY`](environment-variables#cribl_dist_worker_proxy) for more details.
