# Fleets


Fleets & their corresponding Subfleets allow you to author and manage configuration settings for a particular set of Edge Nodes. The Cribl Edge UI offers you the flexibility to group edge nodes into logical Fleets and Subfleets so they can share and reuse configurations. 

To create a new Fleet, first click the top nav's **Manage** tab. Then, from the resulting **Fleets** lower tab, click **New Fleet**.

To add a new Fleet, enter a descriptive **Fleet Name**, and optionally, a relevant **Description**. To enable teleporting to the Edge Nodes via the Leader's UI, set **UI access** to **Yes**, then click **Save**.

## Subfleets & Inheritance Configurations 

You can organize your Fleets and Subfleets into a hierarchy of configuration layers from the top level. At the Fleet level, this might include grouping Edge Nodes with basic configurations like common logging locations, metrics, as well as common Sources and Destinations. At the Subfleet level, you can group Edge Nodes to pick up configurations specific to the applications and services that are running on the nodes. 

Layering the configurations allows you to manage a large number of Edge Nodes that share common yet differentiated configurations. For example, as an administrator, you might need multiple layers of configurations to manage the following `OS → DataCenter → Department → Application`. In this case, the Edge Nodes at the `Application` level can inherit and share configurations from the `OS` level. You can update a parent-level configuration (Fleet) and have it applied to all Subfleets and their respective Edge Nodes, reducing the time and effort needed to manage them.

To add a Subfleet, enter a descriptive **ID**. Next, use the **Inherited Config** drop-down to select the existing Fleet from which it will inherit its configuration. Then click **Save**.

You can also add a new Subfleet from a parent Fleet, by clicking its **Actions** column's **New Subfleet** button

![Add a new Fleet](ed-new-fleet.c07c6440ab.png)
{scale="60%" border="true"}

You can similarly create a child Fleet in a selected Subfleet. The maximum depth for inheriting configurations is 5 levels, at which point the **New Subfleet** button will be disabled. 

