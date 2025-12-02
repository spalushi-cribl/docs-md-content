# Creating and Managing Fleets and Subfleets


Learn how to create Fleets and Subfleets in Cribl Edge.

---

Fleets and their corresponding Subfleets allow you to author and manage
configuration settings for a particular set of Edge Nodes, so you can flexibly
group Edge Nodes into logical Fleets and Subfleets. These Fleets and Subfleets
can share and reuse configurations. 

In Cribl Edge v.4.2.x and later, you can assign Fleet-level permissions to
users. For details, see [Members and Permissions](members).

## Create a Fleet

**Required permission**: Cribl Edge Admin

To create a new Fleet:

1. In the sidebar, click **Fleets**. 
1. Click **New Fleet**.
1. Enter a descriptive **Fleet name**, and optionally, a relevant
  **Description** and [**Tags**](settings-group-fleet#fleet-configuration). 

  ![Add a new Fleet](new-fleet.png)
   {scale="75%" border="true"}

   To enable [teleporting to the Edge Nodes](explore-edge#teleport) via the Leader's UI, toggle 
   **Enable teleporting to Nodes** to **Yes**, then click **Save**.

## Add a Subfleet

You can create up to four levels of Subfleets nested below a parent Fleet, and choose
whether the Subfleet will inherit configurations from its parent Fleet.

To add a Subfleet:

1. In the row of your desired Fleet, click **Add Subfleet** in the **Actions** column.
1. Enter a descriptive **Fleet name** and optional **Description** and [**Tags**](settings-group-fleet#fleet-configuration).
1. Select an existing Fleet from the **Inherited configuration** drop-down. The
   Subfleet will inherit its configuration from the selected Fleet. 
1. Click **Save**.

You can also add a new Subfleet from a parent Fleet by clicking the
**Add Subfleet** button in the **Actions** column. 

![Add a Subfleet](new-subfleet.dd2244e0a1.png)
{scale="75%" border="true"}

Similarly, you can create a Subfleet in a selected Subfleet. You can add up
to four levels of Subfleets underneath the parent Fleet, and each Subfleet
will inherit the configurations of every parent Subfleet before it.

![Subfleet maximum depth](ed-max-depth-subfleet.c8615174e8.png)
{border="true"}



### View Subfleet Details and Options

To display the number of Routes, Pipelines, Sources, and Destinations in a Fleet,
click **View** in the **Details** column of a Fleet or Subfleet. 

![View Subfleet details](ed_fleet_view.1b05036f21.png)
{border="true"}

> On Cribl.Cloud, you'll also see a **Group Type** column, showing whether the deployment is **Hybrid** or **Cloud**. 
>
{.box .cloud}

To display the **Edit Fleet**, **[Configure](fleets#configuring)**, **Clone**,
and **Delete** options, click the **Options** (![](actions-dropdown.light.svg)) menu.

![Edit, Configure, Clone, and Delete Fleets and Subfleets](ed_fleet_delete.05d1a2e0c0.png)
{border="true"}

### Clone and Delete a Subfleet

Cloning a Fleet copies the Fleet's settings, while cloning a Subfleet inherits
the parent's configurations.

To clone a Subfleet, click its **Options** (![](actions-dropdown.light.svg)) menu and select **Clone**.

![Clone a Subfleet](clone-fleet.b16de4b1a3.png)
{scale="60%" border="true"}

To delete a Fleet or Subfleet, click **Delete** in the **Options (•••)** menu.
You will be prompted to confirm your decision to delete the Fleet/Subfleet,
click **Yes** to confirm.

You can also commit and deploy changes to Fleets and Subfleets on this page. For
details, see [Committing and Deploying Fleet Configurations](#deploy).   

> Configuring multiple Fleets is available on certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
>
{.box .success}

### Subfleet Inheritance Configurations 

You can organize your Fleets and Subfleets into a hierarchy of configuration
layers from the top level down. Subfleets will inherit all the configurations
from each parent Fleet that comes before it. At the Fleet level, this might
include grouping Edge Nodes with basic configurations like common logging
locations, metrics, as well as common Sources and Destinations. At the Subfleet
level, you can group Edge Nodes to pick up configurations specific to the
applications and services that are running on the Nodes. 

There are several strategies you can use to build Fleet hierarchy:

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

## Access Inherited Packs, Lookups, and Samples

Fleets with inherited configurations can also inherit Packs, Lookups, and
Samples. Here's how to access them:

- Access inherited Packs in **More** > **Data Routes**, then open the
  **Pipeline** drop-down in a Route. Inherited Packs don't appear in the Packs list.
- You can teleport into a Node and view Lookups and Samples in their respective
  lists when you deploy configuration changes to the Edge Nodes. 

## Save and Manage Certificates

Configure certificates at the Fleet level to securely connect your Cribl Edge to Sources and Destinations. For details, see [Setting Authentication on Sources/Destinations](securing-sources-dest).

Subfleets automatically inherit these certificates from their Parent Fleet, streamlining your deployment process. These certificates are then distributed to individual Edge Nodes. Certificates are stored locally on each Edge Node in the `local/edge/auth/certs/` directory.

To view and manage certificates, navigate to **Fleet Settings** > **Security** > **Certificates**.

All certificates from both the parent Fleet and Subfleet are visible in the Subfleet's certificate list.

## Edge Node GUID

When you install and first run the software, a GUID is generated and stored in a
`.dat` file located in `CRIBL_HOME/local/cribl/auth/`.

For example:

`# cat CRIBL_HOME/local/cribl/auth/676f6174733432.dat`

`{"it":1570724418,"phf":0,"guid":"48f7b21a-0c03-45e0-a699-01e0b7a1e061"}`

When deploying Cribl Edge as part of a host image or VM, be sure to remove this
file, so that you don't end up with duplicate GUIDs. The file will be
regenerated on the next run.


