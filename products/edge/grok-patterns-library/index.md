# Grok Patterns Library


## What Is the Grok Patterns Library

Cribl Edge ships with a Grok Patterns Library that contains a set of pre-built common patterns, organized as files.

![Grok Patterns Library](grok-lib-4.9.png)
{border="true"}

## Managing Library Patterns  {#how-does-it-work}

You can access the Grok Patterns Library by selecting **Fleets** from the sidebar and choosing a Fleet.
Then, on the **Fleets** submenu, select **More**, then **Knowledge**, then **Grok Patterns**.
The library contains several pattern files that Cribl provides for basic Grok scenarios, and is searchable. 

To create a new pattern file, select **Add Grok Pattern File**. In the resulting modal, assign a unique **File name**, populate the file with patterns, then select **Save**.

> Pattern files reside in: `$CRIBL_HOME/(default|local)/cribl/grok-patterns/`
>
{.box .info}

## Using Grok Patterns 

In the current Cribl Edge version, you apply Grok patterns by inserting a [Grok Function](grok-function) into a Pipeline, then manually typing or pasting patterns into the **Pattern** field(s).
