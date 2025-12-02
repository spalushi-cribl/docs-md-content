# Source-State Notifications


In Cribl Edge 3.5 and above, you can configure Notifications on Sources and Collectors to trigger under these conditions:

- [High Data Volume](#high)
- [Low Data Volume](#low)
- [No Data](#no-data)

Read on for details about these conditions and how to configure appropriate Notifications.

## High Data Volume {#high}

Cribl Edge will generate a Notification when incoming data over your configured **Time window** exceeds your configured **Data volume** threshold. This selection exposes the following fields:

**Notification targets**: The **Default target** is always locked to `System Messages`. 

**Source name**: This field is locked to the Source on which you're setting this Notification.

**Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when the Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, and so forth.

**Data volume**: Enter the threshold above which a Notification will trigger. Accepts numerals with units like `KB`, `MB`, and so forth. For example: `4GB`. If you want the unit to be bytes, enter the numeral only, without a unit designator.

![Configuring a high data volume Notification](se-notifications-source-config.png)
{scale="70%" border="true"}

## Low Data Volume {#low}

Select the `Low Data Volume` tile to trigger Notifications when incoming data over your configured **Time window** is lower than your configured **Data volume** threshold. 

This selection exposes the same additional fields as [High Data Volume](#high), except that here, the **Data volume** value defines a **floor** below which the Notification will trigger.

## No Data {#no-data}

Select the `No Data Received` tile to trigger Notifications when the Source or Collector ingests zero data over your configured **Time window**.

 This selection exposes the same additional fields as [High Data Volume](#high), except it omits the **Data volume** field. With no data entering the Source, there is no threshold to configure.

 ## Source-State Notifications for Webhook Targets

 If you are sending a Source-state Notification to a Webhook Notification target, you can include a variety of expression fields in the target's **Source expression**. For more information, see:

- [Fields Common to All Notification Types](webhook-notification-targets#common-fields)
- [Source High Data Volume](webhook-notification-targets#source-hi-data)
- [Source Low Data Volume](webhook-notification-targets#source-lo-data)
- [Source No Data Received](webhook-notification-targets#source-no-data)

## Configuring Source Notifications  {#configuring-s}

To configure a Source-state Notification:

1. Configure and save the Source.
1. Access this Source's **Notifications** tab by one of the following methods: 
    - Click the **Notifications** button on the **Manage...Destinations** page's appropriate row.
    - Reopen the Source's config modal and click its **Notifications** tab.
2. Click **Add Notification** to access the **New Notification** modal shown below.

![Configuring a Source Notification](notification-source-tab.png)
{border="true"}

###  General

**ID**: Enter a unique ID for this Notification. Notifications are enabled by default, but you can disable the Notification by setting **Enabled** to `No`.

### Configuration

**When**: Select one of the following Notification tiles:

- **High Data Volume**
- **Low Data Volume**
- **No Data Received**

You can create multiple Notifications for the same Source, but you must configure them separately.

**Send notification to**: 
Click **Add Target** to send this Notification to additional targets. You can add multiple targets.
  - Use the resulting **Notification targets** drop-down to select any target you've already configured.
  - Click **Create Target** to configure a new target. 
 
See [Notification Targets](notifications-targets) for details.

**Default target** is always locked to `System Messages`. 

**Source name**: This field is locked to the Source on which you're setting this Notification.

{{< id `time-window-s` >}} **Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when a Destination or Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, and so forth. 

**Only notify on start and resolution**: When this option is set to `Yes`, Cribl Edge will send a Notification at the onset of the triggering condition. On resolution, it will send a second Notification.

If you don't enable this option and a Source-state Notification's trigger condition persists beyond your configured **Time window**, Cribl Edge will send a new Notification, once per **Time window** interval.

### Metadata

You can enter user-defined fields called *metadata*, which Cribl Edge includes in the notification payload. See [Metadata](notifications#metadata) for more information.