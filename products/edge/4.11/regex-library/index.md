# Regex Library


## What Is the Regex Library

Cribl Edge ships with a Regex Library that contains a set of pre-built common regular expression patterns. This library serves as an easily accessible repository of regular expressions. The Library is searchable, and you can assign tags to each pattern for further organization or categorization.

To access the library, select **Fleets** from the sidebar, and choose a Fleet.
Then, on the **Fleets** submenu, select **More**, then **Knowledge**, then **Regexes**.

![Regex Library](regex-lib-4.9.png)
{border="true"}

## Using Library Patterns

To insert a pattern into a [Function](functions)'s regex field, first select the ![Advanced mode button](popout-icon.light.svg) Advanced mode or ![Edit button](edit-button.light.svg) Edit icon beside that field.

In the resulting Regex or Rules modal, Regex Library patterns will appear as typeahead options. Select a pattern to paste it in. You can then use the pattern as-is, or modify it as necessary. 

![Inserting a pattern from the Regex Library](regex-library-typeahead-4.9.png)
{border="true"}

## Adding Patterns to the Library

You can also add new, custom patterns to the Library. In the same modal, once you've built your pattern, select **Save to Library**.

![Adding a custom pattern to the Regex Library from a Function's Regex modal](regex-library-custom-save-4.9.png)
{border="true"}

In the resulting modal, give your custom pattern a unique ID. Optionally, you can also provide a **Description** (name) and groom the **Sample data**. Then select **Save**.

![Identifying the custom pattern](regex-library-custom-details.919fba54ea.png)
{scale="40%" border="true"}

Your custom pattern will now reside in the Regex Library. It will be available to Functions using the same typeahead assist as Cribl's pre-built patterns.

## Cribl vs. Custom and Priority

Within the Library, patterns shipped by Cribl will be listed under the **Cribl** tab, while those built by users will be found under **Custom**. Over time, Cribl Edge will ship more patterns, and this distinction allows for both sets to grow independently. 

In the case of an ID/Name conflict, the Custom pattern takes priority in listings and search. For example, if a Cribl-provided pattern and a Custom one are both named `ipv4`, the one from Cribl will not be displayed or delivered as a search result.
