# Notification Targets


A Notification target specifies the delivery method for a Notification. Every Notification requires one or more targets. 

Available target types include:

- Webhook  
- PagerDuty integration
- Slack
- AWS SNS
- Email 

## Add a Notification Target
 
To add a new Notification target: 

1. In the Cribl Edge sidebar, select **Notifications**, and then select the **Targets** tab. 
2. Select **Add Target** to open the **New Target** modal.
3. Give this target a unique **Target ID**.
4. Select the desired **Target type**.

Then configure the target as described in the appropriate page:

 - [Webhook](webhook-notification-targets)
 - [PagerDuty](pager-duty-notification-targets)
 - [Slack](slack-notification-targets)
 - [AWS SNS](aws-sns-notification-targets)
 - [Email](email-notification-targets) 

When you create a Notification target in Cribl Stream, Edge, or Search, it will also be available to you in the other products. For example, if you create a Notification target in Cribl Stream, you can also access it in Cribl Edge and Cribl Search. 

> Notifications require [certain plan/license tiers](https://cribl.io/pricing/). Without an appropriate license, target configuration
> options will be hidden or disabled in Cribl Edge's UI.
>
{.box .info}

## Manage Notification Targets

You can manage targets by selecting **Notifications** > **Targets**.

Select any existing Notification target to open a modal where you can manage the target, using buttons at the bottom of the page.

Select **Delete Target** to delete an existing Notification target. You can also select multiple targets for deletion, using the check boxes on the left side of the modal.

Select **Clone Target** to copy an existing target. A modal will open so you can modify the configuration of the target you started with.

To edit any target's definition in a JSON text editor, select **Manage as JSON** at the bottom of the **Notifications** > **Targets** modal. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing target configurations as `.json` files.

Some targets have additional controls for target configuration. See the documentation for the target of interest for more information.