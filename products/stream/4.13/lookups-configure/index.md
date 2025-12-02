# Configure Lookups


This page walks you through the key tasks for configuring and using lookup files in Cribl Stream. You'll learn how to upload and index lookup files, reference them in Pipelines or Routes, update them, and remove them when they’re no longer needed.

## Overview of the Workflow for Using Lookups

To use lookups in your Cribl Stream deployment:

1. [Plan your lookup strategy](lookups-about#plan): Begin by making strategic decisions about how and where to use lookups in your deployment. This includes evaluating whether ingest-time or search-time lookup files are appropriate for your use case. You also need to estimate hardware needs and consider strategies for optimizing your system for better performance.

1. [Upload a lookup file](#upload-lookups): Add your lookup files to either the global [Knowledge Library](lookups-library) or to a specific [Pack](packs), depending on your use case.

1. [Add indexes to the lookup file](#add-indexes): Improve performance by indexing the fields you'll use for disk-based lookups. Cribl Stream supports a maximum of four indexes per file.

1. [Reference the lookup file in a Pipeline or Route](#reference): Use the Lookup Function or the `C.Lookup()` expression to enrich, route, or filter data based on lookup results.

1. [Update lookup files as needed](#update-lookups): You can update lookup files manually through the UI or programmatically using the API, which is the ideal method for automated or frequent updates.

## Upload a Lookup File {#upload-lookups}

You can upload lookup files as a global object in the Knowledge Library or as an object inside a Pack. The process for uploading a lookup file to a Pack is nearly the same as uploading it to the global Knowledge library. The processes are relatively similar. The key difference is that you must upload the file from inside the Pack in order to scope it correctly to that Pack’s resources.

> Before uploading a lookup file, make sure it’s properly formatted and optimized for performance. See [Optimize Your Lookup Files](lookups-about#optimize-lookup-files) for guidance on preparing your file.
>
{.box .info}

### Upload a Lookup File to the Knowledge Library

To upload a new lookup file to the Knowledge Library:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Knowledge.** The **Lookups** page opens by default. If it doesn't, select **Lookups** from the side menu.

1. Select **Add Lookup File**, then select how you want to create the file:

   - **Upload a New File:** Browse to and upload an existing lookup file from your computer. See [Supported Lookup File Types](lookups-about#supported-lookup-file-types) for more information.
   - **Create with Text Editor:** Enter comma-separated values (CSV) directly using the built-in editor.

1. In the **New Lookup File** modal, configure the following settings:

   - **Filename:** Enter a unique name for the lookup file.
   - **Description:** Optionally, add a short description of the file's contents or purpose.
   - **Tags:** Optionally, add tags to help you group and filter lookup files on the **Lookups** page. These tags are not added to processed events. Separate tags with a tab or hard return.
   - **Mode:** Select how you want to store and access the lookup file. Options include **Store in Memory** or **Store on Disk**. See [Choose a Lookup File Storage Mode](lookups-about#storage-mode) for guidance about which mode to choose.

1. Select **Save**. The new lookup file appears in the table on the **Lookups** page. Check the **Status** column to confirm that the file is ready for deployment.

1. Select **Deploy** to deploy the lookup file to the Workers in the current Worker Group. In the **Deploy Group** modal, select or clear the check boxes next to the lookup files you want to deploy.

> Disk-based lookups do not require you to commit lookup file changes in order to deploy them to Workers. Lookup filenames are automatically added to `.gitignore` when uploaded.
>
{.box .info}

### Upload a Lookup File to a Pack

Uploading a lookup file to a Pack follows the same general steps as uploading one to the global Knowledge Library. However, to ensure you have properly scoped the lookup file to the Pack, you must upload it from within the Pack itself. When referencing the file in Pipelines or Routes, be sure to use relative paths (often defined with environment variables) to maintain compatibility within the Pack context.

To upload a lookup file to a Pack:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Packs.**

1. Select the Pack where you want to add a lookup file.

1. Inside the Pack, select the **Knowledge** tab. Then follow the standard lookup upload process. See [Upload a Lookup File to the Knowledge Library](#upload-lookups) for details.

1. After deploying the lookup file, validate that you can view the Lookup on your Workers.

1. When referencing the lookup file from within the Pack, ensure that you use a Pack-scoped path.

> Do not reference lookup files using global paths or Leader paths, such as `$CRIBL_HOME/groups/<your group>/data/lookups/host_destination_test.csv`.
> Also avoid using unqualified filenames like `host_destination_test.csv`.
>
{.box .warning}

When working with disk-based lookup files inside a Pack, the **Undo All Changes** button in the Git commit modal does not revert changes to those lookup files. Because Cribl Stream excludes disk-based lookups from Git version control in the UI, you must track modifications and roll them back manually.

## Add Indexes to a Lookup File (Disk-Based Only) {#add-indexes}

In a lookup file, an index is a fast-access data structure built from one or more header fields. For disk-based lookup files, Cribl Stream requires you to index the field you use in the [Lookup Function](lookup-function). If you reference a field that you have not indexed, the lookup will fail.

Cribl Stream pre-processes the indexed fields to avoid row-by-row scans. Instead of searching the entire file for a match, the system can jump directly to the relevant row. This indexing mechanism significantly improves lookup performance, especially for large files that might otherwise affect system latency.

Some notes about index behavior for disk-based lookups:

- Cribl Stream automatically indexes the left-most column of each lookup file by default. To take advantage of this, place your primary key fields in the first column.
- You can define a maximum of four indexes per lookup file.
- You can set each index to have match values in a case-sensitive or case-insensitive manner.

> Before uploading a lookup file, make sure it’s properly formatted and optimized for performance. See [Optimize Your Lookup Files](lookups-about#optimize-lookup-files) for guidance on preparing your file.
>
{.box .info}

To add indexes to a disk-based lookup file:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Knowledge.** By default, this opens the **Lookups** page. If not, select it from the side menu.

1. On the **Lookups** page, select the name of the lookup file you want to edit.

1. In the **Indexes** setting, select **Add Index** or select an existing row to edit it. In the menu that appears, select the column you want to index.

1. Toggle **Case-sensitive** on to enable case-sensitive matching for the indexes listed in that row.

1. Select **Save** to apply your changes and return to the **Lookups** page.

## Reference a Lookup File or GeoIP Database File in a Route or Pipeline {#reference}

After uploading a lookup file or GeoIP database to Cribl Stream, you can then reference that file to enrich your data. You can reference a lookup file in one of two ways:

- **Use the Lookup Function in your Pipeline.** Add the [Lookup Function](lookup-function) to your Pipeline. Any lookup files you have uploaded to the Knowledge Library appear in the **Lookup file path** drop-down menu. You can also manually enter the lookup file path. If needed, you can reference environment variables using `$`, such as `$CRIBL_HOME/file.csv`. See [Lookup Function](lookup-function) for more information.
- **Use the `C.Lookup()` expression in a Route or Pipeline.** You can call the `C.Lookup()` expression in any field that accepts JavaScript expressions, such as in the **Filter** field. See [C.Lookup - Inline Lookup Methods](expressions-lookup) and [Build Custom Logic to Route and Process Your Data](filter-and-transform-data) for more information.

Default lookup file paths:

| Deployment Type | Path |
| --------------- | ---- |
| Cribl-managed (Cloud) | `$CRIBL_HOME/groups/<group-name>/data/lookups/<file-name.csv>` |
| Single instance (on-prem) | `$CRIBL_HOME/data/lookups/<file-name.csv>` |
| Distributed (on-prem) | `$CRIBL_HOME/groups/<group-name>/data/lookups/<file-name.csv>` |
| Lookups stored in a Pack | `$CRIBL_HOME/data/lookups/packs/<pack-name>/<file-name.csv>` |

## Delete a Lookup File

Deleting a lookup file that is actively referenced in Pipelines or Routes can result in runtime errors or failed deployments. Before deleting a file, verify that it is no longer in use or update all references as needed.

To delete a lookup file:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Knowledge.** By default, this opens the **Lookups** page. If not, select it from the side menu.

1. On the **Lookups** page, check the box next to the names of any lookup files you want to delete.

1. Select **Delete Selected Lookup Files**.

1. Type `DELETE` to confirm.

1. Verify the lookup file no longer appears on the **Lookups** page.

1. Commit and deploy your changes.

## Update a Lookup File {#update-lookups}

Your options for updating lookup files might be limited based on the storage mode you selected at configuration time. Disk-based lookup files cannot be edited in place using the UI. See [Plan Your Lookup File Storage Mode](lookups-about#storage-mode) for more information.

> You don’t need to upload a new disk-based lookup file just to update its indexes. You can modify indexes directly. See [Add Indexes to a Lookup File](#add-indexes) for more information.
>
{.box .info}

### Update a Lookup File Using the API

You can update lookup files using the API if needed. Use the API if your lookup files need frequent updates or as part of an automated process. See [Create and Update Lookups](/api/create-update-lookups/) in the API documentation for more information.

### Update a Lookup File in the UI (for In-Memory Lookups)

For in-memory lookups, you can edit lookup files directly through the UI:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Knowledge.** By default, this opens the **Lookups** page. If not, select it from the side menu.

1. On the **Lookups** page, select the name of the lookup file you want to edit.

1. In the settings for the lookups file, you can edit the file directly using either table or text mode:

   - **Table:** Double-click cells to change their value.
   - **Text:** Directly edit the lookup file in comma-separated values (CSV) text format.

1. Select **Save** to apply your changes and return to the **Lookups** page.

### Update a Lookup File in the UI (for Disk-Based Lookups)

For disk-based lookups, you can't edit lookup files directly through the UI. To update a file, you must delete the existing version and upload a new one.

> You don’t need to upload a new disk-based lookup file just to update its indexes. You can modify indexes directly. See [Add Indexes to a Lookup File](#add-indexes) for more information.
>
{.box .info}

To update a disk-based lookup file:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Processing**, then **Knowledge.** The **Lookups** page opens by default. If it doesn't, select **Lookups** from the side menu.

1. On the **Lookups** page, find the name of the lookup file you want to edit and select the ![Download icon](download-icon.png) **Download** icon in the **Action** column.

1. Open the downloaded file in a text editor or spreadsheet tool, and make the necessary changes.

1. Delete the original lookup file from the Knowledge Library.

1. Re-upload the updated version. See [Upload a Lookup File](#upload-lookups) for more information.

> If the re-uploaded file uses the same filename, any references to the lookup file by name will continue to work without changes. However, if you upload the file under a different name, you’ll need to update all references to point to the new filename.
>
{.box .info}
