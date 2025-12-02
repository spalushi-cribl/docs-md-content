# Notifications Overview


With Notifications, you can set up alerts that notify Cribl Stream admins about
issues that require their immediate attention.

## Types of Notifications

In Cribl Stream, you can configure Notifications about:

- Sources (including Cribl Stream Collectors) with high, low, or no data flow rates.
- Destinations that are experiencing backpressure, reporting errors, or
  approaching their persistent queue threshold. 
- Pending expiration of a Cribl Stream license.

Notifications are also sent as events to Cribl Stream
[internal logs](monitoring#internal-logs-and-metrics) – both application-wide,
and with a filtered view available on affected Sources and Destinations.
Cribl Stream stores these application-wide logs in `notifications.log` on the
Leader Node. The Leader Node is also responsible for sending all Notifications.

## License Requirements

Notifications require [certain plan/license tiers](https://cribl.io/pricing/). Without an appropriate
license, the configuration options described below will be hidden or disabled in
Cribl Stream. For more information on license types and a feature comparison, see
our [Pricing page](https://cribl.io/pricing/). 

## Notifications and Targets

A Notification target allows you to specify where Cribl Stream delivers a
Notification. Every Notification requires one or more targets. 

Available target types include:

- Webhook  
- PagerDuty integration
- Slack
- AWS SNS
- Email
- System Messages (default when you create a new Notification without a target).

For details, see [Notification Targets](notifications-targets).

When you create a new Notification, its default target type will be **System Messages**. 
When an event occurs in Cribl Stream to trigger a Notification condition, you will 
see an indicator in the <img className='inline' src="/images/page/icon-messages.svg"></img>
**Messages** icon. 

Select the icon to open the **Messages** drawer and view the Notification details.

## Notifications and RBAC

Notifications work with Cribl Stream role-based access control. For users with
non-administrative permissions, their assigned [Roles and Policies](roles)
determine the Worker Groups on which they can view Notification messages,
configure Notifications, and configure targets.

## Set up Notifications  {#implement-notifications}

To set up Notifications, you can create either the Notification or the target
first — the order doesn't matter. In general, this is the process for creating a
Notification:

- Create a Notification about a Source or Destination, or pending
  license expiration. 
- Add or create a Notification target. See [Targets Tab](#targets-tab) for more information. If
  you don't have a Notification target, the **Messages** icon will indicate you
  have a new Notification.
- Test the target to ensure it works.

### Create a Notification about a Source or Destination

To create a Notification for a Source or Destination:

1. Navigate to your Worker Group.
1. Select Data and choose your Source or Destination.
1. In the **Notifications** column, select **Notifications**.

For specific information about configuring these options, see [Source-State Notifications](source-state-notifications)
and [Destination-State Notifications](destination-state-notifications).

### Create a Notification about License Expiration

To create a license-expiration Notification:

1. Navigate to **Settings** > **Global** > **Licensing**.
1. Select **Add Expiration Notification**.

License-expiration Notifications are only available for customer-managed instances of Cribl Stream. For
specific information about configuring these options, see [License-Expiration
Notifications](license-expiration-notifications).

### General 

**ID**: Provide a unique ID for the Notification in this section. Cribl Stream
enables the Notification by default, but you can disable the Notification by
setting **Enabled** to `No`.

### Configuration

Define the triggering condition for the Notification in this section. Conditions
vary by Notification type, so consult one of the following topics for the
relevant details:

- [Source-State Notifications](source-state-notifications)  
- [Destination-State Notifications](destination-state-notifications)  
- [License-Expiration Notifications](license-expiration-notifications)  
 
Select **Add Target** to add a preexisting target. Select **Create Target** to
create a new target.

If you select or add an email Notification target, an additional set of
configuration options appear. See [Configuring Email
Notifications](email-notifications#config-email-notifications) for more
information.

### Metadata  {#metadata}

Metadata fields are user-defined fields included in the Notification payload.
All Notification types can contain metadata.

Select **Add field** here to add custom metadata fields to your Notifications in
the form of key-value pairs:

- **Name**: Enter a name for this custom field.

- **Value**: Enter a JavaScript expression that defines this field's value,
  enclosed in quotes or backticks. (Can evaluate to a constant.)

> Once you've saved your Notifications, you can see Notification events specific
> to this Destination on the **Events** tab within the Destination configuration
> window. When you set [Source-state](#source) Notifications, a corresponding
> **Events** tab is available on Source and Collector config modals. For a
> comprehensive view of all Notification events, see the system-wide
> [Events Tab](#events-tab).
>
{.box .success}

##  Manage Notifications  {#managing}

You can manage existing Notifications and targets by selecting **Notifications**
in the sidebar. You can also reach this page by selecting an existing
Notification in a Source or Destination modal or in **Settings** > **Global** >
**Licensing**.

###  Notifications Tab  {#notifications-tab}

This tab lists all your configured [Source‑state](#source) and [Unhealthy
Destination](#unhealthy) Notifications, across all integrations, along with any
configured [license-expiration](#license) Notifications. You can't create new
Notifications here, but you can disable or delete existing Notifications. For
any individual Notification, select **View Events** to see events that have
triggered the Notification.

To modify a Notification, select anywhere on its row. 

###  Targets Tab  {#targets-tab}

This tab is where you centrally configure and manage targets that are available
across Cribl Stream – for all Sources, Destinations, and license-based
Notifications. See [Notification Targets](notifications-targets) for details.

To create a new target, select **Add Target**. To delete a target, select
**Delete** in the appropriate row.