# Create and Manage Cribl Guard Rules

Cribl Stream ships with a Guard Rules Library that contains a set of pre-built
regular expression (regex) patterns for identifying sensitive data in your data
streams. When you add custom Sensitive Data Rules, they will also appear in this
library. The library serves as an easily accessible and searchable repository of
sensitive data rules and their regex patterns.

To access the library, select **Worker Groups** from the sidebar, and choose a Worker Group. 
Then, on the **Worker Groups** submenu, select **Processing > Knowledge > Guard Rules.**

![Cribl Guard rules Knowledge library](guard-rules-page.png)
{border="true" scale="85%"}

## Understand Cribl Guard Rules

Cribl Guard comes out-of-the-box with preconfigured rulesets that you can’t edit
or delete. You can identify these by the `Cribl` label on the rule in the
Knowledge library. 

Custom rules are labeled as such, and are created by users in your Workspace.
You can edit and delete custom rules.  By default, the `Cribl` rules appear
first in the list of rules. You can filter by `Custom` to view and edit the
rules you created. See [Cribl Guard Rulesets](guard-rulesets) for more information.

>A good way to create a custom rule is to clone a `Cribl` rule and then edit its details,
>adapting them to your specific usage requirements.
{.box .success}

## Add and Delete Cribl Guard Rules in the Knowledge Library

In the Knowledge library, users with an Editor-level or higher role can add and
edit Cribl Guard rules. When you edit existing Guard rules, the changes are
automatically propagated to all applicable rulesets after you commit and deploy.
You can either add rules manually or use Cribl Copilot as an assistant to create
your rules.

Once you have created a rule, you can verify it works as expected by selecting
a sample file and events. 

### Add a Cribl Guard Rule Manually

To create custom [Cribl Guard Rules](guard-rules-library):

1. Navigate to a Worker Group, then select **Processing > Knowledge**.  
1. Select **Guard Rules** from the Knowledge object menu.  
1. Select **Add Rule** and enter details in the **New Rule** form:

    | Field | Description |
    | :---- | :---------- |
    | **Rule ID** | A unique identifier for this rule.|
    | **Description** | An optional description of what the rule scans for.|
    | **Pattern to match** | The regex used to match data in your events. If you only add a regex, matching data is considered a potential match. Matches are highlighted in green in the sample events panel. Add anchor terms to improve sensitive data matching accuracy.|
    | **Anchor terms** | Text that you can add to provide additional context, which helps improve the accuracy of data matching and limits unnecessary data redaction. <ul><li>For example, adding the anchor terms `cc`, `creditcard`, and `visa` could help identify a 16-digit number string as a credit card instead of a similar, 16-digit UUID.</li> <li>Add up to six anchor terms to a rule.</li><li>Matches are highlighted in the sample events panel.</li></ul>|
    | **Add to rulesets** | One or more rulesets in which to include this rule.|

1. Validate your new rule by using sample data and events.
1. Select **Save** to start using your rule in Pipelines.

Cribl Guard will evaluate your existing rulesets and attempt to map the new rule
into a ruleset where logical. Otherwise, Cribl Guard will create a new
ruleset for the rule.



### Add a Cribl Guard Rule with Copilot AI Rule Builder

Cribl Copilot AI Rule Builder is an interactive, AI-powered assistant. You must enable Copilot
in order to use it to create Cribl Guard rules. Go to [Enable or Disable Cribl Copilot](/copilot/#enable)
for details.

To use Cribl Copilot to help you create rules: 

1. In **Processing > Library**, select **Add Rule**.  
1. Select **Add Rule with Copilot** from the drop-down.  
1. Describe the data you want to mask in your logs. Test the regex pattern and
   context words that Copilot suggests using sample events.  
1. Request changes to iterate and improve the final rule.  
1. Select **Save** when you're finished iterating.



![Creating a Cribl Guard rule with Cribl Copilot](guard-copilot-rule.png)
{border="true" scale="90%"}