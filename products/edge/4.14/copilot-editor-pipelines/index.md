# Create Pipelines With Cribl Copilot Editor


Cribl Copilot Editor is an interactive, AI-powered assistant that helps you build data Pipelines in Cribl Edge using plain-language prompts. Instead of building a Pipeline manually, you can describe what you want to do with your data and Copilot Editor will generate a working Pipeline for you.

Copilot Editor gives you an easy way to explore what Cribl Edge can do, especially if you're new to the platform or want help tackling more complex use cases. You stay in control: Copilot Editor's suggestions are editable, and you can review, modify, or view outputs from the suggestions before using them in production.

Here are a few common ways you can use Copilot Editor:

- **Structure raw data to match a standard schema or convert from one schema to another:** The data for many Destinations follow specific schemas. If events are improperly formatted, some Destinations may drop data or store them in ways that make the data difficult to find and use. With Cribl Edge, you can convert your raw data into normalized schemas, using Copilot to do the heavy lifting around configuring Functions. Copilot Editor is optimized for transforming data into the OCSF, UDM, and ECS schemas. You can also upload your own schema objects to convert to other schemas. See [Convert Data into a Custom Schema](copilot-editor-custom-schemas) for information about using custom schemas or schemas that aren't yet officially supported.
- **Apply common Functions to transform or enrich your data:** Copilot Editor can apply Cribl Edge’s most frequently used Functions based on your intent. You don't need to know Regex or JavaScript syntax or which Functions to use up front. Just describe your goals and Copilot Editor builds the appropriate logic for you.
- **Filter and route data dynamically:** Use plain-language prompts to define filtering or routing logic without writing Regex or JavaScript expressions manually. Copilot Editor builds the first draft of the filtering and control logic. You can use its suggestions as-is or fine-tune as needed.

![Copilot Editor](copilot-editor-sample-data.png)
{border="true"}

## When to Use Copilot Editor

Copilot Editor is useful for anyone who wants help building Pipelines in Cribl Edge, especially if you're just getting started or working through a complex use case. You might find Copilot Editor especially helpful if:

- **You are new to Cribl Edge and want a guided way to learn how it works.** Copilot Editor acts as an interactive learning tool. Copilot Editor is especially helpful when you’re not sure how to start building a Pipeline, or when you're stuck writing complex Regex or JavaScript expressions. It can generate example Pipelines based on your goals, suggest alternative approaches, and help you explore solutions you might not have considered. Even if you don’t use the generated Pipeline, it's a great way to learn how different Functions work together and how to build your own Pipelines.
- **You’ve inherited a Cribl deployment and need to make changes quickly.** If you’re a system administrator or operator who needs to stand up a deployment fast or maintain an existing one, Copilot Editor can help you make quick progress without needing to dive deep into configuration syntax right away.
- **You want to experiment with AI to speed up Pipeline building.** Whether you’re curious about AI or looking for a faster way to translate your intent into working logic, Copilot Editor lets you test ideas and see results with minimal setup.

You’re always in control of the final result and can review, refine, or copy its suggestions into the Pipelines you build manually.

## How to Use Copilot Editor

This section explains the overall workflow for using Copilot Editor to build Pipelines.

### Before You Begin {#enable-copilot}

Before you can start using Copilot Editor, you must first enable Cribl Copilot for your Organization:

1. From **Settings**, select the **Global** tab, and then select **AI Settings**.

