# Fleets


Fleets and their corresponding Subfleets allow you to author and manage
configuration settings for a particular set of Edge Nodes, so you can flexibly
group Edge Nodes into logical Fleets and Subfleets. These Fleets and Subfleets
can share and reuse configurations. 

With Cribl Edge v.4.2.x and later, you can assign Fleet-level permissions to
users. For details, see [Members and Permissions](members).

## Creating a Fleet

To create a new Fleet:

1. Click the **Manage** tab in the top nav. 
1. In the **Fleets** lower tab, click **New Fleet**.
1. Enter a descriptive **Fleet name**, and optionally, a relevant
**Description**. To enable teleporting to the Edge Nodes via the Leader's UI,
toggle **Enable teleporting to Nodes** to **Yes**, then click **Save**.

![Add a new Fleet](new-fleet.3611827c77.png)
{scale="75%" border="true"}

## Subfleets and Inheritance Configurations 

You can organize your Fleets and Subfleets into a hierarchy of configuration
layers from the top level down. At the Fleet level, this might include grouping
Edge Nodes with basic configurations like common logging locations, metrics, as
well as common Sources and Destinations. At the Subfleet level, you can group
Edge Nodes to pick up configurations specific to the applications and services
that are running on the Nodes. 

There are several ways you can build the hierarchy:

- Organizationally. 
- Geographically (if your organization collects different data categories in different regions).
- Data center–based.
- OS-based.

Layering the configurations allows you to manage a large number of Edge Nodes
that share common yet differentiated configurations. For example, as an
administrator, you might need multiple layers of configurations to manage the
following `Org > Department > OS > Application`. In this case, the Edge Nodes at
the `Application` level can inherit and share configurations from the `Org`
level. You can update a parent-level configuration (Fleet) and have it applied
to all Subfleets and their respective Edge Nodes, reducing the time and effort
needed to manage them. For guidance, see our better practices doc: [Fleets
Hierarchy and Design](fleets-design).

### Adding a Subfleet

To add a Subfleet:

1. In the row of your desired Fleet, click **Add Subfleet** in the **Actions** column.
1. Enter a descriptive **Fleet name** and optional **Description**.
1. Select an existing Fleet from the **Inherited configuration** drop-down. The
   Subfleet will inherit its configuration from the selected Fleet. 
1. Click **Save**.

You can also add a new Subfleet from a parent Fleet by clicking the
**Add Subfleet** button in the **Actions** column. 

![Add a Subfleet](new-subfleet.dd2244e0a1.png)
{scale="75%" border="true"}

Similarly, you can create a child Fleet in a selected Subfleet. Subfleets can
inherit configurations to five levels, at which point the **Add Subfleet**
button will be disabled. 

