# Packs



Packs enable Cribl Edge administrators and developers to pack up and share complex configurations and workflows across multiple Worker Groups, or across organizations. 

### Packs = Portability

With a Cribl Edge deployment of any size, using Packs can simplify and accelerate your work. Packs can also accelerate internal troubleshooting, and accelerate working with Cribl Support, because they facilitate quickly replicating your Cribl Edge environment. 

For example, where a Pipeline's configuration references Lookup file(s), Cribl Edge will import the Pipeline only if the Lookups are available in their configured locations. A Pack can consolidate this dependency, making the Pipeline portable across Cribl Edge instances. If you import the Lookups into the Pack, you can develop and test a configuration, and then port it from development to production instances, or readily deploy it to multiple Fleets.

We don't claim to have brokered world peace here, but we do modestly hope to promote a stable, prosperous Pax Criblatica for the Cribl Edge ecosystem.

### What Is a Pack?

Packs are implemented as a user interface (described on this page) and as a `.crbl` file format.

### What's in a Pack?

Currently, a Pack can pack up everything between a Source and a Destination:

  * Routes (Pack-level)
  * Pipelines (Pack-level)
  * Functions (built-in and custom)
  * Sample data files 
  * Knowledge objects (Lookups, Parsers, Global Variables, Grok Patterns, and Schemas)

![A Pack with internal Routes & Pipelines; no Knowledge or samples](pack-schematic.2f7cb911db.png)
{scale="90%" border="true"}

As the above list suggests, a Pack can encapsulate a whole set of infrastructure for a given use case.

> Packs have access only to [Knowledge objects](knowledge-library) (Lookups, Global Variables, and so forth)
> that are defined within the Pack itself.
> That means they can't access global Knowledge objects or those included in other Packs.
> If your Pack needs a Knowledge object that is defined elsewhere, you need to re-create it in the Pack.
{.box .warning}

### What's Not in a Pack?

Sources, Collectors, and Destinations are external to Packs, so you can't specify them within a Pack. This excludes a few other things:

- Routes configured within a Pack can't specify a Destination.
- Packs can't include Event Breakers, which are associated with Sources. (However, you can instead use the [Event Breaker Function](event-breaker-function) in Packs' contained Pipelines.)

