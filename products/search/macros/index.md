# Macros


Reuse query text across different searches, by using Macros.

---

Macros are reusable query snippets that you can reference by name when [writing your query](your-first-query), using the
`${...}` syntax. They can contain any valid Cribl Search query text, including functions, operators, or literals.

For example, you can [create](#create-a-macro) a Macro called `externalVpcReject` that consists of the following text:

```kusto
(action=reject srcip!=10.* srcip!=172.16.*)
```

...and then [use](#use-a-macro-in-a-query) it anywhere in a query, like this:

```kusto
${externalVpcReject}
```

...which is equivalent to writing:

```kusto
(action=reject srcip!=10.* srcip!=172.16.*)
```

With Macros, you can construct queries faster and keep them consistent, especially if you frequently rerun the same
complex query in various configurations.

You can [share](#sharing-macros-with-other-members) Macros with other Members of your Organization. To share Macros more
broadly, you can export and import them in [Packs](packs).

### Macro Parameters

Macro parameters are numbered placeholders (`$1`, `$2`, `$3`, ...) that you fill in with different values each time you
inject the Macro into a query. They work similarly to
[Bash positional parameters](https://www.gnu.org/software/bash/manual/bash.html#Positional-Parameters).

For example, you can [create](#create-a-macro) a Macro called `filterByActionAndAnswer` that consists of the following
text:

```kusto
// define three parameters
action=$1 answer=$2 $3
```

...and then [use](#use-a-macro-in-a-query) it anywhere in a query, like this:

```kusto
// pass the same number of arguments, in the same order
${filterByActionAndAnswer, 'reject', 42, ips="172.16.*"}
```

...which is equivalent to writing:

```kusto
action='reject' answer=42 ips="172.16.*"
```

You can pass any type of value as an argument (including other Macros with parameters). If an expression contains commas
(`,`), wrap it in quotes, for example: `${myMacro, "one, two, three", 42}`.

## View Macros Available to You

Certain Macros may already be available to you, depending on your organization's configuration:

1. In Cribl Search, go to **Knowledge** > **Macros**.
1. Here, you can search and filter the available Macros:
   - To see only Macros built into Cribl Search by default, select the **Cribl** filter at the top right.
   - To see only Macros that you [created](#create-a-macro) or that were [shared](#sharing-macros-with-other-members)
     with you, select the **Custom** filter.
   - To search for a specific Macro, type its ID in the **Filter Macros** box.
1. To see a Macro's details, select its row.

## Use a Macro in a Query

You can insert a Macro anywhere in any query. Type `$` followed by the Macro's ID in curly braces `{}`:

```kusto
${macroId}
```


> If the Macro is part of a [Pack](packs), you'll need to include the Pack's ID
> as well. See: [Use a Pack Macro](packs#use-macro).
{.box .info}

For example, to insert Macros named `myMacro1` and `myMacro2`:

```kusto
dataset=myDataset ${myMacro1}
   | limit 1000 ${myMacro2}
```

When a Macro's definition includes a number of [parameters](#macro-parameters), you need to pass the same number of
arguments, in the same order. Pass your arguments after the Macro ID, separated by commas. For example:

```kusto
dataset=myDataset ${myMacro1, argument1, argument2, argument3}
```

If you reference a Macro inside a string or another literal (like regex), it will behave as plain text. For example:

```kusto
// here, "bar" won't be expanded
| where foo="${bar}"
```

Cribl Search's [autocomplete](build-a-search#autocomplete) will suggest available Macros as you type.

## Expand Macros Used in a Query

After you run a query that contains Macros, you can expand the Macros to show the text that each Macro resolved to. This
way, you can see the query exactly as it was executed, which is helpful for debugging your searches.

1. [Run a search](build-a-search).
1. After the search starts running, select **Details** under the query box.
1. In the **Key** column, **query** row, select **Expand Macros**. The query expands to show the text that each Macro
   resolved to during the search execution.

## Create a Macro

To create a new Macro:

1. In Cribl Search, select **Knowledge**, then **Macros**.
1. At the top right, select **Add Macro**. The **New Macro** modal appears.
   - In **ID**, enter your Macro's ID. This is how you and others will reference the Macro in your queries. The ID must
     be unique among all Macros within your organization (even those unavailable to you). For details, see
     [Naming Convention for Macros](#naming-convention-for-macros).
   - In **Description**, enter a brief description of the Macro's purpose.
   - In **Tags**, enter any tags that can help you and others find your Macro.
   - In **Definition**, enter the query text that the Macro will expand to when referenced in a query.
1. In the bottom right, select **Save**.


> You can also clone an existing Macro by selecting its row, and then **Clone Macro**.
{.box .info}

### Naming Convention for Macros

As Macros can be [shared](#sharing-macros-with-other-members) with multiple Members of your organization, and they all
share a single namespace, every Macro ID must be unique across the entire [Cribl.Cloud](cloud) Search tenant.

To manage your namespace in a way that makes the most sense for your organization, we suggest following a consistent
naming convention.

For example, you can use the following convention: `<teamName>_<DatasetName>_<functionName>`. This might create IDs
like:

- `ops_flowlogs_uniqueAddresses`
- `IRT_emailAnomalies_lookupName`

## Edit a Macro

To edit a Macro:

1. Make sure you have the **Maintainer** [Permission](#macro-permissions) for the Macro.
1. In Cribl Search, select **Knowledge**, then **Macros**.
1. Select the row of the Macro that you want to edit. The Macro's details appear.
1. Edit the Macro's **Description**, **Tags**, and **Definition**. For details, see the
   [Create a Macro](#create-a-macro) section.
1. In the bottom right, select **Save**.

## Delete a Macro

To delete a Macro:

1. Make sure you have the **Maintainer** [Permission](#macro-permissions) for the Macro.
1. In Cribl Search, select **Knowledge**, then **Macros**.
1. Select the row of the Macro that you want to delete. The Macro's details appear.
1. In the bottom left, select **Delete Macro**.
1. Type **DELETE**, and then select **Delete Selected Macro**.


>  If the Macro you want to delete belongs to a Pack, see [Delete a Pack Resource](packs#delete).
{.box .info}


## Sharing Macros with Other Members

You can share your Macros with other Members of your organization, to build a library of useful functions and
transformations.

### Macro Permissions

You can [share](#share-a-macro) Macros with individual users or entire Teams, granting one of the two access levels,
**Read Only** or **Maintainer**:

| Permission                                          | **Read Only** | **Maintainer** |
| --------------------------------------------------- | ------------- | -------------- |
| [View the Macro](#view-macros-available-to-you)     | ✓             | ✓              |
| [Use the Macro in a query](#use-a-macro-in-a-query) | ✓             | ✓              |
| [Edit the Macro](#edit-a-macro)                     |               | ✓              |
| [Delete the Macro](#delete-a-macro)                 |               | ✓              |

When you create a Macro, you automatically become its **Maintainer**.

### See Who Can Access Your Macros

By default, Macros that you create are available only to you and the [Search Admin](cloud-managing#search-member-roles).

To see who exactly has access to a Macro:

1. In Cribl Search, select **Knowledge**, then **Macros**.
1. In the row of the Macro that you want to share, select **Share**. The **Macro Members and Teams** modal appears.
1. Here, you can see who has [Permission](#macro-permissions) for the Macro.

### Share a Macro

To share a Macro with another Member of your organization:

1. In Cribl Search, select **Knowledge**, then **Macros**.
1. In the row of the Macro that you want to share, select **Share**. The **Macro Members and Teams** modal appears.
1. Search for the Member or Team that you want to share the Macro with.
1. Select the [Permission level](#macro-permissions) that you want to grant to the Member or Team.
1. Select **Add Access**.
