# Notifications


Send Notifications about your [scheduled searches](scheduled-searches).

---

Cribl Search can alert you when your scheduled searches generate results that match triggering conditions that you specify. You set up alerts in these basic stages:

1. Optionally, configure any custom [Notification targets](#targets) to which you want to send alerts. (Cribl Search comes preconfigured with at least one default `System Messages`. target, Some plans add a preconfigured `system_email`.)
1. Save and schedule each search from which you want to trigger alerts.
1. Configure one or more [Notifications](#configure) on each scheduled search.


> You can't configure Notifications on a saved or scheduled search that resides within a [Pack](packs).
{.box .info}

## Notification Targets {#targets}

A Notification target specifies the delivery method for a Notification. Every Notification must have at least one target. Available target types are:




- System Messages: Banner messages in the Cribl Search UI (the default target).
- [Email](email-notifications) messages.
- [Amazon SNS](aws-sns-notification-targets) topics.
- [PagerDuty](pager-duty-notification-targets) accounts.
- [Slack](slack-notification-targets) channels.
- [Webhook](webhook-notification-targets) connections.


By default, any Notification that you configure will have a `System Messages` target. Also, your Notification will include this target regardless of any other target that you add. When a Notification condition is triggered, Cribl Search will add an indicator on the top nav's **🔔 Messages** button. Here, select this button to view details in the **Messages** drawer.

### Add a Notification Target {#add-target}

To add a new Notification target:

1. In Cribl Search, go to **Settings** > **Search** > **Notification Targets**.
1. Select **Add Target** to open the **New Target** modal.
2. Give this target a unique **Target ID**.
3. Select the desired **Target type**.
1. Configure the target as outlined on that target's appropriate page (see [Notification Targets](#targets)).

You can also use the Cribl API to [create a Notification target](/cribl-as-code/save-search-get-alerts/#create-target).

When you create a Notification target in Cribl Search, Stream, or Edge, it will also be available to you in the other products. For example, if you create a Notification target in Cribl Search, you can also access that target in Cribl Stream and Cribl Edge. (Where you would, of course, configure very different Notification conditions.)

### Manage Notification Targets {#manage-targets}

You manage existing targets on the same **Notification Targets** page where you create targets.

Select any existing Notification target to open a modal where you can manage the target, using buttons at the bottom of the configuration modal.

Select **Clone Target** to copy an existing Notification target. This refreshes the modal, enabling you to customize the new target.

Select **Delete Target** to delete an existing target.

To edit any target's definition in a JSON text editor, select **Manage as JSON** at the bottom of the **Notifications Targets** modal. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to share existing target configurations as `.json` files.

## Configure Notifications {#configure}

To configure how, when, and where to send Notifications:

1. [Schedule a search](scheduled-searches#schedule-a-search),
or [open an existing scheduled search](scheduled-searches#manage-scheduled-searches).
2. In the search's configuration modal, open the **Notifications** left tab.
3. Toggle **Send notifications** on, and configure the options listed below. 

   > The **Notifications** left tab unlocks only after you enable the **Schedule** tab's **Run on schedule** option. 
   >
   >To configure further Notifications on the same scheduled search, select **Add Notification**.
   {.box .info}

You can also use the Cribl API to [create a Notification](/cribl-as-code/save-search-get-alerts/#send-alerts).

### Notification Options {#notification-options}

As you configure each Notification, the **Notifcations** tab offers the following options.

- **When**: Select the type of trigger condition, then define the corresponding trigger. (See [Notification Triggers](#triggers).)

- **Send Notification to**: Select an existing Notification target to deliver the Notification, or select **Create** to create a new target. (For target options, see [Notification Targets](#targets).)

  > All Notifications, regardless of their configured target, will also generate a message in the Cribl Search UI.
  {.box .info} 


- **Include search results**: You must toggle this on if you want to insert the `{{resultSet}}` template variable into the **Message** field, in order to include or attach complete search results with Notifications. (See [Message Options](#message-options) below for details and payload limits.) However, this is not required in order to include the contents of specified events and fields, in the format: `{{resultSet[0].<field-name>}}`. To see how each alert type implements this option, follow the links from [Notification Targets](#targets).

- **Message**: Message payload template. For details, see [Message Options](#message-options) below.

#### Message Options {#message-options}

The message sent with each Notification can contain a maximum of 1,000 characters, and a maximum of 100 events x 20 fields.

In the **Message** pane, you can customize each Notification's contents by inserting the template variables listed below. These are placeholders to populate your message with fields generated by the search. Insert a variable between pairs of double braces, in this format: `{{savedQueryId}}`. 

Here's an example that inserts some literal text followed by a corresponding variable: `Date: {{timestamp}}`. This is the first string in the default template, shown below.


![ **Message** pane's Default template](search-message-template.png)
{scale="100%"}

The following template variables are available.

| Variable | Description |
| -------- | ----------- | 
| <div style="width: 200px"> `resultSet` | Array containing complete results of the search, up to the [maximum message size](#message-options). |
| `{{resultSet[<event-number>].<field‑name>}}` | Contents of a specified event and field from the search results. |
| `savedQueryId` | ID of the saved search that triggered the Notification. |
| `searchId` | ID of the search job. |
| `searchResultsUrl` | URL corresponding to the search job results. |
| `notificationId` | ID of this Notification (autogenerated). |
| `timestamp` | Date when this Notification was triggered. |
| `tenantId` | ID of the Cribl Organization. |

If you choose to explicitly add the `{{resultSet}}` variable to the message body, this will have different effects with different targets:

- With a [PagerDuty](pager-duty-notification-targets) or [Slack](slack-notification-targets) Notification, you can include `{{resultSet}}` to control where inline search results will be displayed within alerts. 

- With an [Email](email-notification-targets) Notification, including `{{resultSet}}` will insert placeholder text indicating that the results will appear either "in the table below" (if you've selected **Send as**: **Inline table**) or "attached" (if you've selected **Send as**: **CSV** or **JSON**).

- With an [Amazon SNS](aws-sns-notification-targets) or [Webhook](webhook-notification-targets) Notification, including `{{resultSet}}` will send duplicate result sets – one each in the message and on the payload.


> In order to insert the complete `{{resultSet}}` template variable, you must toggle on **Include search results**. This is not required in order to insert individual events/fields.
{.box .info}

## Notification Triggers {#triggers}

On the **Notifications** left tab, use the **When...** controls to define the trigger condition that will send the Notification. The drop-down here provides two options.

- **Where**: Select this to define a custom condition. See [Notification Trigger Examples](#trigger-examples) below.

- **Count of Results**: Exposes a drop-down where you can select a comparison operator, and a field where you can enter the corresponding threshold number.

### How Notification Triggers Work {#trigger-details}

Cribl Search will send a Notification when at least one event satisfies the trigger condition. This can be useful if you want to define a search query that returns multiple results, but want alerts sent only when a specific condition among these results is met.

### Notification Trigger Examples {#trigger-examples}

For a query like this:
 
```kusto {runSearch=true}
dataset="cribl_internal_logs" error 
| summarize errorCount=count() by message 
| top 10 by errorCount
```

You could enter this **Where** condition to alert when `Request failed` is in the top 10 errors:

```
message == "Request failed"
```

For a query like this:

```kusto {runSearch=true}
dataset="cribl_search_sample" log_status="OK" 
| summarize count() by action
```

You could enter this **Where** condition to alert upon connection errors:

```
action == "REJECT"
```
