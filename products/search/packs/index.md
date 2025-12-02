# Cribl Search Packs


Import, export, and share pre-built Cribl Search resources.

---

## Why Use Packs {#why}

A Pack is a collection of preconfigured Cribl Search assets, such as ready-to-use Dashboards, saved searches, or lookup
files. Packs can help you and your team get started faster, quickly see what Cribl Search is capable of, and easily move
complex configurations across different environments.

## What's in a Pack {#what}

A Pack can contain any of the following resources:

- [Dashboards](dashboards)
- [Saved searches](search-overview#saved)
- [Datatypes](datatypes)
- [Macros](macros)
- [Lookups](lookups)

A Pack can't reference other Packs, but it can reference other resources within the same Pack. It can also reference
global, non-Pack resources. For details, see [Pack Dependencies](#dependencies) and [Packs Limitations](#limitations).

## Who Can Use Packs {#who}

Packs are available to Cribl Search **Admins** and **Editors**. For reference, see
[Search Member Permissions](cloud-managing#search-member-roles).

Admins and Editors can:

- [View](#view) Packs installed in the Organization.
- [Use](#use) a Pack's resources, if the Pack is set to [allow global access](#access).
- [Get](#get) new Packs from the Cribl Packs Dispensary, or import them from a file, URL, or Git repo.
- [Modify](#modify) existing Packs, including adding more resources, or modifying metadata.
- [Create](#create) new Packs from scratch, using the Cribl Search UI.
- [Share](#share) Packs, allowing access to Pack resources, or exporting a Pack to a file.

## View Packs {#view}

To see Packs installed in your Organization, go to the **Packs** page in Cribl Search: On the top bar, select
**Products** > **Search** > **Packs**.

The **ID** column shows each Pack's ID, by which you can [reference](#use) the Pack's resources.

You can hover over an ID and select ![](copy.svg) to copy it to the system clipboard.


![**Packs** page](search-packs-page.png)
{scale="100%"}

Select a Pack to view the resources it contains. You can then switch among the upper tabs to view the Pack's
**Dashboards**, **Saved Searches**, **Datatypes**, **Macros**, and **Lookups**.


![Dashboards within a Pack](search-packs-dashboards.png)
{scale="100%"}

## Use Packs {#use}

If a Pack is set to [allow global access](#access), you can use its resources similarly to any other resources in Cribl
Search. For example, you can run saved searches included in the Pack, reference a Pack Macro in a query, or use a Pack
lookup.

### Use a Pack Macro {#use-macro}

To use a Pack [Macro](macros#use-a-macro-in-a-query) from outside its Pack, make sure the Pack is set to
[allow global access](#access). Then, reference the Pack's ID (`packId`) and the Macro's ID (`macroId`), in the
following format:

```kusto
${pack(packId).macroId}
```

For example, to use `myMacro` residing in a Pack called `cribl-sample-data`:

```kusto
${pack(cribl-sample-data).myMacro}
```

### Use a Pack Lookup {#use-lookup}

To use a Pack [lookup](lookups) from outside its Pack, make sure the Pack is set to [allow global access](#access).
Then, reference the Pack's ID (`packId`) and the lookup's ID (`lookupId`), in the following format:

```kusto
lookup pack(packId).lookupId
```

For example, to look for a common field in the event and in a `lookupTable` residing in `myPack`:

```kusto
dataset=myDataset
 | lookup pack(myPack).lookupTable on commonField
```

You can't create or update Pack lookups with the [`export`](export) operator. For example,<br>
  `| export to lookup pack(packId).lookupId` will **not** work.

### Use a Pack Saved Search {#use-saved-search}

To use a Pack [saved search](search-overview#saved) from outside its Pack, make sure the Pack is set to
[allow global access](#access). Then, reference the Pack's ID (`packId`) and the saved search's ID (`savedSearchId`), in
the following format:

```kusto
packId.savedSearchId
```

### Use a Pack Datatype {#use-datatype}

To use a Pack [Datatype](datatypes) from outside its Pack, make sure the Pack is set to [allow global access](#access).
Then, reference the Pack's ID (`packId`) and the Datatype's ID (`datatypeId`), in the following format:

```kusto
packId.datatypeId
```

## Get Packs {#get}

As a Cribl Search Admin or Editor, you can install Packs in your Organization by importing them from the following
sources:

- [Packs Dispensary](#dispensary)
- [File](#file)
- [URL](#url)
- [Git repo](#git)

You can also [create a new Pack](#create) from scratch, or [modify](#modify) an existing Pack.

### Add a Pack from Dispensary {#dispensary}

The [Cribl Packs Dispensary](https://packs.cribl.io/) is a Cribl-hosted resource for you to find and share Packs. Cribl,
our partners, and community users develop Packs and submit them to the Dispensary for easy sharing. Cribl tests
submissions before publication, to ensure each Pack's quality, security guardrails, and stability.

You can install Dispensary Packs as follows:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select **Add Pack** > **Add from Dispensary**. The [Packs Dispensary](https://packs.cribl.io/) will open in a drawer.
1. Using the drawer controls, browse or search for the Pack(s) you want. (You can filter for only Packs **Built by
   Cribl**.)
1. Select any Pack tile to display its details page with its README. This will typically outline the Pack's purpose,
   compatibility, requirements, and installation.
1. To proceed, select **Add Pack** on this page.
1. That's it! You'll see a banner confirming that the Pack is now installed.





### Import a Pack from File {#file}

To import a new Pack, or an updated version of an existing Pack, from your local filesystem:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select **Add Pack** > **Import from File**.
1. Select the file to import.
1. Optionally, give the Pack an explicit, unique **New Pack ID**. (For details about this option, see
   [Upgrade a Pack](#upgrading).)
1. Select **Import** to confirm the import.


> When you import Packs that were exported in `merge` mode, make sure to re-enter any required secrets to restore full functionality. This is necessary because Cribl Search deletes all encrypted fields during the export process, to ensure security and prevent the accidental sharing of sensitive information.
{.box .info}

### Import a Pack from URL {#url}

To import a Pack from a known, public or internal, URL:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select **Add Pack** > **Import from URL**.
1. Enter a valid URL for the Pack's source. (This field's input is validated for URL format, but not for accuracy.)
1. Optionally, give the Pack an explicit, unique **New Pack ID**. (See [Upgrade a Pack](#upgrading).)
1. If the Pack matches an installed Pack's **Pack ID**, confirm that you want to **Overwrite** the existing Pack.
1. Select **Import** to confirm the import.

### Import a Pack from Git {#git}

You can import a Pack from a known public or private Git repo.

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select **Add Pack** > **Import from Git**.
1. Enter the source repo's valid URL.
   > This field's input is validated for URL format, but not for completeness or accuracy. When targeting a private
   > repo, use the format: `https://<username>:<token/password>@<repo‑address>`. Public repos need only
   > `https://<repo‑address>`.
1. Optionally, give the Pack an explicit, unique **New Pack ID**. (See [Upgrade a Pack](#upgrading).)
1. If the Pack matches an installed Pack's **Pack ID**, confirm that you want to **Overwrite** the existing Pack.
1. Optionally, enter a **Branch or tag** to filter the import source using the repo's metadata. You can specify a branch
   (such as `main`) or a tag (such as a release number like `0.5.1`).
1. Select **Import** to confirm the import.

#### Pack Dispensary GitHub Repo {#repo}

A particularly useful public repo is the [Cribl Pack Dispensary](https://github.com/criblpacks) on GitHub. This repo was
established prior to the Cribl-hosted [Cribl Packs Dispensary](#dispensary) site, and it is a place to collaborate on
developing Packs prior to submitting them to the newer site.

You can install Packs directly from this repo using the [Import from Git](#git) option. However, if you prefer, you can
click through to any Dispensary repo's release page, download the corresponding `.crbl` file, and then
[upload the file](#file) into Cribl Search.

## Modify Packs {#modify}

As a Cribl Search Admin or Editor, you can modify existing Packs:

- [Add a resource to a Pack](#add-resources)
- [Modify a Pack's metadata](#pack-settings)
- [Set a Pack's logo](#logo)
- [Upgrade a Pack](#upgrading)
- [Overwrite a Pack](#overwrite)

### Add a Resource to a Pack {#add-resources}

You can fill a Pack with Dashboards, saved searches, Datatypes, Macros, and lookups.

For example, to add a Dashboard to a Pack:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack you're interested in. Use the search box to filter the list.
1. From the **Dashboards** tab, select **Add Dashboard** to access the two options listed below:
   - **Add Existing**: Opens an **Add Existing Dashboards** modal, where you can select one or more Dashboards that are
     available to you, and add them to this Pack. (The list displayed in this modal will, by design, omit Dashboards
     already included in the Pack.)
   - **Add New**: Opens a **New Dashboard** modal, where you can configure a new Dashboard using the controls covered in
     [Create a Dashboard](creating-adding-to-dashboards).

For details on how to reference different types of Pack resources, see [Pack Dependencies](#dependencies).

### Delete a Pack Resource {#delete}

To remove a Pack resource (Dashboard, saved search, Datatype, Macro, or lookup),
you need to delete it from within its Pack.

For example, to delete a Pack Dashboard:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack you're interested in. Use the search box to filter the list.
1. From the **Dashboards** tab, select the checkbox of the Dashboard to delete.
1. Select **Delete selected Dashboards**.

### Modify a Pack's Metadata {#pack-settings}

You can modify a Pack's metadata, such as display name, version, description, and others.

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack you're interested in. Use the search box to filter the list.
1. Select the **Pack Settings** upper tab to expose two options on the left:
   - **README**: Here, you can edit extended information about the Pack, including its purpose, requirements
     (prerequisites), and usage instructions.
   - **Settings**: Here, you can update the Pack's **Display name**, **Version** number, short **Description**,
     **Author**, and arbitrary **Tags**. You can also [control access to the Pack](#access).
1. Save your changes.


![Pack Settings](search-packs-settings.png)
{scale="100%"}

### Set a Pack's Logo {#logo}

We recommend adding a logo to each custom Pack, to visually distinguish the Pack UI from the surrounding Cribl Search UI
(as well as from other Packs).

You can upload a `.png` or `.jpg`/`.jpeg` file, up to a maximum size of 2 MB and 350x350 px. Cribl recommends a
transparent image, sized approximately 280x50 px.

To set a logo for an existing Pack:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack you're interested in. Use the search box to filter the list.
1. Go to **Pack Settings** > **Display**.
1. Select **Choose File** to upload a new logo, and confirm.

### Upgrade a Pack {#upgrading}

To upgrade an existing Pack to a newer version:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Open the Actions ![](actions-dropdown.light.svg) menu on a Pack's row, and then select **Upgrade**.

When upgrading a Pack, we recommend that you:

- Initially, import the updated Pack with a new ID, and with a new **Display name** that includes the version number.
  (For example, `Cribl Search Activity 0.5`.) This enables you to review and adjust new behavior against
  currently–deployed configurations.
- Do a side–by–side comparison of the previous and new versions of the Pack. Remember to review the new Pack's README.
- If the Pack includes any user–modified versions of default Cribl Search Knowledge resources (for
  example, [lookups](lookups)): Be sure to copy the modified files locally for safekeeping, before upgrading the Pack.
  After you install the upgrade, copy those files back to the upgraded Pack, overwriting the default versions in the
  Pack.
- Test, test, and test!
- If you're confident in the new version, you have the option to now overwrite the existing Pack, using the same ID.
- Commit and Deploy.

### Overwrite a Pack {#overwrite}

Each Pack that you install within a given Organization must have a unique ID. The ID is based on the internal
configuration of the Pack – not on its Display name, nor on its parent file name. You cannot share an ID between two (or
more) installed Packs.

If you import a Pack whose internal ID matches an installed Pack – whether an update, or just a duplicate – you'll be
prompted to assign a unique **New Pack ID** to import it as a separate Pack.

Alternatively, you have the option to **Overwrite** the installed Pack, reusing the same ID.


> With the **Overwrite** option, the imported Pack completely replaces your existing Pack.
> We recommend creating a [local backup copy](#export) of the existing Pack first.
>
> If you've modified an installed Pack, Cribl Search will block the overwrite of the Pack,
> to prevent deletion of your locally created resources.
{.box .danger}

## Create Packs {#create}

As a Cribl Search Admin or Editor, you can create a new Pack from scratch, [using the Cribl Search UI](#ui).

You can also create new Packs by [importing](#file), [modifying](#modify), [upgrading](#upgrading), or
[overwriting](#overwrite) existing Packs.

When [adding resources](#add-resources) to a Pack, learn about how [Pack dependencies](#dependencies) work.

### Create a Pack from UI {#ui}

You can create a new Pack from scratch, using the Cribl Search UI.

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select **Add Pack** > **Create Pack**.
1. Enter the following details:
   - **Display name** is an arbitrary, human-readable name to identify this Pack.
   - **Pack ID** is required, and must be unique within your Cribl Search Organization.
   - **Version** is required, and identifies this Pack's own versioning.
   - **Description** and **Author** are optional
     identifiers.
   - **Tags** are optional, arbitrary labels that you can use to filter/search and organize Packs.
   - If you want to [allow global access](#access) to this Pack, check **Allow global access**. This will make all of
     the Pack's resources visible and usable from anywhere in the current Workspace.
1. Select **Save**.
1. On the **Packs** page, select the new Pack's row to open the Pack.
1. [Configure the resources](#add-resources) you want to pack up, using the the standard Cribl Search controls for
   [Dashboards](dashboards), [saved searches](search-overview#saved), [Datatypes](datatypes), [Macros](macros), or
   [lookups](lookups). As you save changes in the UI, they're saved to the Pack.
   
   > If you want to reference resources that reside outside the Pack, learn
   > about how [Pack dependencies](#dependencies) work.
1. Set the pack's [logo](#logo), to visually distinguish the Pack UI from the surrounding Cribl Search UI (as well as
   from other Packs).



## Share Packs {#share}

As a Cribl Search Admin or Editor, you can share Packs in the following ways:

- [Allow access](#access) to all of a Pack's resources, making them usable from anywhere in the current Workspace.
- Move a Pack resource [out of](#move-out) its Pack, to make the resource available globally.
- Move a resource [into](#move-in) a Pack, to make the resource available only within that Pack.
- Move a Pack resource [to another](#move-between) Pack.
- [Clone](#clone) a Pack resource, to play around with it without affecting the original.
- [Export](#export) an entire Pack to a file, so that you can share it with other Organizations.


> To move a sample resource shipped by Cribl, you need to clone it first.
{.box .info}

### Allow Global Access to a Pack {#access}

To make all of a Pack's resources visible and usable from anywhere in the current Workspace:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack you're interested in. Use the search box to filter the list.
1. Go to **Pack Settings** > **Pack Info**.
1. Check **Allow global access**, and confirm with **Save**.

### Move a Resource Out of a Pack {#move-out}

To make a resource (Dashboard, saved search, Datatype, Macro, or lookup) available outside its original Pack, you can
move that resource to your Organization's global level.

For example, to move a Dashboard out of its Pack:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack whose resource you want to move.
1. On the **Dashboards** tab, open a Dashboard's Actions ![](actions-dropdown.light.svg) menu.
1. Under **Move**, select **All Dashboards**.

### Move a Resource Into a Pack {#move-in}

You can move a global resource (Dashboard, saved search, Datatype, Macro, or lookup) into a Pack. This makes the
resource available only within that Pack, and removes it from the global context.

For example, to move a Dashboard into a Pack:

1. Go to the **Dasbboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. Open the Actions ![](actions-dropdown.light.svg) menu of the custom Dashboard you want to move.
1. Under **Move**, hover over **Packs**, and select a Pack.


> Moving a Dashboard into a Pack changes the Dashboard's **Created by** and **Last Modified by** indicators to match your identity. The move also discards any sharing Permissions that were set on the original Dashboard. Share the Dashboard from its new location to add new Permissions.
>
{.box .warning}

### Move a Resource to Another Pack {#move-between}

You can move a resource (Dashboard, saved search, Datatype, Macro, or lookup) from one Pack to another.

For example, to move a Dashboard to another Pack:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack whose resource you want to move.
1. On the **Dashboards** tab, open a Dashboard's Actions ![](actions-dropdown.light.svg) menu.
1. Under **Move**, hover over **Packs**, and select the Pack into which you want to move the resource.

### Clone a Pack Resource {#clone}

To play around with a Pack resource (Dashboard, saved search, Datatype, Macro, or lookup) without affecting the
original, you can create a clone.

Cloned resources are saved to your current UI location. For example, if you clone a Dashboard while [viewing](#view) its
Pack, the clone is saved as part of the Pack. If you clone the same Dashboard while viewing the global **Dashboards** page,
the clone is saved as a global resource.

For example, to clone a Dashboard into its Pack:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. Select the Pack whose resource you want to clone.
1. On the **Dashboards** tab, open a Dashboard's Actions ![](actions-dropdown.light.svg) menu.
1. Select **Clone**. The copy is saved to the Pack, and its visibility outside the Pack depends on the
   [global access](#access) setting.

To clone a Pack Dashboard into the global **Dashboards** page:

1. Go to the **Dasbboards** page in Cribl Search: On the top bar, select **Products** > **Search** > **Dashboards**.
1. Find the Pack Dashboard you want to clone. Look for the Pack's name in the **Pack** column.
1. Open the Dashboard's Actions ![](actions-dropdown.light.svg) menu.
1. Select **Clone**. The copy is saved to the global **Dashboards** page.

### Export a Pack {#export}

To download an existing Pack as a single file, for sharing with other Organizations:

1. Go to the **Packs** page in Cribl Search: On the top bar, select **Products** > **Search** > **Packs**.
1. In the row of the Pack you're interested in, select the Actions ![](actions-dropdown.light.svg) menu, then
   **Export**.
1. In the resulting **Export Pack** modal, the filename defaults to the Pack's current ID, with the version number
   appended. You can modify this, as desired, before confirming the export.

If the Pack references any resources residing outside the Pack (for example, a global lookup file), you'll see a
warning, saying `This Pack has external dependencies`.


> When you export Packs in `merge` mode, make sure to re-enter any required
> secrets after importing the Pack, to restore full functionality. This is necessary because
> Cribl Search deletes all encrypted fields during the export process, to ensure
> security and prevent the accidental sharing of sensitive information.
{.box .info}

## Pack Dependencies {#dependencies}

When you're [creating](#create) a new Pack, or [adding resources](#add-resources) to an existing Pack, you can reference
resources residing outside that Pack ("global" resources). Or, the other way around: you might want to reference a Pack
resource from outside the Pack. Keep in mind the following guidelines about such dependencies.

Pack resources:

- Can reference other resources within the same Pack.
- Can reference global, non-Pack resources.
- Can't reference other Packs.

Global resources, in turn, can reference a Pack's resources only when the Pack is set to [allow global access](#access).

### Reference a Macro

To reference a **global** [Macro](macros) from the **global** context (for example, simply
[using a Macro in a query](macros#use-a-macro-in-a-query)), use the standard Macro syntax.

```kusto
${macroId}
```

To reference a **Pack** Macro from the **global** context, make sure the Pack is set to [allow global access](#access).
Then, specify the Pack's ID with `pack(packId)`, like this:

```kusto
${pack(packId).macroId}
```

To reference a **global** Macro **from within a Pack**, specify the global Pack called `cribl`.

```kusto
${pack(cribl).macroId}
```

To reference a **Pack** Macro **from within the same Pack**, you can use the standard Macro syntax:

```kusto
${macroId}
```


> A Pack can’t reference a Macro residing in another Pack.
{.box .info}

### Reference a Lookup

To reference a **global** [lookup](lookups) from the **global** context (for example, simply using the
[`lookup`](lookup#examples) operator in a query), use the standard lookup syntax. For example:

```kusto
dataset=myDataset
| lookup lookupId on commonField
```

To reference a **Pack** lookup from the **global** context, make sure the Pack is set to [allow global access](#access).
Then, specify the Pack's ID with `pack(packId)`, like this:

```kusto
dataset=myDataset
| lookup pack(packId).lookupId on commonField
```

To reference a **global** lookup **from within a Pack**, specify the global Pack called `cribl`.

```kusto
dataset=myDataset
| lookup pack(cribl).lookupId on commonField
```

To reference a **Pack** lookup **from within the same Pack**, you can use the standard lookup syntax:

```kusto
dataset=myDataset
| lookup lookupId on commonField
```

The following limitations apply:

- A Pack can’t reference a lookup residing in another Pack.
- You can't create or update Pack lookups with the [`export`](export) operator.

### Reference a Saved Search

To reference a **global** [saved search](search-overview#saved) from the **global** context (for example, when
[basing a Dashboard visualization on an existing saved search](editing-dashboards#existing)), use the saved search's ID:

```kusto
savedSearchId
```

To reference a **Pack** saved search from the **global** context, make sure the Pack is set to
[allow global access](#access). Then, specify the Pack's ID (`packId`) like this:

```kusto
packId.savedSearchId
```

To reference a **global** saved search **from within a Pack**, specify the global Pack called `cribl` in the following
format.

```kusto
cribl.savedSearchId
```

To reference a **Pack** saved search **from within the same Pack**, you can use the standard syntax:

```kusto
savedSearchId
```


> A Pack can’t reference a saved search residing in another Pack.
{.box .info}

### Reference a Datatype

To reference a **global** [Datatype](datatypes) from the **global** context (for example, when
[adding a Dataset](connect-to-data#dataset)), you normally use the Datatype's ID:

```kusto
datatypeId
```

To reference a **Pack** Datatype from the **global** context, make sure the Pack is set to
[allow global access](#access). Then, specify the Pack's ID (`packId`) like this:

```kusto
packId.datatypeId
```


> You can't reference a Datatype **from within** a Pack.
{.box .info}

## Packs Limitations {#limitations}

Here's what you can't do with Packs:

- Packs can't reference other Packs.
- You can't configure [Notifications](notifications) on a saved search that resides within a Pack.
- You can't create or update Pack lookups with the [`export`](export) operator. For example,<br>
  `| export to lookup pack(packId).lookupId` will **not** work.
