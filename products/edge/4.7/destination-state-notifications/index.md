# Destination-State Notifications


In Cribl Edge 3.5 and above, you can configure Notifications on Destinations that will trigger under these conditions:

- [Destination Backpressure Activated](#backpressure)
- [Persistent Queue Usage](#pq)
- [Unhealthy Destination](#unhealthy)

Read on for details about these conditions and how to configure appropriate Notifications.

## Destination Backpressure Activated {#backpressure}

Cribl Edge will generate a Notification when one of the following events occurs:

- The Destination's **Backpressure behavior** is set to `Block` or `Drop`, and backpressure causes outgoing events to block or drop.
- The Destination's **Backpressure behavior** is set to [Persistent Queue](persistent-queues), but its **Queue‑full behavior** is set to either `Block` or `Drop new data`, and a filled queue causes the Destination to to block or drop outgoing events.

The threshold for the Notification to trigger is: Cribl Edge detected a blocked or dropped state during ≥ 5% of the trailing **Time window** that you configure in the [Configuring Destination Notifications](#configuring-d).

![Backpressure Notification](backpressure-notification.png)
{scale="70%" border="true"}

## Persistent Queue Usage {#pq}

Cribl Edge will generate a `Persistent Queue usage has surpassed <threshold>%` Notification when the [PQ](persistent-queues) accumulates files past the `<threshold>` percentage of capacity that you set in the **Usage threshold** field. This field appears only when you configure a Notification for **Persistent Queue Usage**.

![Persistent Queue Notification](notification-pq.png)
{scale="70%" border="true"}

## Unhealthy Destination {#unhealthy}

Cribl Edge will generate a `Destination <name> is unhealthy` Notification when the Destination's health has been in red status (as indicated on the UI's [Monitoring](monitoring) page) over the trailing **Time window** that you configure in [Configuring Destination Notifications](#configuring-d). 

The algorithm has slight variations among Destination types, but red status generally means that ≥ 5% of health checks, aggregated over the **Time window**, reported one of the following conditions:

- An error inhibiting the Destination's normal operation, such as a connection error.
- For multiple-output Destinations like [Splunk Load Balanced](destinations-splunk-lb) or [Output Router](destinations-output-router), \> 50% of the Destination senders are in an error state.

![Destination Unhealthy Notification](destination-unhealthy-notification.png)
{scale="70%" border="true"}

 ## Destination-State Notifications for Webhook Targets

 If you are sending a Destination-state Notification to a Webhook Notification target, you can include a variety of expression fields in the target's **Source expression**. For more information, see:

- [Fields Common to All Notification Types](webhook-notification-targets#common-fields)
- [Unhealthy Destination](webhook-notification-targets#unhealthy-dest)
- [Destination Backpressure Activated](webhook-notification-targets#dest-bp-activated)

## Configuring Destination Notifications {#configuring-d}

To configure a Destination-state Notification:

1. Configure and save the Destination.
1. Access this Destination's **Notifications** tab by one of the following methods: 
    - Click the **Notifications** button on the **Manage...Destinations** page's appropriate row,  or
    - Reopen the Destination's config modal and click its **Notifications** tab.
1. Click **Add Notification** to access the **New Notification** modal shown below.

![Configuring a Destination Notification](notification-dest-tab.png)
{border="true"}

### General 

**ID**: Enter a unique ID for this Notification. Notifications are enabled by default, but you can disable the Notification by setting **Enabled** to `No`.

### Configuration

**When**: Select one of the following Notification tiles:

- **Destination Backpressure Activated**
- **Persistent Queue Usage**
- **Unhealthy Destination**

You can set up multiple Notifications for the same Destination, but you must configure them separately.

**Send Notification to**: 
Click **Add Target** to send this Notification to additional targets. You can add multiple targets.
  - Use the resulting **Notification targets** drop-down to select any target you've already configured.
  - Click **Create Target** to configure a new target. 
 
See [Notification Targets](notifications-targets) for details.

**Default target** is always locked to `System Messages`. 

**Destination name**: This field is locked to the Destination on which you're setting this Notification.

{{< id `time-window-d` >}} **Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when a Destination or Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, and so forth. 

**Only notify on start and resolution**: When this option is set to `Yes`, Cribl Edge will send a Notification at the onset of the triggering condition and a second Notification to report its resolution.

If you don't enable this option and a Destination-state Notification's trigger condition persists beyond your configured [Time window](#time-window-d), Cribl Edge will send a new Notification, once per **Time window** interval.

### Metadata

You can enter user-defined fields called *metadata*, which Cribl Edge includes in the notification payload. See [Metadata](notifications#metadata) for more information.