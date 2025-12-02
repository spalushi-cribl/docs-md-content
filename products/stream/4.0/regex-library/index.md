# Regex Library


## What Is the Regex Library

Cribl Stream ships with a Regex Library that contains a set of pre-built common regex patterns. This library serves as an easily accessible repository of regular expressions. The Library is searchable, and you can assign tags to each pattern for further organization or categorization. Access the Library from Cribl Stream's top nav under **Processing** > **Knowledge** > **Regex Library** .

![Regular Expression Library](regex-lib-3.0.3c123e0638.png)
{border="true"}

## Using Library Patterns  {#how-does-it-work}

As of this version, the Library contains 25 patterns shipped by Cribl Stream. To insert a pattern into a [Function](functions)'s regex field, first click the pop-out or Edit icon beside that field.

![Opening a Regex modal](regex-library-access.8f4c8bb30c.png)
{border="true"}

In the resulting Regex or Rules modal, Regex Library patterns will appear as typeahead options. Click a pattern to paste it in. You can then use the pattern as-is, or modify it as necessary. 

![Inserting a pattern from the Regex Library](regex-library-typeahead.27c4727414.png)

## Adding Patterns to the Library

You can also add new, custom patterns to the Library. In the same modal, once you've built your pattern, click the **Save to Library** button.

![Adding a custom pattern to the Regex Library from a Function's Regex modal](regex-library-custom-save.2da40437e4.png)
{border="true"}

In the resulting modal, give your custom pattern a unique ID. Optionally, you can also provide a **Description** (name) and groom the **Sample data**. Then click **Save**.

![Identifying the custom pattern](regex-library-custom-details.919fba54ea.png)
{scale="60%"}

Your custom pattern will now reside in the Regex Library. It will be available to Functions using the same typeahead assist as Cribl's pre-built patterns.

## Cribl vs. Custom and Priority

Within the Library, patterns shipped by Cribl will be listed under the **Cribl** tab, while those built by users will be found under **Custom**. Over time, Cribl Stream will ship more patterns, and this distinction allows for both sets to grow independently. 

In the case of an ID/Name conflict, the Custom pattern takes priority in listings and search. For example, if a Cribl-provided pattern and a Custom one are both named `ipv4`, the one from Cribl will not be displayed or delivered as a search result.
