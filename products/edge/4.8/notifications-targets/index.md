# Notification Targets


A Notification target specifies the delivery method for a Notification. Every Notification requires one or more targets. 

Available target types include:

- Webhook  
- PagerDuty integration
- Slack
- AWS SNS
- Email 

## Adding a Notification Target
 
To add a new Notification target from the [Manage Notifications](notifications#managing) page's **Targets** tab:

1. Click **Add Target** to open the **New Target** modal shown below.
2. Give this target a unique **Target ID**.
3. Select the desired **Target type**.

![Add targets on the **Targets** tab](notifications-targets.997cc1fbf2.png)
{border="true"}

Then configure the target as described in the appropriate page:

 - [Webhook](webhook-notification-targets)
 - [PagerDuty](pager-duty-notification-targets)
 - [Slack](slack-notification-targets)
 - [AWS SNS](aws-sns-notification-targets)
 - [Email](email-notification-targets) 

When you create a Notification target in Cribl Stream, Edge, or Search, it will also be available to you in the other products. For example, if you create a Notification target in Cribl Stream, you can also access it in Cribl Edge and Cribl Search. 

> Notifications require an Enterprise or Standard
> [license](licensing#license-types). Without an appropriate license, target configuration
> options will be hidden or disabled in Cribl Edge's UI.
>
{.box .info}

## Managing Notification Targets

You can manage targets by selecting **Manage** > **Notifications** > **Targets**.

Click any existing Notification target to open a modal where you can manage the target, using buttons at the bottom of the UI.

![Manage Notification targets](manage-notification-target-buttons.png)
{border="true", scale="100%"}

Click **Delete Target** to delete an existing Notification target. You can also select multiple targets for deletion, using the check boxes on the left side of the modal.

Click **Clone Target** to copy an existing target. A modal will open so you can modify the configuration of the target you started with.

To edit any target's definition in a JSON text editor, click **Manage as JSON** at the bottom of the **Notifications** > **Targets** modal. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing target configurations as `.json` files.

Some targets have additional controls for target configuration. See the documentation for the target of interest for more information.