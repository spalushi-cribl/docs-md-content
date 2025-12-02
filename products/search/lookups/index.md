# Lookups


Enrich events with lookup tables.

---

Lookups are tables of data referenced by the [`lookup`](lookup) and [`ip-lookup`](ip-lookup) operators to enrich events. For example, you can use lookups to map user IDs to additional attributes like names or addresses, and return this enriched data in the search results.

You can access the **Lookups** page, which provides a management interface for all lookups, by selecting **Knowledge** >
**Lookups**.

This lookups table is searchable, and you can add Tags to each lookup as necessary. There's full support for `.csv`
files. Compressed files are supported, but must be in gzip format (`.gz` extension).


![Lookups page](search-lookups.png)
{scale="90%"}


> For details about who can access and modify this resource, see [Search Member Permissions](cloud-managing#member-roles). With appropriate Permissions, you can export, share, and import lookups in [Packs](packs).
>
{.box .info}

## Add a Lookup

As an alternative to using the **Lookups** page to add a lookup, you can use the [`export` operator](export##export-to-lookup-table) in a query to create or update a lookup. The `export` operator can write a maximum of 10,000 rows to the table.

1.  On the top right, select **Add Lookup File**.
1.  Select **Upload a New File** or **Create with Text Editor**.
1.  Give your lookup a unique **Filename**. You'll use this to identify the lookup table in a lookup operation.
1.  Optionally, enter a **Description**.
1.  Optionally, enter any desired **Tags**.
1.  If you selected **Create with Text Editor**, enter your lookup table in the text box. Use CSV (comma-separated
    values) format with the header values on the first row as the field names, and the field values in the subsequent
    rows.
    
    > Lookup field names must use letters, numbers, and underscores only.
    {.box .warning}
1.  Select **Save**.

You can't use the `export` operator to create [Pack](packs) lookups.

### Example Lookup Table

```
service_name,port_number,transport
tcpmux,1,tcp
tcpmux,1,udp
compressnet,2,tcp
compressnet,2,udp
compressnet,3,tcp
compressnet,3,udp
rje,5,tcp
rje,5,udp
echo,7,tcp
```

## Modify a Lookup

To re-upload, expand, edit, or delete an existing lookup, select its row on the **Lookups** page.


>  If the lookup you want to delete belongs to a Pack, see [Delete a Pack Resource](packs#delete).
{.box .info}

In the resulting modal, you can edit files in **Table** or **Text** mode. However, **Text** mode is disabled for files
larger than 1 MB. The maximum lookup size is 10,000 rows.


![Editing in Table mode](search-lookup-edit-table-mode.png)
{scale="90%"}


![Editing in Text mode](search-lookup-edit-text-mode.png)
{scale="90%"}


> ##### Editing Regex with Quotation Marks
> When a regex contains quotes, edit it in **Text** mode. Do not edit it in **Table** mode, which can cause the regex to
> behave in unexpected ways.
{.box .warning}

You can't use the [`export`](export) operator to modify [Pack](packs) lookups.

## Export a Lookup {#export}

To export a lookup file, including any modifications that you've made, you have two options: 

- From a lookup's configuration modal, select the **Download File** button.

- From the **Lookups** page, Open the Actions ![Actions button](actions-dropdown.light.svg) menu on the lookup file's table row, then select **Download**.

## List Available Lookups

You can list all your lookup files straight from the query box, using the [`$vt_lookups`](vt_lookups) virtual table:

```kusto {runSearch=true}
// lists all lookups available to the current user
dataset="$vt_lookups"
```

## Search the Contents of Your Lookups

You can use the [`$vt_lookups`](vt_lookups) virtual table to search the contents of your lookups.

For example, to get the contents of a lookup named `service_names_port_numbers.csv`:

```kusto {runSearch=true}
dataset="$vt_lookups" lookupFile="service_names_port_numbers"
```

