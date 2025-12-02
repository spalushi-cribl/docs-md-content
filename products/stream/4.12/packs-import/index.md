# Import Packs


Packs created by Cribl and the Cribl Community let you speed up configuring your data flows by providing ready-made solutions for many common integrations and use cases.

You can import Packs into your deployment from the [Cribl Pack Dispensary](https://packs.cribl.io),
or, alternatively, from a Git repository.

You can then customize each imported Pack to suit your needs.

## Import Pack from the Dispensary

To import a Pack from the Dispensary directly into your deployment:

1. In Cribl Stream, in the sidebar, select **Worker Groups**, then choose a Worker Group.
1. On the **Worker Groups** submenu, select **Processing**, then **Packs**.
1. Select **Add Pack** and then **Add from Dispensary**.
1. The [Packs Dispensary](https://packs.cribl.io/) will open in a drawer.

   ![Packs Dispensary drawer open over Cribl UI, with a list of available Packs](pack-dispensary-drawer-4.12.png)
   {scale="80%" border="true" caption="Searching for Packs in the Dispensary drawer."}

1. Using the drawer controls, browse, or search for the Pack(s) you want.
1. Select any Pack tile to display its details page with its README. This will typically outline the Pack's purpose, compatibility, requirements, and installation.
1. To proceed, select **Add Pack** on this page.
1. If the Pack you are importing binds [variables](#pack-variables) to any fields, you will see a **Configure Variables** button in the notification about successful import.
   Select it to go to the **Variables** page, where you can configure the values for the variables for your deployment.

The Pack will be downloaded and automatically added to your selected Worker Group.



## Explore and Customize the Pack

Click inside the new Pack. In the Pack submenu you can see all the components that the Pack contains:
Routes, Pipelines, Sources, Destinations, and Knowledge Objects.

![Pack menu showing Routes, Pipelines, Sources, Destinations, and Knowledge Objects](pack-menu-4.12.png)
{scale="80%" border="true"}

At this point you can edit any elements in the Pack to customize it to your needs.
Do it the same way you would when working directly in any Worker Group.

> Remember that all elements inside the Pack, including Knowledge Objects, are scoped to that Pack.
> This means you cannot use, for example, a Knowledge Object defined inside a Pack outside it.
{.box .info}

## Upgrade Pack

When a new version of a Pack is published to the Dispensary, you can upgrade your local Pack to get all the latest improvements.

If you didn't introduce local changes to the Pack, you can upgrade the Pack directly from the UI.
On the **Packs** page, open the Options ••• menu for your Pack, and then select **Upgrade**.

![Upgrading an existing Pack](pack-upgrade-4.12.png)
{scale="80%" border="true"}

If you've modified the Pack, Cribl Stream will not overwrite it during upgrading,
to prevent deletion of your locally created resources.
To reconcile your own changes with improvements coming in from a new Pack version:

1. Import the updated Pack as a separate Pack with a new name.
   Give it a name that includes the version number (for example, `cribl‑syslog‑input‑120`).
1. Do a side–by–side comparison of the previous and new versions of the Pack.
   Copy configurations from your modified Pack into the upgraded Pack.
   Review all comments in the upgraded Pack to understand changes introduced in the new version.
1. Enable or disable any Functions in the new Pack as necessary. 
1. Update any Routes, Pipelines, Sources, or Destinations that use the previous Pack version to reference the new Pack.
1. If you modified default Cribl Stream Knowledge Objects (for example, lookup files),
   copy the modified files locally for safekeeping, before upgrading the Pack.
   After you install the upgrade, copy those files back to the upgraded Pack, overwriting the default versions in the Pack.
1. Commit and Deploy.

