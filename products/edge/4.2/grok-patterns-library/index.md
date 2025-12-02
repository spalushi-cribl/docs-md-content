# Grok Patterns Library


## What Is the Grok Patterns Library

Cribl Edge ships with a Grok Patterns Library that contains a set of pre-built common patterns, organized as files.

![Grok Patterns Library](grok-lib.4aeab5f60c.png)
{border="true"}

## Managing Library Patterns  {#how-does-it-work}

You can access the Grok Patterns Library from the UI's top nav by selecting **Processing** > **Knowledge** > **Grok Patterns**. The library contains several pattern files that Cribl provides for basic Grok scenarios, and is searchable. 

To edit a pattern file, click **Edit** in its **Actions** column. 

To create a new pattern file, click **New Grok pattern file**. In the resulting modal, assign a unique **Filename**, populate the file with patterns, then click **Save**.

![Adding Grok patterns](grok-add-patterns.3c8cc4a9e2.png)
{border="true"}

> Pattern files reside in: `$CRIBL_HOME/(default|local)/cribl/grok-patterns/`
>
{.box .info}

## Using Grok Patterns 

In the current Cribl Edge version, you apply Grok patterns by inserting a [Grok Function](grok-function) into a Pipeline, then manually typing or pasting patterns into the **Pattern** field(s).
