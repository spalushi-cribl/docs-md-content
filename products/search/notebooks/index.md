# Cribl Search Notebooks


Run an entire investigation in one tab, and share your work with others.

---

## Why Use Notebooks {#why}

A Notebook is a document-like workspace in Cribl Search where you and other data analysts can combine search queries,
data visualizations, and Markdown notes into persistent, shareable investigations. Here's what you can do with
Notebooks:

- **Iterate**: Run and maintain multiple searches next to one another, for faster, more-efficient investigations.
- **Annotate**: Add context and clarity with markdown notes for rich storytelling.
- **Collaborate**: Share your work through fine-grained edit or read-only access.
- **Control**: See who last edited your Notebook, and when.
- **Summarize**: Use Cribl Copilot to generate summaries of your findings, and to run queries from natural-language prompts.

Start working with Notebooks from anywhere in Cribl.Cloud: On the top bar, select **Products** > **Search** >
**Notebooks**.

## What's in a Notebook {#ui}

A Notebook consists of **search cells** and **note cells**.

When a Notebook is shared, **Maintainers** can add and edit cells, drag cells up and down, and clone the Notebook. **Read Only** users can view the
cells and their results. Read more about [Notebooks access](#access).


![A Notebook with two sample searches](search-notebook-sui.png)
{scale="100%" caption="A Notebook with two sample searches"}

## Start a New Notebook {#create}

{{% tabs "empty" "empty" "From Scratch" "results" "From Current Search" "saved" "From Saved Search" "history" "From History" %}}
{{% tab-item "empty" %}}

You can create an empty Notebook, and then add searches and notes as needed.

1. Go to the **Notebooks** page in Cribl Search: On the top bar, select **Products** > **Search** > **Notebooks**.
1. Select **Add Notebook**. Your new Notebook opens and gets autosaved.
1. Start [running searches](#search) and [adding notes](#notes).

{{% /tab-item %}} {{% tab-item "results" %}}

You can start a Notebook from a search you just ran.

1. Start running your search from [**Search Home**](search-overview#home).
1. Select the **Actions** drop-down, and select **Add to Notebook**.
1. Enter the **Notebook name**.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}}

{{% tab-item "saved" %}}

You can turn any [saved search](search-overview#saved) (as long as it's not part of a Pack) into a Notebook search cell,
creating a new Notebook.

1. Go the **Saved Searches** page in Cribl Search: On the top bar, select **Products** > **Search** > **Saved
   Searches**.
1. Select the Actions button to the right of a search, and select **Add to Notebook**.
1. Enter the **Notebook name**.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}} {{% tab-item "history" %}}

You can turn any search kept in [**History**](search-overview#history) into a Notebook search cell, creating a new
Notebook.

1. Go the **History** page in Cribl Search: On the top bar, select **Products** > **Search** > **History**.
1. Select the Actions button to the right of a search, and select **Add to Notebook**.

1. Enter the **Notebook name**.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}} {{% /tabs %}}

## Add Searches to Your Notebook {#search}

{{% tabs "notebook" "notebook" "Inside the Notebook" "results" "From Current Search" "saved" "From Saved Search" "history" "From History" %}}
{{% tab-item "notebook" %}}

