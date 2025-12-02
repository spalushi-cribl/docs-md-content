# Notifications Overview


Notifications alert Cribl Stream admins about issues that require their immediate attention.

This page describes the uses for Cribl Stream Notifications, their license requirements, and how to configure them. 

## Types of Notifications

In Cribl Stream, you can configure Notifications about: 
- Sources (including Cribl Stream Collectors) with abnormal data flow rates.
- Sources (including Cribl Stream Collectors) with no data flow.
- Destinations experiencing backpressure, approaching their persistent queue threshold, or that report errors. 
- Pending expiration of a Cribl Stream license.

Notifications are also sent as events to Cribl Stream [internal logs](monitoring#internal-logs-and-metrics) – both application-wide, and with a filtered view available on affected Sources and Destinations. Cribl Stream stores these application-wide logs in `notifications.log` on the Leader Node. The Leader Node is also responsible for sending all Notifications.

## License Requirements

Notifications require an Enterprise or Standard license. Without an appropriate license, the configuration options described below will be hidden or disabled in Cribl Stream. For more information on license types and a feature comparison, see our [Pricing page](https://cribl.io/pricing/). 

## Notifications and Targets

A Notification target specifies the delivery method for a Notification. Every Notification requires one or more targets. 

Available target types include:

- Webhook  
- PagerDuty integration
- Slack
- AWS SNS
- Email 

For details, see [Notification Targets](notifications-targets).

By default, any Notification that you configure will have a target of `System Messages`. When a Notification condition is triggered, Cribl Stream will add an indicator on the top nav's **🔔 Messages** button. Click this button to view details in the **Messages** drawer. 

## Notifications and RBAC

Notifications work with Cribl Stream role-based access control. For users with non-administrative permissions, their assigned [Roles and Policies](roles) determine the Worker Groups on which they can view Notification messages, configure Notifications, and configure targets.

## Implementing Notifications  {#implement-notifications}

Implementing a Notification is a three-step process:

- Create a Notification about a Source state, Destination state, or pending license expiration. 
- Add or create a Notification target.
- Test the target to be sure it will work when needed.

You can create either the Notification or the target first — the order doesn't matter.

To create a Notification about a Source or Destination state, click **Add Notification** in the Source or Destination modal, **Notifications** tab.

To create a license-expiration Notification, click **Add Expiration Notification** in the **Global Settings** > **Licensing** UI.

### General 

**ID**: Provide a unique ID for the Notification in this section. Cribl Stream enables the Notification by default, but you can disable the Notification by setting **Enabled** to `No`.

### Configuration

Define the triggering condition for the Notification in this section. Conditions vary by Notification type, so consult one of the following topics for the relevant details:

- [Source-State Notifications](source-state-notifications)  
- [Destination-State Notifications](destination-state-notifications)  
- [License-Expiration Notifications](license-expiration-notifications)  
 
Click **Add Target** to add a preexisting target. Click **Create Target** to create a new target.

If you select or add an email Notification target, an additional set of configuration options appear. See [Configuring Email Notifications](email-notifications#config-email-notifications) for more information.

### Metadata  {#metadata}

Metadata fields are user-defined fields included in the notification payload. All Notification types can contain metadata.

Click **Add field** here to add custom metadata fields to your Notifications in the form of key-value pairs:

- **Name**: Enter a name for this custom field.

- **Value**: Enter a JavaScript expression that defines this field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

> Once you've saved your Notifications, you can see Notification events specific to this Destination on the Destination config modal's **Events** tab. (When you set [Source-state](#source) Notifications, a corresponding **Events** tab is available on Source and Collector config modals.) For a comprehensive view of all Notification events, see the system-wide [Events Tab](#events-tab).
>
{.box .success}

##  Managing Notifications  {#managing}

You can manage existing Notifications and targets from **Manage** > **Notifications**. You can also reach this UI by clicking an existing Notification in a Source or Destination modal or in the **Global Settings** > **Licensing** UI.

###  Notifications Tab  {#notifications-tab}

This tab lists all your configured [Source‑state](#source) and [Unhealthy Destination](#unhealthy) Notifications, across all integrations, along with any configured [license-expiration](#license) Notifications. You can't create new Notifications here, but you can disable or delete existing Notifications. For any individual Notification, click **View Events** to see events that have triggered the Notification.

![Notifications tab](notifications-tab.png)
{border="true"}

To modify a Notification, click anywhere on its row. 

###  Targets Tab  {#targets-tab}

This tab is where you centrally configure and manage targets that are available across Cribl Stream – for all Sources, Destinations, and license-based Notifications. See [Notification Targets](notifications-targets) for details.

![Targets tab](notifications-targets-tab.png)
{border="true"}

To create a new target, click **Add Target**. To delete a target, click **Delete** in the appropriate row.