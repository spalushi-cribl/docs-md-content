# Create Pipelines Using Cribl Copilot


The Cribl Copilot Pipeline assistant in Cribl Stream uses AI to help you create and edit data transformation Pipelines. Simply describe what you want the Pipeline to do using natural language, and Cribl Copilot will create the Pipeline for you. You’ll usually get results in about 10 seconds. 

 > The Cribl Copilot Pipeline assistant is available in Cribl.Cloud, and in on-prem installations of Cribl Stream v4.8 and newer. Cribl Copilot always requires internet access.   
 {.box .info}

The Cribl Copilot Pipeline assistant can generate Pipelines of any length using the following Functions:
* [CEF Serlializer](/stream/cef-serializer-function/)
* [Code](/stream/code-function/)
* [Comment](/stream/comment-function/)
* [DNS Lookup](/stream/dns-lookup-function/)
* [Drop](/stream/drop-function/)
* [Eval](/stream/eval-function/) 
* [Flatten](/stream/flatten-function/)
* [Mask](/stream/mask-function/)
* [Numerify](/stream/numerify-function/)
* [Regex Extract](/stream/regex-extract-function/)
* [Regex Filter](/stream/regex-filter-function/)
* [Rename](/stream/rename-function/)
* [Serialize](/stream/serialize-function/)
* [Suppress](/stream/suppress-function/)

 > Cribl does not make any warranty regarding the availability, uptime, performance, or accuracy of Cribl Copilot, including PII Detection and Prevention. Please read the [Supplemental Terms for Cribl Copilot](https://cribl.io/legal/supplementalterms/) for more information. 
 {.box .info}

**To create a Pipeline:**
1. In a Cribl Stream Worker Group, on the Worker Group top menu, select **Processing** and select **Pipelines**. 
1. Select **Add Pipeline**, and select **Add Pipeline with Cribl Copilot**.
Enter a name for your Pipeline in the **ID** field. The ID can contain only letters (A-Z, a-z), numbers (0-9), and the characters "_" and “-”.<br>
Optionally, change the async function timeout, and add a description or tags. <br>
Select **Save**. 
1. On the right, select an input event that your Pipeline will apply transformations to.
1. On the left, in the text box under **Build your Pipeline with Cribl Copilot**, type a prompt that describes what you want your Pipeline to do.
   
    ![Sample Pipeline prompt](copilot-pipeline-prompt.png)
    {scale="50%" border="true"}

    > Make sure you have an input event selected on the right before trying to send a Pipeline creation prompt. You will know an event is selected when it is outlined in blue.
    {.box .warning}
1. Select Send ![](copilot-send-icon.light.svg).
1. Confirm that the Pipeline performs the action you expected. If not, you can make edits manually, or cancel and try entering a new Pipeline description. 
1. Save the Pipeline.

## Example Prompts

The following are sample prompts you can try to better understand how Cribl Copilot uses information about Cribl Stream, Pipelines, your query, and the sample input event, and combines this information to generate a new Pipeline.

* **Redact PII (personally identifiable information).** Try a prompt like, “Redact any log data that may fall under GDPR compliance restriction”
*  **Reduce data storage costs by dropping unnecessary events.** Try a prompt like, “Filter out any logs that have a 200 status code or a null ‘error’ field”
*  **Enrich incoming data.** Try a prompt like, “Parse any IP addresses from my logs and then perform a DNS lookup on them. Place the IP and DNS hostname from the results in a separate top-level field.”

## How to Use Prompts Successfully

To ensure the best results from the Pipeline assistant: 

* **Be specific.** To receive high-quality results, be clear and explicit with your requests. Provide exact names of fields that you want to reference or create.
* **Provide examples.** For instance, if you want to mask a specific piece of text, describe the target text in detail and explain what it should be masked to (for example, `Mask all IPv4 IP addresses to X.X.X.X`).
* **Ask questions.** Use the Cribl Copilot [chatbot](/suite/copilot-chat/) to answer general questions about Pipelines. For example, you could ask the chatbot, `What Pipeline Functions can I use to redact sensitive information like IP addresses?`

## What the Pipeline Assistant Doesn’t Do

You can't edit an existing Pipeline Function; Cribl Copilot will always create new Functions and append them to the existing Pipelines. Also, this feature, like any AI, is still learning. It may not always work exactly as expected, so be sure to check the generated output events and logic carefully. Please provide feedback to help Cribl Copilot learn!

## What Data the Pipeline Assistant Can Access

The Cribl Copilot Pipeline assistant has access to:

* The natural language query, written by the user.
* The sample input event selected by the user.

Cribl Copilot has access to no other user or Organization data.