You can run multiple new searches directly from your Notebook. You need to be the Notebook [Maintainer](#access) for
this.

1. Open an existing Notebook or [create](#create) a new one.
1. At the bottom of the Notebook, select **New Search**. A cell with a query box opens.
1. Run your query, as you would anywhere else in Cribl Search. To learn how, see some quick
   [examples](build-a-search#examples), or this tutorial: [Write Your First Query](your-first-query). If your
   Organization has [Cribl Copilot enabled](/copilot/#enable), you can also use your search cells to run
   [natural-language queries](search/copilot-kql).
1. To add another query, select **New Search** again. You can start another search while the first one is still running.

Alternatively, you can create a new search cell by selecting a field within a field summary or event details drawer, and then selecting **Add field to new cell** from the resulting context menu.

![Field's context menu with options to add the field to a new cell or new search](search-notebook-new-cell.png)
{scale="45%" caption="Creating a new cell from a returned field"}

{{% /tab-item %}} {{% tab-item "results" %}}

You can add a search you just ran to an existing Notebook. You need to be the Notebook [Maintainer](#access) for this.

1. Start running your search from [**Search Home**](search-overview#home).
1. Select the **Actions** drop-down, and select **Add to Notebook**.
1. Select **Use Existing**.
1. Select the Notebook.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}}

{{% tab-item "saved" %}}

You can turn any [saved search](search-overview#saved) (as long as it's not part of a Pack) into a Notebook search cell,
adding it to an existing Notebook. You need to be the Notebook [Maintainer](#access) for this.

1. Go the **Saved Searches** page in Cribl Search: On the top bar, select **Products** > **Search** > **Saved
   Searches**.
1. Select the Actions button to the right of a search, and select **Add to Notebook**.
1. Select **Use Existing**.
1. Select the Notebook.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}} {{% tab-item "history" %}}

You can turn any search kept in [**History**](search-overview#history) into a Notebook search cell, adding it to an
existing Notebook. You need to be the Notebook [Maintainer](#access) for this.

1. Go the **History** page in Cribl Search: On the top bar, select **Products** > **Search** > **History**.
1. Select the Actions button to the right of a search, and select **Add to Notebook**.
1. Select **Use Existing**.
1. Select the Notebook.
1. Enter the **Cell title**. This will be the name of the search cell in your Notebook.
1. Select **Add & Go to Notebook**.

{{% /tab-item %}} {{% /tabs %}}

## Add Notes to Your Notebook {#notes}

You can add markdown-formatted notes to your Notebook using headings, lists, links, and more. See
[Markdown Guide](https://markdownguide.offshoot.io/basic-syntax/) for basic syntax.

You need to be the Notebook [Maintainer](#access) for this.

1. Open an existing Notebook or [create](#create) a new one.
1. At the bottom of the Notebook, select **Add Note**. A new note cell opens.
1. Write your notes using markdown. The Notebook gets autosaved.

## Summarize Your Notebook With Cribl Copilot {#summarize}

If your Organization has [Cribl Copilot enabled](/copilot/#enable), you can generate an AI summary of your Notebook
findings.

1. Open an existing Notebook or [create](#create) a new one.
1. In the top-right corner, select **Summarize**.

A summary of your Notebook appears in a new note cell at the top of the Notebook. You can edit the summary as needed.

## Customize Notebook Display {#display}

To make your Notebook easier to skim, you can manage how much detail the Notebook displays. In the top-left corner of each cell, a Collapse/Expand toggle enables you to reduce the cell's vertical depth to a summary view.

In the top-right corner of the Notebook itself, the Actions ![](actions-dropdown.light.svg) drop-down provides two toggles to manage the appearance of the whole Notebook:

- Select **Collapse All Cells** or **Expand All Cells** to control the vertical spread of all cells at once.

- Select **Wide Layout** or **Default Layout** to control the horizontal width available for Notebook contents.

![Screenshot of Notebook-level Summarize button, Share button, and Actions drop-down, showing options to control overall display depth and width](search-notebook-actions-menu.png)
{scale="32%" caption="Summarize and Actions controls"}

## Export Notebook Search Results {#export-results}

You can export the results of a Notebook search as a CSV or NDJSON file.

1. Open an existing Notebook or [create](#create) a new one.
1. In a search cell, select the Actions ![](actions-dropdown.light.svg) button.
1. From the drop-down, select **Export as**, and then either **Export Results as CSV** or **Export Results as NDJSON**.

## Export a Notebook Chart {#export-chart}

You can export a Chart contained in a Notebook search cell, as a JPG or PNG file.

1. Open an existing Notebook or [create](#create) a new one.
1. In a search cell, select the Actions ![](actions-dropdown.light.svg) button.
1. From the drop-down, select **Export as**, and then either **Export Chart as JPG** or **Export Results as PNG**.

## Share Your Notebook {#share}

You can allow others to view or edit your Notebook at any time. For example, you might want to:

- Invite colleagues to join the investigation and contribute their expertise.
- Let others retrace your steps and pick up where you left off.
- Tell the full story behind your analysis, so stakeholders can understand how you reached your conclusions and review
  any assumptions you made.

You can share a Notebook with other Members or Teams. You need to be the Notebook [Maintainer](#access) for this.

1. Open an existing Notebook or [create](#create) a new one.
1. In the top-right corner, select the Share ![](share.svg) button. Now, you can see who has access to the Notebook and
   at what level.
1. Search for the Members or Teams you want.
1. Select the access level (**Read Only** or **Maintainer**). For details, see [Notebooks Access](#access).
1. Confirm with **Add Access**.


![Sharing a Notebook](search-notebook-share.png)
{scale="100%" caption="Sharing a Notebook"}

## Notebooks Access {#access}

By default, your Notebooks are available to the Notebook creator, Search Admins, Organization Admins, and Organization Owners.

| [Search Member Type](cloud-managing#search-member-roles) | Create Notebooks | List Notebooks     | Delete Notebooks   |
| -------------------------------------------------------- | ---------------- | ------------------ | ------------------ |
| Search Editor                                            | ✓                | Only own or shared | Only own or shared |
| Search User                                              | ✓                | Only own or shared | Only own or shared |
| Search Admin                                             | ✓                | ✓                  | ✓                  |
| Organization Admin                                       | ✓                | ✓                  | ✓                  |
| Organization Owner                                       | ✓                | ✓                  | ✓                  |

When you [share a Notebook](#share), you can assign one of two access levels: **Read Only** or **Maintainer**. Here's
what each level allows:

| Action               | Read Only | Maintainer |
| -------------------- | --------- | ---------- |
| View the Notebook    | ✓         | ✓          |
| View Results         | ✓         | ✓          |
| Export Results       | ✓         | ✓          |
| Open in Cribl Search | ✓         | ✓          |
| Rerun cells          |           | ✓          |
| Add cells            |           | ✓          |
| Edit cells           |           | ✓          |
| Delete cells         |           | ✓          |
| Share the Notebook   |           | ✓          |
| Lock or unlock the Notebook |    | ✓          |
| Delete the Notebook  |           | ✓          |

## Synchronize or Lock Edits {#lock}

As a Notebook creator, or with a [Maintainer](#access) or higher Permission: When you save changes to a Notebook, a pop-up will alert you to any changes that other collaborators have made since your last save. In this read-only state, you will be required to reload the Notebook before saving.

You also have the option to lock a shared Notebook into a read-only state for other collaborators (as well as yourself). This is useful if you need to freeze a completed investigation's results, to preserve their integrity against further changes.

In the Actions ![](actions-dropdown.light.svg) drop-down at the top-right corner of the Notebook, select the **Lock Notebook** toggle to preserve the current Notebook state. Select **Unlock Notebook** to make the Notebook editable again.

## Notebooks Retention {#retention}

Notebooks have a hard-coded 30-day retention period to facilitate extended investigations. Exceeding the [**Search history job limit**](limits#jobs) will cause other jobs to be removed before Notebook jobs, to respect this extension.
