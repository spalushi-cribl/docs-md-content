# Regexes


Use the built-in library of regular expressions, or define your own.

---

A regular expression (regex) is a sequence of characters that specifies a match pattern in text. Cribl Search uses regexes in definitions for [Parsers](parsers).

Cribl Search ships with many commonly used regular expressions. The **Regexes** page provides an interface for creating and
editing regular expressions. The table is searchable, and you can add Tags to each regex as necessary.

To open the **Regexes** page, select **Knowledge** > **Regexes**.


![Regexes page](search-regexes-page.png)
{scale="100%"}


> For details about who can access and modify this resource, see [Search Member Permissions](cloud-managing#member-roles).
>
{.box .info}

## Add Custom Regex

1. On the top right, select **Add Regex**.
1. In the resulting modal, give your custom regex a unique **ID** and optionally, provide a **Description**.
1. Enter your **Regex pattern** and any **Sample data** the regex matches against.
1. Optionally, you can assign **Tags** for further organization or categorization in Cribl.
1. Select **Save**.

## Exporting and Importing Regexes

You can export and import Custom (or Cribl) Regexes as JSON files. This facilitates sharing Regexes among Worker Groups
or [Cribl Stream](/stream/) and [Cribl Edge](/edge/) deployments.

To export a Regex:

1. Select to open an existing Regex, or create a new Regex.
1. In the resulting modal, select **Manage as JSON** to open the JSON editor.
1. You can now modify the Regex directly in JSON, if you choose.
1. Select **Export**.

To import any Regex that has been exported as a valid JSON file:

1. [Create](#add-custom-regex) a new Regex.
1. In the resulting modal, select **Manage as JSON** to open the JSON editor.
1. Select **Import**, and choose the file you want.
1. Select **OK** to get back to the **New regex** modal.
1. Select **Save**.


> Every Regex must have a unique value in its top `id` key. Importing a JSON file with a duplicate `id` value will fail
> at the final **Save** step, with a message that the Regex already exists. You can remedy this by giving the file a
> unique `id` value.
>
{.box .warning}

## Cribl Regexes Versus Custom Regexes

Regexes shipped by Cribl are listed under the **Cribl** tag, while user-built regexes are listed under
**Custom**. Over time, Cribl will ship more patterns, so this distinction allows for both sets to grow independently. In
the case of an ID/Name conflict, the Custom pattern takes priority.