You connect a Pack with a Source and Destination by attaching it to a Route (see [below](#where)), just as you'd attach a Pipeline.



### Where Can I Get Some Packs?

Easy now. See [The Cribl Packs Dispensary™](#dispensary) below.

## Using Packs

These instructions cover using predefined Packs, as well as creating and modifying Pack configurations.

> ##### Version Compatibility  {{< id `compatibility` >}}
>
> Packs created or modified in Cribl Edge 4.0.x cannot be used in any pre-4.0 version of Cribl Edge. (If you try, you'll see a `should NOT have additional properties` error.) To avoid this problem, Cribl recommends that you upgrade to Cribl Edge 4.0.x or later. 
> 
> For compatibility questions about any individual Packs, please contact us in Cribl [Community Slack](https://cribl-community.slack.com/)'s `#packs` channel.
>
{.box .warning}

###  Where Can I Use Packs?  {#where}

Wherever you can reference a Pipeline, you can specify a Pack:

- In Sources, where you attach pre-processing Pipelines.
- In Destinations, where you attach post-processing Pipelines.
- In Routes, in the Routing table's **Pipeline/Output** column.

This expanded view shows how a Pack can replace a Pipeline in a Route:

![A Pack snaps into Cribl Edge like an enhanced Pipeline](pack-context-exploded.c40453ad38.png)
{scale="90%" border="true"}

Packs are distinguished in the display with a **PACK** badge, as you can see here in the Routing table:

![PACKs badged in Routing table's Pipeline column](packs-in-routes.98d0cb8524.png)
{border="true"}

The **PACK** badge is also displayed when you click into a resource – shown here on one of the Routes from the above table:

![PACK badge on a Pack connected to a Route](pack-in-1-route.4d8326589e.png)
{scale="90%" border="true"}

Cribl Edge's **Monitoring** page includes a **Packs** link where you can monitor Packs' throughput.

### Accessing Packs

You access Packs differently, depending on your [deployment type](/stream/deploy-types).

#### Single-Instance Deployment {#single}

In a single-instance deployment, Packs are global. From Cribl Edge's top-level navigation, just select **Processing** > **Packs**. On the filesystem, Packs (including those that you add) are stored at `$CRIBL_HOME/default/`.

![Packs, single-instance navigation](packs-single-instance.f2e8fc48ed.png)
{scale="80%" border="true"}

#### Distributed Deployment {#group}

In a [distributed deployment](/stream/deploy-distributed), Packs are associated with (and installed within) Fleets. Select **Manage**, and then click into the Fleet you want to manage (for example, `default`). Next, from that Fleet's top nav, select **Processing** > **Packs** (Stream) or **More** > **Packs** (Edge).



![Stream Group > Manage Packs page](packs-worker-group.79cb5e0821.png)
{border="true"}

![Edge Fleet > Manage Packs page](packs-fleet.4cf2544db0.png)
{border="true"}

Each Group's Packs are stored at  at `$CRIBL_HOME/groups/<group-name>/default/`. (In a typical installation, you'll find the starter `HelloPacks` Pack at `/opt/cribl/groups/default/default/`.)

> By design, you can readily share Packs **across** Fleets by exporting/importing them (both operations covered below).
>
{.box .success}

### Getting Started with Packs  {#nav}

To unpack Packs, use the above instructions (per deployment type) to navigate to the **HelloPacks** example Pack shipped with Cribl Edge. On the **Manage Packs** page, click this Pack's row to see its configuration.

Click **Pipelines** on the Pack's submenu, and you'll see that the Pack includes `devnull`, `main`, and `passthru` Pipelines, corresponding to the default Pipelines provided at Cribl Edge's global level. This Pack also includes an Apache-specific sample Pipeline – click it to unpack that, too.

![Pipelines within example Pack](pack-sample-pipelines.23e239d3e1.png)
{border="true"}

Click **Routes** on the Pack's submenu, and you'll see that this Pack also provides both a `default` and an Apache-specific Route.

### Configuring a Pack  {#pack-configuration}

Once loaded, each Pack displays a submenu with familiar links: **Routes**, **Pipelines**, **Knowledge**, and **Settings** on the left pane, along with **Sample Data**, and **Preview Simple** on the right. The Pack's submenu is a subset of Cribl Edge's top nav.

![Configuring a Pack](se-pack-config.495596b977.png)
{border="true"}

The left pane's submenu links give you access to configuration objects specific to this Pack. 

### Sample Data {#sample-data}

The right pane defaults to displaying all sample data files available on your Cribl Edge instance. If you prefer to filter only sample files internal to the Pack, toggle **In Pack only** to the right.

If you add sample data files via this Pack UI, they will be internal to that Pack. Each sample file here displays its own **In Pack** toggle on its row, which works as follows:

A light-blue toggle is locked, meaning that this sample file is internal to the Pack. It will export with the Pack. If you want to make this sample available across Cribl Edge, you'll need to also add it via the global right preview pane (accessed from **Routing** > **Data Routes** or **Processing** > **Pipelines**).

A grayed-out or dark-blue toggle means that this sample file is global to Cribl Edge. It is available to this Pack. Toggle this to `Yes` (dark blue) if you want the sample file to export along with the Pack.

![Sample file in Pack](se-in-pack-toggle-3.5.f4f9b70541.png)
{border="true"}

Basically, you can manipulate all the options here as you'd work with their big sister or brother in Cribl Edge's global navigation.

### Importing or Upgrading a Pack  {#import}

To import a new Pack, or an updated version of an existing Pack, from your filesystem: 

1. Navigate to the **Manage Packs** page.
1. Click **Add Pack** at the upper right.
1. Select your desired **Add**/**Import** source: [Dispensary](#dispensary), [File](#file), [URL](#url), or [Git repo](#git).
1. Follow the above links to details on each of these options.

![Importing a Pack](pack-import.25f6cd3d84.png)
{border="true"}

> ##### Custom Functions {{< id `custom-functions` >}}
>
> Packs can include Pipelines containing custom functions, which can (in turn) run arbitrary JavaScript. Before you install a Pack, make sure it comes from a provider you trust, such as the [Cribl Packs Dispensary](#dispensary) or your own organization.
> 
> As an additional protection layer, all Pack import modals provide an **Allow custom functions** toggle. In the toggle's default `No` position, if Cribl Edge detects custom functions in the specified Pack, it will block the import with an error message. If you trust the Pack's provider,set the toggle to `Yes`, and the import will proceed normally.
>
{.box .warning}

#### The Cribl Packs Dispensary™  {#dispensary}

You might be wondering, "Where can I find a reliable source of Packs that add useful features to Cribl Edge, vetted for safety?"

Well, Cribl is proud to point you to the [Cribl Packs Dispensary™](https://packs.cribl.io/). Here, Cribl's own solutions engineers have seeded several strains of high-productivity Cribl Edge configurations. Because the Packs Dispensary™ is a place to share good stuff, we expect many new hybrids to sprout from the community. Cribl will test and curate submissions to ensure the quality of the repo's contents.

You can install Dispensary Packs directly through Cribl Edge's UI, as outlined in [Add from Packs Dispensary](#score) below.

![Cribl Packs Dispensary™ (as displayed in Cribl Edge's **Add** drawer)](se-packs-dispensary-drawer.efcfb4a33b.png)
{border="true"}

> Interested in publishing your own Packs on the Cribl Packs Dispensary™? See [Publishing a Pack](packs-standards#publish).
>
{.box .success}

#### Add from Packs Dispensary  {#score}

To add a Pack from the Cribl Packs Dispensary™ sharing site:

1. From the **Manage Packs** page's **New Pack** submenu, select **Add from Dispensary**.
1. The [Packs Dispensary](https://packs.cribl.io/) will open in a drawer, as shown in the screenshot [above](#dispensary).
1. Using the drawer's controls, browse or search for the Pack(s) you want. (You can use the check boxes at the left to filter by data type, use case, and technology.)
1. Click any Pack's tile to display its details page with its README. This will typically outline the Pack's purpose, compatibility, requirements, and installation.
1. To proceed, click **Add Pack** on this page.
1. That's it! You'll see a banner confirming that the Pack is now installed.

#### Import from File  {#file}

To import a Pack (`.crbl` file) from your local filesystem:

1. From the **New Pack** submenu, select **Import from File**.
1. From the resulting File Open dialog, select the file to import.
1. Optionally, give the pack an explicit, unique **New Pack ID**. (For details about this option, see [Upgrading an Existing Pack](#upgrading) below.)
1. Where appropriate (see just [above](#custom-functions)), enable **Allow custom functions**.  
1. Click **OK** to confirm the import.

![Importing from a file](pack-file-import.0cd46205e1.png)
{scale="70%" border="true"}

#### Import from URL  {#url}

To import a Pack from a known, public or internal, URL:

1. From the **New Pack** submenu, select **Import from URL**.
1. Enter a valid URL for the Pack's source. (This field's input is validated for URL format, but not for accuracy, before you submit the modal.)
1. Optionally, give the pack an explicit, unique **New Pack ID**. (See [Upgrading an Existing Pack](#upgrading).)
1. Where appropriate, enable **Allow custom functions**. (See [Custom Functions](#custom-functions).)
1. Click **OK** to confirm the import.

![Confirming file import from URL](pack-url-import.3c7077a066.png)

> To import a Pack from a public URL, Cribl Edge's Leader Node (or single instance) requires Internet access. A [distributed deployment](/stream/deploy-distributed)'s Leader can then deploy the Pack to Workers even if the Workers lack Internet access.
>
{.box .success}

####  Import from Git Repos  {#git}

To import a Pack from a known public or private Git repo:

1. From the **New Pack** submenu, select **Import from Git**.
1. Enter the source repo's valid URL. <br/>
  This field's input is validated for URL format, but not for completeness or accuracy, before you submit the modal. When targeting a private repo, use the format: `https://<username>:<token/password>@<repo‑address>`. Public repos need only `https://<repo‑address>`, as shown in the example below.

1. Optionally, give the pack an explicit, unique **New Pack ID**. (See [Upgrading an Existing Pack](#upgrading).)
1. Optionally, enter a **Branch or tag** to filter the import source using the repo's metadata. You can specify a branch (such as `master`) or a tag (such as a release number: `0.5.1`, etc.).
1. Where appropriate (see [Custom Functions](#custom-functions)), enable **Allow custom functions**.  
1. Click **OK** to confirm the import.

![Importing from a Git repo](pack-git-import-tag.b29425fb8b.png)
{scale="70%" border="true"}

> To import a Pack from a public repo, Cribl Edge's Leader Node (or single instance) requires Internet access. A [distributed deployment](/stream/deploy-distributed)'s Leader can then deploy the Pack to Workers even if the Workers lack Internet access.
>
{.box .success}

####  Dispensary GitHub Repo  {#repo}

One authoritative public repo is the [Cribl Pack Dispensary](https://github.com/criblpacks) on GitHub. (This is the precursor to the Cribl-hosted [Cribl Packs Dispensary™](#dispensary) site.)

You can install Dispensary Packs directly through Cribl Edge's UI, as outlined in [Import from Git Repos](#git) above. However, if you prefer, you can click through to any Dispensary repo's release page, download the corresponding `.crbl` file, and then [upload the file](#file) into Cribl Edge.

![Downloading a `.crbl` file from the Cribl Pack Dispensary's Web UI](packs-repo-download.52d562c6fe.png)
{border="true"}

> If you've posted completed Packs to our GitHub repo, we encourage you to now submit them to our new Cribl Packs Dispensary™ site. See [Publishing a Pack](packs-standards#publish).
>
{.box .success}



####  Upgrading an Existing Pack  {#upgrading}

Each Pack that is installed within a given Fleet (or single-instance deployment) must have a unique ID. The ID is based on the Pack's internal configuration – not its container's file name, nor on its Display name.

If you import a Pack whose internal ID matches an installed Pack – whether an update, or just a duplicate – you'll be prompted to assign a unique **New Pack ID** to import it as a separate Pack. 

![Renaming a Pack on import](pack-deconflict.a128f140f5.png)
{scale="70%" border="true"}

You'll also have the option to **Overwrite** the installed Pack, reusing the same ID.

> If you toggle this option to `Yes`, the imported Pack will completely overwrite your existing Pack's configuration.
> 
> Each Pack within a Cribl Edge instance must have a unique **Pack ID**, so you cannot share an ID between two (or more) installed Packs.
>
{.box .danger}

To explicitly upgrade an existing Pack, you can instead click the menu icon on its row and select **Upgrade**.

![Upgrading an existing Pack](pack-upgrade.c7db59b5da.png)
{border="true"}



> If you've modified an installed Pack, Cribl Edge will block the overwrite of the Pack, to prevent deletion of your locally created resources.
>
{.box .success}

When upgrading a Pack, Cribl recommends:
- Import the updated Pack under a new name that includes the version number (e.g., `cribl‑syslog‑input‑120`). This allows you to review and adjust new functionality against currently–deployed configurations.
- Do a side–by–side comparison of the previous and new versions of the Pack – remember to review all comments in the new Pack. Doing this side-by-side comparison allows you to copy Function expressions and other settings from the current version into the same fields in the new version.
- Enable or disable any Functions in the new Pack as necessary. 
- Update any Routes, Pipelines, Sources, or Destinations that use the previous Pack version to reference the new Pack.
- If the Pack includes any user–modified Knowledge objects (e.g., Lookup files), be sure to copy the modified files locally for safe keeping before upgrading the Pack. After installing the upgrade, copy those files over the Pack upgrade's default versions.
- Test, test, and test!
- Commit and Deploy.

### Creating a Pack

You can create a new Pack from scratch, to consolidate and export multiple Cribl Edge configuration objects:

1. Navigate to the **Manage Packs** page.
2. Click **Add Pack**.
3. From the submenu, select **Create Pack**.
4. In the resulting **New Pack** modal, fill in a unique **Pack ID** and other details.

    > - Each Pack within a Cribl Edge instance must have a separate **Pack ID**, but you can assign arbitrary **Display name**s.
    > - **Version** is a required field identifying the Pack's own versioning. 
    > - **Minimum Stream version** is an optional field specifying the lowest compatible version of Cribl Edge software.
    > - **Description** and **Author** are optional identifiers.
    > - **Data type**, **Use cases**, and **Technologies** are optional combo boxes. You can insert one or multiple keywords to help users filter Packs that you [post publicly](#publish) on the Cribl Packs Dispensary™.
    > - **Tags** are optional, arbitrary labels that you can use to filter/search and organize Packs.
    {.box .info}

5. Click **Save**.

![Creating a Pack](se-add-pack-3.5.e6de4a6127.png)
{scale="60%" border="true"}

6. On the **Manage Packs** page, click the new Pack's row to open the Pack.

![Manage Packs page](packs-manage.a11571fc9e.png)
{border="true"}

7. Use the standard Cribl Edge controls (see [above](#controls)) to configure and save the infrastructure you want to pack up. As you save changes in the UI, they're saved to the Pack.

> If you'd like to share your Pack with the community of Cribl users, you can [publish](packs-standards#publish) it on the [Cribl Packs Dispensary™](#dispensary).
> 
> The Cribl Packs Dispensary™ site is designed for sharing completed Packs. If you want to collaborate with others on iteratively developing a Pack, Cribl recommends relying on our [Dispensary GitHub Repo](#repo) for the development phase.
> 
> If you have a Cribl.Cloud account, you can also collaborate there by inviting team members to your Cribl.Cloud Organization. See [Inviting and Managing Other Users](cloud-managing).
> 
> Once your Pack is ready to share, we encourage you to submit it to the Cribl Packs Dispensary™ site. If you already have completed Packs on our GitHub repo, bring them over here!
>
{.box .info}

###  Modifying Pack Settings {#modify}

You can update a Pack's metadata (Version, Description, Author, etc.) and display settings. If you're developing a new Pack to share, you'll want to use this interface to populate the Pack's README and display logo.

1. From the Pack's submenu, select **Pack Settings**. The left **README** tab will gain focus.
2. To populate the Pack's README file, toggle **View** to **Edit**, replace the placeholder markdown content, and **Save**.

![Editing Pack's README](pack-readme.63b0f9a8b6.png)
{border="true"}

3. To update other metadata, click the left **Settings** tab.

![Editing Pack's metadata](pack-metadata.d051028c8c.png)
{border="true"}

4. To add a Pack logo, click the Pack's **Settings** > **Display** left tab.

    Cribl recommends adding a logo to each custom Pack, to visually distinguish the Pack's UI from the surrounding Cribl Edge UI (as well as from other Packs). You can upload a `.png` or `.jpg`/`.jpeg` file, up to a maximum size of 2MB and 350x350px. Cribl recommends a transparent image, sized approximately 280x50px.

### Exporting a Pack  {#export}

> You can also [use the Cribl API to export a Pack](/api/copy-export-install-packs#export-install-pack).
>
{.box .info}

To export a newly created or modified Pack, click its menu icon on the Packs page and select **Export**. 

![Exporting a Pack](pack-export.5ba3e0b794.png)
{border="true"}

The resulting Export Pack modal provides the following options.

#### Export Mode {#export-mode}

Select one of these three buttons:

- **Merge safe**: Attempt to safely merge local modifications into the Pack's default layer (original configuration), then export.

- **Merge**: Force-merge local modifications into the Pack's original configuration, then export.

  > **Merge** is the only export mode available when you've selected `Groups` as your [Export target](#target).
  {.box .info}

- **Default only**: Export only the Pack's original configuration, without local modifications.

The **Merge safe** option is conservative, and will block the export where Cribl Edge can't readily merge conflicting, modified contents with the Pack's original contents:

![**Merge safe** error](pack-export-safe-error.3e83d33788.png)
{scale="90%" border="true"}

If you encounter an error like the example shown above, use the **Merge** or **Default only** export mode instead.

#### Export Target  {#target}



The options here are:

- **File** (the default): You'll be prompted to confirm a file name and destination after you click **OK**.  
(In Cribl Edge 3.5 and higher, the default file name automatically includes the Pack's version number.)

- **Groups**: Selecting this displays a **Groups** control, prompting you to select one or multiple existing Worker Groups/Fleets to export the Pack to. (The current Fleet is automatically omitted from the options.)

#### Exporting Multiple Packs  {#export-multiple}

You can export multiple Packs in one operation, by selecting their check boxes and then clicking **Export multiple Packs**. This option comes with a few constraints:

- You can export multiple Packs only to Groups (not to Files).
- Therefore, this option is available only in [distributed deployments](#deploy-distributed).
- The only [export mode](#export-mode) available is **Merge**.
- The **Exported Pack ID** field is disabled, and hidden.

![Exporting multiple packs at once](packs-export-multiple.f1dffd571d.png)
{border="true"}

A status modal will list any Packs that failed to export.

#### Exported Pack ID {#export-id}

Defaults to the Pack's current ID, with the version number appended. This is an opportunity to change the Pack's ID if you're exporting it as you develop a new version.