![Subfleet's maximum depth](ed-max-depth-subfleet.c8615174e8.png)
{border="true"}



### Viewing Subfleet Details and Options

To display the Fleet's number of Routes, Pipelines, Sources, and Destinations,
click **View** in a Fleet's or Subfleet's **Details** column. 

![View Subfleet Details](ed_fleet_view.1b05036f21.png)
{border="true"}

> On Cribl.Cloud, you'll also see a **Group Type** column, showing whether the deployment is **Hybrid** or **Cloud**. 
>
{.box .cloud}

To display the **Edit Fleet**, **[Configure](fleets#configuring)**,
**Clone**, and **Delete** options, click the Fleet's or Subfleet's ••• (Options)
menu. 

![Edit, Configure, Clone, and Delete Fleets and Subfleets](ed_fleet_delete.05d1a2e0c0.png)
{border="true"}

### Cloning and Deleting a Subfleet

Cloning a Fleet copies the Fleet's settings, while cloning a Subfleet inherits
the parent's configurations.

To clone a Subfleet, click its **Options (•••)** menu and select **Clone**.

![Clone a Subfleet](clone-fleet.b16de4b1a3.png)
{scale="60%" border="true"}

To delete a Fleet or Subfleet, click **Delete** in the **Options (•••)** menu.
You will be prompted to confirm your decision to delete the Fleet/Subfleet,
click **Yes** to confirm.

You can also commit and deploy changes to Fleets and Subfleets on this page. For
details, see [Committing and Deploying Fleet Configurations](#deploy).   

> Configuring multiple Fleets requires an Edge Enterprise or Standard [license](licensing).
>
{.box .success}

## Accessing Inherited Packs, Lookups, and Samples

Fleets with inherited configurations can also inherit Packs, Lookups, and
Samples. Here's how to access them:

- Access inherited Packs in **More** > **Data Routes**, then open the
  **Pipeline** drop-down in a Route. Inherited Packs don't appear in the Packs list.
- You can teleport into a Node and view Lookups and Samples in their respective
  lists when you deploy configuration changes to the Edge Nodes. 
  

## Configuring a Fleet {#configuring}

Navigate to a Fleet/Subfleet, click the **Options (•••)** menu to display the
**Configure** option. Click **Configure** to display an interface for authoring
and validating its configuration. You can configure everything for this Fleet as
if it were a single Edge instance – using a similar visual interface.

> ##### Can't Log into the Edge Node as Admin User?
>
> To explicitly set passwords for Fleets, see [User Authentication](/stream/authentication#worker_pwd).
>
{.box .warning}

## Committing and Deploying Fleet Configurations {#deploy}

To make new configurations visible and available to Subfleets, you must first
**Commit** the changes within parent Fleets' configurations.

On Cribl Edge v.4.1.x and later, when you commit changes to a parent Fleet, you
have the option to **Commit & Deploy with Subfleets**. When you
**Deploy with Subfleets**, the Subfleets will receive the new inherited
configuration. For guidelines on deploying to a large number of Edge Nodes, see
[Leader Scalability](deploy-planning#scaling).

![Deploy with Subfleets](ed-deploy-fleets-1.650a326473.png)
{border="true"}

## Recovering a Fleet

If you delete a Fleet, you lose the context required to access the Version
Control menu. As a result, you also lose the option to recover the Fleet before
it was deleted. 

You can recover a deleted Fleet using one of the following two methods:

- [Creating a New Fleet with the Same Name](#newfleet)
- [Temporarily Stopping the Cribl Server](#stopping)

### Creating a New Fleet with the Same Name {#newfleet}

1. Create a new Fleet with the same name as the one that was deleted. This
   action will restore the deleted Fleet to the system. 

1. Select a previous version of the deleted Fleet from the Version Control menu
   if you previously committed the deletion of your Fleet.

If you did not previously commit the deletion of your Fleet, the system will
automatically restore it.

### Temporarily Stopping the Cribl Server {#stopping}

1. Stop the Cribl server.

1. Identify the last commit before the Fleet was deleted.

1. Revert to that commit.

1. Restart the Cribl server.

## Mapping Edge Nodes to Fleets {#mapping}

Mapping Rulesets are used to map Edge Nodes to Fleets. Within a ruleset, a list
of rules evaluate Filter expressions on the information that Edge Nodes send to
the Leader. 

**Only one Mapping Ruleset can be active at any one time, although a ruleset can
contain multiple rules. At least one Fleet should be defined and present in the
system.**

In a ruleset, the order of Rules matters. The **Filter** section supports full JS
expressions. The ruleset matching strategy is first-match, and one Edge Node can
belong to only one Fleet.


## Managing Edge Nodes on Multiple Platforms {#platforms}

The Leader is unaware of Edge Nodes' platforms (i.e, Linux or Windows) within a
Fleet, which means the ConfigHelper omits platform-specific limitations.
Therefore, when you manage Edge Nodes on heterogeneous platforms, create a
Windows-specific Fleet and mapping.

For mapping details, see the Mapping sections below. For what's supported on
Windows, see [Cribl Edge on Windows](explore-edge-win).
 
### Creating a Mapping Ruleset

To create a Mapping Ruleset:

1. Click **Manage** on the top nav > **Mappings**. 
1. Click **Add Ruleset**. 
1. Give the **New Ruleset** a unique **ID** and click **Save**. 
1. Click the **Configure** button and start adding rules with **Add
Rule**. 

While you build and refine rules, the Preview in the right pane will
show which currently reporting and tracked workers map to which Fleets.

A ruleset must be activated before it can be used by the Leader. To activate it,
go to **Mappings** and click **Activate** on the required ruleset. The
**Activate** button will then change to an **Active** toggle. Using the adjacent
buttons, you can also **Configure** or **Delete** a ruleset, or **Clone** a
ruleset if you'd like to work on it offline, test different filters, etc. 

Although not required, Edge Nodes can be configured to send a preferred Fleet
name with their payload (found in payload's `cribl.group` key)
See [below](#priority) how this ranks in mapping priority. 

> For platform-specific mapping, set the `platform` property to `win32`
> for Windows, or to `linux` for Linux.  
>
{.box .info}

### Add a Mapping Rule – Example {#example}

Within a Mapping Ruleset, click **Add Rule** to define a new rule. Assume that
you want to define a rule for all hosts that satisfy this set of conditions: 

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

Priority for mapping to a fleet is as follows: Mapping Rules > Fleet
(`cribl.group`) sent by Edge Node > `default_fleet` Fleet.

  * If a Filter matches, use that Fleet.
  * Else, if an Edge Node has a Fleet defined, use that.
  * Else, map to the `default_fleet` Fleet.

## Deploying Configurations {#deploy} 

A typical workflow for deploying Edge configurations looks like this:

1. Work on configs.
2. Save your changes.
3. Commit (and optionally push).
4. Locate your desired Fleet and click **Deploy**.

Once you save and commit your configuration changes, you can deploy the updated
configs to the Edge Nodes. You deploy new configurations at the Fleet level.

### Deploying with Subfleets

On Cribl Edge v.4.1.x and later, when you commit changes to a parent Fleet, you
have the option to **Deploy with Subfleets**. 

When you **Deploy with Subfleets**, the Subfleets will receive the new inherited configuration. 

![Deploy with Subfleets](ed-deploy-fleets-1.650a326473.png)
{border="true"}

Edge Nodes that belong to the Fleet/SubFleet will start **pulling** updated
configurations on their next check-in with the Leader.

> Can't log in to the Edge Node as Admin User? When an Edge Node pulls
> its first configs, the admin password will be randomized, unless specifically
> changed. This means that users won't be able to log in on the Edge Node with
> default credentials. For details, see
> [User Authentication](/stream/authentication#worker_pwd). 
>
{.box .warning}

## Configuration Files

On the Leader, a Fleet's configuration resides under: 

`$CRIBL_HOME/groups/<FleetName>/local/edge`.

On the managed Edge Node, after configs have been pulled, they are extracted under: 

`$CRIBL_HOME/local/edge/`.

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


