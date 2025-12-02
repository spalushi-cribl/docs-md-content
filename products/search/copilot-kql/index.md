# Write Queries Using Cribl Copilot


Use the Cribl Copilot KQL assistant to generate Kusto Query Language expressions from natural language.

---

The Cribl Copilot KQL assistant enables you to define search queries in natural language. The assistant translates these into [Kusto Query Language (KQL)](your-first-query#introduction). You can use all basic KQL features – including grouping, aggregating, filtering, and sorting – without having to write anything in KQL. You’ll usually get results in around 5 to 30 seconds. 

Additional options here are: 

- Selecting from automatically suggested [follow-up queries](#autocomplete).

- Pasting KQL queries into the assistant to generate natural-language summaries of their logic.

- Pasting problematic KQL queries into the assistant, to debug syntax that's triggered errors.

> To globally enable or disable Cribl Copilot for all Cribl products in your Organization, select ⚙️ **Settings** > **Global** > **AI Settings**.
>
{.box .success}

## Select from Cribl Copilot Suggestions {#autocomplete}

On the **Search Home** page, below the query box, Cribl Copilot automatically suggests follow-up queries that you might find useful, based on your most recently used query and Dataset, and on other recent search history.

![Cribl Copilot autocomplete options](search-copilot-query-prompts-1.png)
{scale="100%"}

You have multiple options to respond to these suggestions, covered in the following subsections.

### Accept a Suggestion {#accept}

You can select one of the three follow-up query tiles. As shown below, this will immediately execute the query, while placing it in the query box for you to refine, as desired.

![Suggested query after selection](search-copilot-query-prompts-2.png)
{scale="100%"}

### Request Explanations {#request}

You can toggle on **Explain queries** to expand all the suggested-query tiles. This will reveal details about each suggestion.

![Explain queries selected](search-copilot-query-prompts-3.png)
{scale="100%"}

### Roll Your Own {#write-own}

You can simply ignore the suggestions, and build your new follow-up queries in the query box. Or, use the upper query box to describe your desired search in plain (natural) language. As described in the [next section](#generate), Cribl Copilot will generate a corresponding query in KQL.

## Generate a Query Using Cribl Copilot {#generate}

To generate a KQL query from natural language: 

1. On the **Search Home** page, beside the search query box, select the Cribl Copilot button ![](copilot-icon.svg).
    
2. In the upper query box, describe what you want to find, using ordinary language. 
3. Select Send ![](copilot-send-icon.light.svg) to generate a KQL version of your query. 
4. Edit the resulting query if you want to make it more precise, and then select **Search**. 

>You can also use the Cribl Copilot [chatbot](/copilot/copilot-chat/) to generate queries. Select the Cribl Copilot button at the bottom right of any page, and then from the left drop-down select **Generate KQL**.
> 
>Unlike the KQL assistant, the chatbot has no access to Cribl Search, your query history, or your Datasets. So KQL generation here is limited. However, this might be a good option for organizations with strict privacy policies.
{.box .info}

## How to Use Prompts Successfully {#tips}

To ensure the best results from your natural language query: 

- **Be specific.** If you know the names of specific Datasets or fields, or have other constraints, include these in the prompt. Example:
`Sort results by column <my_timestamp>`

- **Refine your queries.** If the generated query is not exactly what you were looking for, refine your request to improve the result.

- **Provide feedback.** Select the thumbs-up or thumbs-down button to share feedback on responses. This helps improve the accuracy and relevance of Cribl Copilot.

## How the KQL Assistant Works {#how}

When you enter a natural language query using the Cribl Copilot KQL assistant, a large language model (LLM) uses information about KQL, your 10 last-used Datasets, and the query you've entered, and combines this information to generate a valid KQL query.

