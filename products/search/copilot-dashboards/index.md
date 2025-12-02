# Add Visualizations Using Cribl Copilot


Use the Cribl Copilot AI assistant to generate Dashboard visualizations.

---

As an alternative to [designing Dashboard visualizations from scratch](creating-adding-to-dashboards), you can use the Cribl Copilot visualization assistant to analyze your Dataset and suggest effective display options. You can then adapt one of the initial suggestions, request different suggestions, or reject all suggestions and configure your own visualization.

> To globally enable or disable Cribl Copilot for all Cribl products in your Organization, select ⚙️ **Settings** > **Global** > **AI Settings**.
>
{.box .success}

## Add a Visualization from a Dataset {#dataset}

Cribl Copilot can suggest visualizations directly from the **Search Home** page. Hover your pointer over any Dataset to expose a **Visualize** button, as shown below.

![Cribl Copilot Visualize button](search-copilot-visualize-0.png)
{scale="100%"}

Select the button, and a **Visualize Dataset** drawer will open on the right. Here, Cribl Copilot will analyze the selected Dataset. After a few seconds, if the analysis is successful, the drawer will suggest three visualizations that might be useful in displaying the discovered data. Above the visualization panels, you will also see several buttons to generate alternative queries.

![Suggested visualizations for your Dataset](search-copilot-visualize-2-suggestions.png)
{scale="100%"}

You have the following options here:

- To accept one of the suggested visualizations, select its Actions ![](actions-dropdown.light.svg) button (upper right), then select **Add to Dashboard**.

- To substitute a new query and generate a different set of visualizations, select one of the three buttons at the top right. As shown below, this will open a corresponding natural language prompt in a new query box. Refine the query as desired, then select the Send ![Send icon](copilot-send-icon.light.svg) button – or press `Enter`/`return` – to see your new visualization options.

- Select **Write your own**. This will open a similar query box, where you can describe your own natural language query from scratch. As with the predefined query buttons, this will generate new suggested visualizations.

![Adapt a new query to suggest new visualizations](search-copilot-visualize-4-nl-result.png)
{scale="100%"}

## Add a Visualization when Creating a Dashboard {#creating}
 
When you add a new Dashboard, you can use Cribl Copilot to generate suggested visualizations.  

1. From the Cribl Search sidebar, select **Dashboards**. 
2. Select **Add Dashboard**. 
3. Give your Dashboard a name, configure any additional settings you want, and then select **Save**. 
4. Select **Add Visualization Using Copilot**.
   
   ![Add Visualization Using Copilot button](add-visualization-copilot.png)
   {scale="50%"}

5. Select the Dataset you want to use as your visualization's base.
> Soon, Cribl Copilot will display three recommended visualizations. (This could take up to several minutes with large Datasets.)
> 
6. Select **Add** to add any suggested visualization to your new Dashboard, or select **Refresh** ![](refresh-button.light.svg) to generate three new visualizations.
7. You're now working in edit mode (described in the following section).  
 > To add more visualizations, select the Cribl Copilot button ![](copilot-icon.svg) near the upper right. 

## Add a Visualization when Editing a Dashboard {#editing}

When you're editing an existing Dashboard, you can use Cribl Copilot to generate suggested visualizations.

1. From the Cribl Search sidebar, select **Dashboards**. 
2. Select the Dashboard you want to edit. 
3. Select the Actions ![](actions-dropdown.light.svg) button at the upper right, and then select **Edit**.
4. Near the upper right, select the Cribl Copilot button ![](copilot-icon.svg).
    >If the Dashboard is empty, you’ll instead see the **Add Visualization Using Copilot** option, described in the [preceding section](#creating).
    {.box .info} 
    >
5. From the list on the right, select the Dataset you want to use as your visualization's base.  

>Soon, Cribl Copilot will display three recommended visualizations. (This could take up to several minutes with large Datasets.)
>
6. Select **Add** to add any visualization to your new Dashboard, or select **Refresh** ![](refresh-button.light.svg) to generate three new suggested visualizations.

You have two additional options when editing an existing Dashboard: 

- To replace the Dataset used for your visualization, select **Change Dataset**. 

- To refine your results, describe your preferences in everyday language in the text box at the top of the Copilot drawer. 

![Sample prompt to suggest only donut charts](dashboard-copilot-prompt.png)
  {scale="60%"}
For more information about working with Dashboards in Cribl Search, see [Create a Dashboard](/search/creating-adding-to-dashboards/) and [Edit a Dashboard](/search/editing-dashboards/). 