![Subfleet's maximum depth](ed-max-depth-subfleet.bc25dea4c0.png)
{border="true"}



In a Fleet's or Subfleet's **Details** column, click **View** to display the Fleet's number of Routes, Pipelines, Sources, and Destinations.

![Configure, Clone, and Delete Fleets and Subfleets](ed_fleet_view.ebc7e16c9a.png)
{border="true"}

> On Cribl.Cloud, you'll also see a **Group Type** column, showing whether the deployment is **Hybrid** or **Cloud**. 
>
{.box .cloud}

Click the Fleet's or Subfleet's ••• (Options) menu to display the **Edit Fleet**, **[Configure](fleets#configuring)**, **Clone**, and **Delete** options. 

![Edit, Configure, Clone, and Delete Fleets and Subfleets](ed_fleet_delete.9190515abb.png)
{border="true"}

Cloning a Fleet or Subfleet displays the **Clone** modal. Cloning a Fleet copies the Fleet's settings, while cloning a Subfleet inherits the parent's configurations. 

![Clone a Subfleet](ed_clone_fleet.a5f6c61cee.png)
{scale="60%" border="true"}

To delete a Fleet or Subfleet, click the ••• (Options) menu's **Delete** option. You will be prompted to confirm your decision to delete the Fleet/Subfleet, click **Yes** to confirm. 

> Configuring multiple Fleets requires an Edge Enterprise or Standard [license](licensing).
>
{.box .success}


## Inheritance Limitations

The inheritance configurations are currently subject to the following limitations. We will update this section as we enable more features:
- Inherited Packs are not visible in the Packs list. You can access inherited Packs by selecting **More** > **Data Routes**, then opening a Route's **Pipeline** drop-down. 
- Inherited Lookups and Samples are not visible in their respective lists. 

However, when you deploy the configuration changes to the Edge Nodes, you can teleport into a Node and view the inherited Packs, Lookups, and Samples in their respective lists.

## Configuring a Fleet {#configuring}

Navigate to a Fleet/Subfleet, click the ••• (Options) menu to display the **Configure** option. Click **Configure** to display an interface for authoring and validating its configuration. You can configure everything for this Fleet as if it were a single Edge instance – using a similar visual interface.

> ##### Can't Log into the Edge Node as Admin User?
>
> To explicitly set passwords for Fleets, see [User Authentication](/stream/authentication#worker_pwd).
>
{.box .warning}

## Committing and Deploying Fleet Configurations

To make new configurations visible and available to Subfleets, you must first **Commit** the changes within parent Fleet configurations.

Deploying a parent Fleet does not automatically deploy the configuration changes to Subfleets. You must deploy each Subfleet separately.

After you commit a parent Fleet's new configuration, its Subfleets will receive the new inherited configuration after you deploy those Subfleets.

## Recovering a Fleet

If you delete a Fleet, you lose the context required to access the Version Control menu. As a result, you also lose the option to recover the Fleet before it was deleted. 

You can recover a deleted Fleet, using one of the following two methods:

- [Creating a New Fleet with the Same Name](#newfleet)
- [Temporarily Stopping the Cribl Server](#stopping)

### Creating a New Fleet with the Same Name {#newfleet}

1. Create a new Fleet with the same name as the one that was deleted. This action will restore the deleted Fleet to the system. 

1. Select a previous version of the deleted Fleet from the Version Control menu if you previously committed the deletion of your Fleet.

If you did not previously commit the deletion of your Fleet, the system will automatically restore it.

### Temporarily Stopping the Cribl Server {#stopping}

1. Stop the Cribl server.

1. Identify the last commit before the Fleet was deleted.

1. Revert to that commit.

1. Restart the Cribl server.

## Mapping Edge Nodes to Fleets {#mapping}

Mapping Rulesets are used to map Edge Nodes to Fleets. Within a ruleset, a list of rules evaluate Filter expressions on the information that Edge Nodes send to the Leader. 

**Only one Mapping Ruleset can be active at any one time, although a ruleset can contain multiple rules. At least one Fleet should be defined and present in the system.**

In a ruleset the order of Rules matters. The **Filter** section supports full JS expressions. The ruleset matching strategy is first-match, and one Edge Node can belong to only one Fleet.


## Managing Edge Nodes on Multiple Platforms {#platforms}

The Leader is unaware of Edge Nodes' platforms (i.e, Linux or Windows) within a Fleet. So the ConfigHelper omits platform-specific limitations. Therefore, when you manage Edge Nodes on heterogeneous platforms, create a Windows-specific Fleet and mapping. 

For mapping details, see the Mapping sections below. For what's supported on Windows, see [Cribl Edge on Windows](explore-edge-win).
 
### Creating a Mapping Ruleset

To create a Mapping Ruleset, Click **Manage** on the top nav, then click the **Mappings** tab. On the resulting page, click **New Ruleset**. Give the resulting **New Ruleset** a unique **ID** and click **Save**. Click your new ruleset's **Configure** button, and start adding rules by clicking on **+ Rule**. While you build and refine rules, the Preview in the right pane will show which currently reporting and tracked workers map to which Fleets.

A ruleset must be activated before it can be used by the Leader. To activate it, go to **Mappings** and click **Activate** on the required ruleset. The **Activate** button will then change to an **Active** toggle. Using the adjacent buttons, you can also **Configure** or **Delete** a ruleset, or **Clone** a ruleset if you'd like to work on it offline, test different filters, etc. 

Although not required, Edge Nodes can be configured to send a preferred Fleet name with their payload (found in payload's `cribl.group` key) See [below](#priority) how this ranks in mapping priority. 

> For platform-specific mapping, set the `platform` property to `win32` for Windows, or to `linux` for Linux.  
>
{.box .info}

### Add a Mapping Rule – Example {#example}

Within a Mapping Ruleset, click **+ Add Rule** to define a new rule. Assume that you want to define a rule for all hosts that satisfy this set of conditions: 

* IP address starts with `10.10.42`, AND:
* More than 6 CPUs OR `CRIBL_HOME` environment variable contains `DMZ`, AND:
* Belongs to `Fleet420`.

#### Rule Configuration

* **Rule Name**: `myFirstRule`
* **Filter**: `(conn_ip.startsWith('10.10.42.') && cpus > 6) || env.CRIBL_HOME.match('DMZ')`
* **Fleet**: `Fleet420`

### Default Fleet and Mapping

When an Edge instance runs as Leader, the following are created automatically: 

- A `default_fleet` Fleet.
- A `default` Mapping Ruleset,

### Mapping Order of Priority {#priority}

Priority for mapping to a fleet is as follows: Mapping Rules > Fleet (`cribl.group`) sent by Edge Node > `default_fleet` Fleet.

  * If a Filter matches, use that Fleet.
  * Else, if an Edge Node has a Fleet defined, use that.
  * Else, map to the `default_fleet` Fleet.

## Deploying Configurations {#deploy} 

Your typical workflow for deploying Edge configurations is the following:

1. Work on configs.
2. Save your changes.
3. Commit (and optionally push).
4. Deploy.

Deployment is the last step after configuration changes have been saved and committed. **Deploying here means propagating updated configs to Edge Node.** You deploy new configurations at the Fleet level: Locate your desired Fleet and click on **Deploy**. Edge Nodes that belong to the Fleet will start **pulling** updated configurations on their next check-in with the Leader. 

> ##### Can't Log into the Edge Node as Admin User?
>
> When an Edge Node pulls its first configs, the admin password will be randomized, unless specifically changed. This means that users won't be able to log in on the Edge Node with default credentials. For details, see [User Authentication](/stream/authentication#worker_pwd).
>
{.box .warning}

## Configuration Files

On the Leader, a Fleet's configuration resides under: 
`$CRIBL_HOME/groups/<FleetName>/local/edge`.

On the managed Edge Node, after configs have been pulled, they are extracted under: `$CRIBL_HOME/local/edge/`.

> Some configuration changes will require restarts, while many others will require only reloads. For details, see [Configurations and Restart](configuration-files#restart). Upon restart, individual Edge Nodes might temporarily disappear from the Leader's **Nodes** tab, before reappearing.
>
{.box .info}

## Environment Variables {#environment-variables}

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

  * `CRIBL_DIST_MODE` – Set to one of: `managed-edge` (managed Node), `master` (Leader instance), or `edge` (single instance). 
  * `CRIBL_HOME` – Auto setup on startup. Defaults to parent of `bin` directory.
  * `CRIBL_CONF_DIR` – Auto setup on startup. Defaults to parent of `bin` directory.
  
  * `CRIBL_TMP_DIR` – Defines the root of a temporary directory. {{< id `cribl-tmp-dir` >}}
  
     Sources use this directory to stage downloaded Parquet data. The format of derived temporary directories is: `$CRIBL_TMP_DIR || os.tmpdir())/cribl_temp[/<componentPath>/<PID>]`  
     ... where: `<componentPath>` = `<inputId>`; `<PID>` = Worker Process ID; and `os.tmpdir()` defaults to `/tmp` on Linux. Override that default using this `CRIBL_TMP_DIR` environment variable.

  * `CRIBL_VOLUME_DIR` – Sets a directory that persists modified data between different containers or ephemeral instances.
  * `CRIBL_DIST_WORKER_PROXY` - Communicate to the Leader Node via a SOCKS proxy. Format: `<socks4|socks5>://<username>:<password>@<host>:<port>`. Only `<host>:<port>` are required. 
  
    The default protocol is `socks5://`, but you can specify `socks4://proxyhost:port`if needed. 
  
    To authenticate on a SOCKS4 proxy with username and password, use this format: `username:password@proxyhost:port`. The `proxyhost` can be a `hostname`, `ip4`, or `ip6`. 

## Edge Node GUID

When you install and first run the software, a GUID is generated and stored in a `.dat` file located in `CRIBL_HOME/local/cribl/auth/`, for example:

`# cat CRIBL_HOME/local/cribl/auth/676f6174733432.dat`

`{"it":1570724418,"phf":0,"guid":"48f7b21a-0c03-45e0-a699-01e0b7a1e061"}`

When deploying Cribl Edge as part of a host image or VM, be sure to remove this file, so that you don't end up with duplicate GUIDs. The file will be regenerated on next run.