1. Read the [privacy policy](https://cribl.io/legal/privacy-policy/?_gl=1*phkcpv*_gcl_au*MTM1MTI4NTE3NC4xNzQ1NjA2Mzg0*_ga*MTMzMDQ4OTE4LjE3NDU2MDYzODM.*_ga_1V3PHS2009*czE3NDczMTEyMzIkbzUwJGcxJHQxNzQ3MzExNTIyJGo0MCRsMCRoMA..) and accept the terms and conditions.

> See [Enable or Disable Cribl Copilot](/copilot/) for information about how to enable or disable Cribl Copilot programmatically.
>
{.box .info}

### Launch Copilot Editor {#launch-copilot}

You can launch Copilot Editor in three ways:

- Through [QuickConnect](quickconnect).
- Through the [Pipelines](pipelines) page.
- Inside a Pipeline through the **Add Function** menu.

Select a tab for more detailed instructions about each method for launching Copilot Editor:

{{% tabs "quick-connect" "quick-connect" "QuickConnect" "pipelines" "Pipeline page" "add-function" "Add Function menu" %}}
{{% tab-item "quick-connect" %}}
To launch Copilot Editor using QuickConnect, first set up your QuickConnect environment:

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Worker Groups**, select a Worker Group.

1. Select the **QuickConnect** tile.

1. Select **Add Source** to add at least one Source and **Add Destination** to add at least one Destination. Configure them as needed. Any previously configured Sources and Destinations automatically appear in QuickConnect.

1. Connect a Source to a Destination, but do not create a Pipeline yet.

   ![Connect a Source to a Destination](quickconnect-multiple.19843ce03e.png)
   {border="true"}

1. Select a connection to open the **Connection Configuration** modal. Select **Build with Copilot Editor** to launch the interactive Copilot Editor modal.

You can also select **Build with Copilot Editor** at the top of the **QuickConnect** page to launch.
{{% /tab-item %}}
{{% tab-item "pipelines" %}}

Before you can launch Copilot Editor through the [Pipelines](pipelines) page, you first need to manually add a Pipeline. See [Add a Pipeline](pipelines#add-pipelines) for more information. From the Pipelines page, launch Copilot Editor from the main Pipeline menu by selecting **Add Pipeline**, then **Add Pipeline with Cribl Copilot**.

![Create or import a Pipeline](pipeline-add-options-4.13.png)
{border="true"}
{{% /tab-item %}}
{{% tab-item "add-function" %}}
To launch Copilot Editor from inside a Pipeline, select the **Add Function** menu, then **Edit with Copilot Editor**.

![Launch Copilot from the Function menu](copilot-edit.png)
{border="true"}
{{% /tab-item %}}
{{% /tabs %}}

### Build a Pipeline with Copilot Editor

The interactive flow of Copilot Editor follows a few general phases:

1. **Capture sample data:** When you first launch Copilot Editor, you see a chat window that prompts you to capture sample data. It asks you to choose a method for capturing sample data, such as collecting a live sample or by uploading sample data for that Source.

   > **Why does it need sample data?**
   >
   > Copilot Editor uses this data to help you build, preview, test, and validate any data transformations you ask it to create in your Pipeline. It helps you compare the inbound and outbound data to ensure that any Pipeline and Function configurations created with Copilot Editor produce the expected output, giving you confidence it is processing your data as intended.
   >
   {.box .info}

1. **Choose an objective:** After analyzing your sample data, Copilot Editor displays your data in the preview window, similar to how data is presented in [Data Preview](data-preview) in a standard Pipeline. The chat window will summarize the data and present you with a set of options for transforming your data. Select any objectives you would like to apply to your data. Copilot Editor will prompt you if it needs additional information about your objectives.

1. **View and edit the Pipeline plan:** Based on your selection, Copilot Editor generates a suggested Pipeline plan. It outlines:

   - Assumptions it made when generating the plan.
   - Transformations that it recommends based on your objectives.
   - Fields to keep and drop.

   > If needed, select **Edit** to further customize the proposed plan before confirming. For example, you could rename any fields as needed.
   >
   {.box .info}

   ![Copilot Editor Pipeline Plan](copilot-editor-pipeline-plan.png)
   {border="true"}

1. **Confirm the Pipeline plan and build the Pipeline:** After you confirm the plan, Copilot Editor builds a suggested Pipeline and displays a preview. Expand the Functions to explore the settings it applied. You can continue to edit the Pipeline at this stage as needed. You can also use natural language prompts in the chat window to request changes.

1. **Apply the Pipeline:** After you apply the Pipeline, select **Back to QuickConnect** at the top of the chat window to return to QuickConnect. Select **Save Connection** to add the Pipeline to your deployment.

   > Take note of the Pipeline ID displayed in the chat window so that you can find this Pipeline later.
   >
   {.box .info}

1. **Confirm the Pipeline is present:**  Confirm that the Pipeline ID you built with Copilot Editor appears in the Pipeline page or in QuickConnect. To find it in QuickConnect, select the connection that you used to launch Copilot Editor or its Pipeline icon, then select **Pipeline** to view the list of Pipelines for this connection.

### Edit or Update a Pipeline With Copilot Editor

Any time you run a Copilot Editor, it eventually asks you to choose an objective for that session. You can select **Edit an Existing Pipeline** as the objective. It then presents you with a list of Pipelines to select and edit.

With that in mind, you can start a Pipeline editing session with Copilot Editor from a few different entry points:

- Start a new Copilot Editor session, then select the **Edit an Existing Pipeline** as the objective.
- Open an existing Pipeline, select **Add Function** , then **Edit with Copilot Editor**. Select the **Edit an Existing Pipeline** as the objective.

> You can edit any existing Pipeline with Copilot Editor, not just Pipelines that were previously generated with Copilot Editor.
>
{.box .info}

## Guidelines and Better Practices

When working with Copilot Editor, keep these guidelines in mind:

- **Always review and validate Pipeline suggestions:** As with any AI tool, you should always check and confirm that the suggested changes match your expectations. Trust, but verify.
- **Copilot Editor may take different paths to meet an objective:** AI tools do not always solve problems in the exact same way every time, which sometimes takes some getting used to. This behavior is by design, but just be aware that the results may vary.

Use effective prompts to ensure the best results from the Copilot Editor. Some general guidelines:

- **Be specific:** To receive high-quality results, be clear and explicit with your requests. Provide exact names of fields that you want to reference or create.
- **Provide examples:** For instance, if you want to mask a specific piece of text, describe the target text in detail and explain what it should be masked to (for example, `Mask all IPv4 IP addresses to X.X.X.X`).
- **Ask questions:** Use the chat window to answer general questions about Pipelines. For example, you could ask the chatbot, `What Pipeline Functions can I use to redact sensitive information like IP addresses?`

## Feature Support

Cribl does not make any warranty regarding the availability, uptime, performance, or accuracy of Cribl Copilot, including PII Detection and Prevention. Please read the [Supplemental Terms for Cribl Copilot](https://cribl.io/legal/supplementalterms/) for more information.

### Requirements

Cribl Copilot Editor always requires internet access.

### Supported Deployment Types

Cribl Copilot Editor is available:

- For Cribl.Cloud or hybrid deployments.
- For Cribl Edge 4.12.0 and newer.

### Supported Schemas {#supported-schemas}

Cribl Copilot Editor specializes in transforming data into these schemas:

| Schema | Supported Version(s) |
| ------ | -------------------- |
| **OCSF**<br>Open Cybersecurity Schema Framework | <ul><li>1.6.0</li><li>1.5.0</li><li>1.4.0</li></ul> |
| **UDM**<br>Unified Data Model | <ul><li>6.0</li></ul> |
| **ECS**<br>Elastic Common Schema | <ul><li>9.0.0 (base version only)</li></ul> |

### Supported Functions and Integrations

Copilot Editor can build Pipelines using these Functions:

- [Aggregations](/stream/aggregations-function)
- [Code](/stream/code-function)
- [Comment](/stream/comment-function)
- [Drop](/stream/drop-function)
- [Eval](/stream/eval-function)
- [Flatten](/stream/flatten-function)
- [Lookup](/stream/lookup-function)
- [Mask](/stream/mask-function)
- [Numerify](/stream/numerify-function)
- [Regex Extract](/stream/regex-extract-function)
- [Regex Filter](/stream/regex-filter-function)
- [Rename](/stream/rename-function)
- [Serialize](/stream/serialize-function)
- [Unroll](/stream/unroll-function)

Copilot Editor can support all available Sources and Destinations.

### What Data the Copilot Editor Can Access

The Copilot Editor assistant has access to:

- The natural language query, written by the user.
- The sample input event selected by the user.

Copilot Editor has access to no other user or Organization data.
