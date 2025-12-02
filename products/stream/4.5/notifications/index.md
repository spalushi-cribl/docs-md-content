# Notifications


Notifications alert Cribl Stream admins about issues that require their immediate attention.

In Cribl Stream (LogStream) 3.1 or later, and all Cribl Edge versions, you can configure Notifications about: 
- Sources and Collectors that report abnormally high or low data flow rates.
- Sources and Collectors that report no data flow.
- Destinations experiencing backpressure. 
- Destinations approaching their persistent queue threshold.
- Destinations that report errors.
- Pending expiration of a Cribl Stream license. 

Notifications are also sent as events to Cribl Stream's [internal logs](monitoring#internal-logs-and-metrics) – both application-wide, and with a filtered view available on affected Sources and Destinations. The application-wide logs are recorded as `notifications.log` on the Leader Node. The Leader Node is also responsible for sending all Notifications.

Notifications are not designed to take the place of alerts on your overall infrastructure's health, but they warn you about conditions that could impede expected data flow into and out of Cribl Stream.

> Notifications require an Enterprise or Standard [license](licensing), without which the configuration options described below will be hidden or disabled in Cribl Stream's UI.
>
{.box .success}

## Notifications and Targets

Every Notification is sent to one or more targets. By default, any Notification that you configure will have a target of `System Messages`. This means that when a Notification is triggered, it will add an indicator on the top nav's **🔔 Messages** button. Click this button to view details in the **Messages** drawer. 

You can also send any Notification to additional targets, including a Webhook, PagerDuty integration, Slack, AWS SNS, or email. For details, see [Configuring Targets](notifications-targets).

## Notifications and RBAC

Notifications work with Cribl Stream's role-based access control. For users with non-administrative permissions, their assigned [Roles and Policies](roles) determine the Worker Groups on which they can view Notification messages, and can create and manage Notifications and targets.

## Configuring Notifications  {#configuring}

Destination-state, Source-/Collector-state, and license-expiration Notifications are configured separately.

Clicking **New Notification** brings up **Notification Settings** modals, whose controls are listed in the respective sections below.

See [Metadata](#metadata) for metadata settings common to all Notification types.

### Destination-State Notifications  {#destination}

On individual Destinations, you can configure Notifications that will trigger under these conditions:

- [Destination Backpressure Activated](#backpressure)
- [Persistent Queue Usage](#pq)
- [Unhealthy Destination](#unhealthy)

Read on for details about these conditions and how to configure appropriate Notifications.

#### Destination Backpressure Activated {#backpressure}

This will generate a Notification when one of the following events occurs:

- The Destination's **Backpressure behavior** is set to `Block` or `Drop`, and backpressure causes outgoing events to block or drop.
- The Destination's **Backpressure behavior** is set to [Persistent Queue](persistent-queues), but its **Queue‑full behavior** is set to either `Block` or `Drop new data`; and a filled queue causes the Destination to to block or drop outgoing events.

The threshold for the Notification to trigger is: Cribl Stream detected a blocked or dropped state during ≥ 5% of the trailing **Time window** that you configure in the [Notification Settings](#notification-settings-d).

![Backpressure Notification](backpressure-notification.png)
{scale="70%" border="true"}

#### Persistent Queue Usage  {#pq}

This will generate a `Persistent Queue usage has surpassed <threshold>%` Notification when the [PQ](persistent-queues) accumulates files past the `<threshold>` percentage of capacity that you set in the **Usage threshold** field. This field appears only when you configure a Notification for **Persistent Queue Usage**.

![Persistent Queue Notification](notification-pq.png)
{scale="70%" border="true"}

#### Unhealthy Destination  {#unhealthy}

This will generate a `Destination <name> is unhealthy` Notification when the Destination's health has been in red status (as indicated on the UI's [Monitoring](monitoring) page) over the trailing **Time window** that you configure in [Notification Settings](#notification-settings-d). 

The algorithm has slight variations among Destination types, but red status generally means that ≥ 5% of health checks, aggregated over the **Time window**, reported either:

- An error inhibiting the Destination's normal operation, such as a connection error; or
- For multiple-output Destinations like [Splunk Load Balanced](destinations-splunk-lb) or [Output Router](destinations-output-router), \> 50% of the Destination senders are in an error state.



![Destination Unhealthy Notification](destination-unhealthy-notification.png)
{scale="70%" border="true"}

#### Configuring Destination Notifications  {#configuring-d}

To start configuring a Destination-state Notification:

1. Configure and save the Destination.
1. Access this Destination's **Notifications** tab. Either: 
    - Click the **Notifications** button on the **Manage...Destinations** page's appropriate row,  or
    - Reopen the Destination's config modal and click its **Notifications** tab.
1. Click **Add Notification** to access the **New Notification** modal shown below.

![Configuring a Destination Notification](notification-dest-tab.png)
{border="true"}

####  General 

**ID**: Enter a unique ID for this Notification.

#### Configuration

**When**: Select one of the following Notification tiles:

- **Destination Backpressure Activated**
- **Persistent Queue Usage**
- **Unhealthy Destination**

You can set up multiple Notifications for the same Destination, but you must configure them separately.

**Send Notification to**: 
Click **Add Target** to send this Notification to additional targets. You can add multiple targets.
  - Use the resulting **Notification targets** drop-down to select any target you've already configured.
  - Click **Create Target** to configure a new target. 
 
See [Configuring Targets](notifications-targets) for details.

**Default target** is always locked to `System Messages`. 

**Destination name**: This field is locked to the Destination on which you're setting this Notification.

{{< id `time-window-d` >}} **Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when a Destination or Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, etc. 

**Only notify on start and resolution**: When this option is set to `Yes`, Cribl Stream will send a Notification at the onset of the triggering condition and a second Notification to report its resolution.

If you don't enable this option and a Destination-State Notification's trigger condition persists beyond your configured [Time window](#time-window-d), Cribl Stream will send a new Notification, once per **Time window** interval.



### Source-State Notifications  {#source}

In Cribl Stream 3.5 and above, you can configure Notifications on Sources and Collectors to trigger under these conditions:

- [High Data Volume](#high)
- [Low Data Volume](#low)
- [No Data](#no-data)

Read on for details about these conditions and how to configure appropriate Notifications.

#### High Data Volume {#high}

This will generate a Notification when incoming data over your configured **Time window** exceeds your configured **Data volume** threshold. This selection exposes the following fields:

**Notification targets**: The **Default target** is always locked to `System Messages`. 

**Source name**: This is locked to the Source on which you're setting this Notification.

**Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when the Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, etc.

**Data volume**: Enter the threshold above which a Notification will trigger. Accepts numerals with units like KB, MB, etc. For example: `4GB`.

![Configuring a High Data Volume Notification](se-notifications-source-config.png)
{scale="70%" border="true"}

#### Low Data Volume {#low}

Select the `Low Data Volume` tile to trigger Notifications when incoming data over your configured **Time window** is lower than your configured **Data volume** threshold. 

This selection exposes the same additional fields as [High Data Volume](#high), except that here, the **Data volume** value defines a **floor** below which the Notification will trigger.

#### No Data {#no-data}

Select the `No Data Received` tile to trigger Notifications when the Source or Collector ingests zero data over your configured **Time window**.

 This selection exposes the same additional fields as [High Data Volume](#high), except (for obvious reasons) it omits the **Data volume** field – there is no threshold, because this is a binary condition.

#### Configuring Source Notifications  {#configuring-s}

To start configuring a Source-state Notification:

1. Configure and save the Source.
1. Access this Source's **Notifications** tab. Either: 
    - Click the **Notifications** button on the **Manage...Destinations** page's appropriate row,  or
    - Reopen the Source's config modal and click its **Notifications** tab.
1. Click **Add Notification** to access the **New Notification** modal shown below.

![Configuring a Source Notification](notification-source-tab.png)
{border="true"}

####  General

**ID**: Enter a unique ID for this Notification.

#### Configuration

**When**: Select one of the following Notification tiles:

- **High Data Volume**
- **Low Data Volume**
- **No Data Received**

You can set up multiple Notifications for the same Source, but you must configure them separately.

**Send notification to**: 
Click **Add Target** to send this Notification to additional targets. You can add multiple targets.
  - Use the resulting **Notification targets** drop-down to select any target you've already configured.
  - Click **Create Target** to configure a new target. 
 
See [Configuring Targets](notifications-targets) for details.

**Default target** is always locked to `System Messages`. 

**Source name**: This field is locked to the Source on which you're setting this Notification.

{{< id `time-window-s` >}} **Time window**: This field's value sets the threshold period before the Notification will trigger. The default `60s` will generate a Notification when a Destination or Source has reported the trigger condition over the past 60 seconds. To enter alternative numeric values, append units of `s` for seconds, `m` for minutes, `h` for hours, etc. 

**Only notify on start and resolution**: When this option is set to `Yes`, Cribl Stream will send a Notification at the onset of the triggering condition and a second Notification to report its resolution.

If you don't enable this option and a Source-State Notification's trigger condition persists beyond your configured **Time window**, Cribl Stream will send a new Notification, once per **Time window** interval.

###  License-Expiration Notifications  {#license}

To prevent interruptions in data throughput, you can configure a Notification that will be triggered two weeks before your Cribl Stream [paid license](licensing#license-types) expires, and then again upon expiration. (If the two-week Notification is cleared from the **💬 Messages** tab between those dates, but the license has not been extended, it will trigger again.) 

####  Configuring License-Expiration Notifications  {#configuring-license}

1. From the top nav, select **Settings** > (**Global Settings** >) **Licensing**. 
1. Click **Add expiration notification** to access the **New Notification** modal shown below.

![Configuring a license expiration Notification](license-notifications-config-modal.png)
{border="true"}

This **New Notification** modal provides [General](#general-l), [Configuration](#configuration-l), and [Metadata](#metadata) tabs, with a subset of the controls available in the [Unhealthy Destination modal](#unhealthy).

####  General  {#general-l}

**ID**: Enter a unique ID for this Notification.

#### Configuration {#configuration-l}

**When**: This modal's triggering condition is locked to `License Expiration`.

**Send notification to**: This section contains a list of the targets receiving this Notification. The **Default target** is always locked to `System Messages`. 

Click **Add Target** for each additional existing target that you want to send this Notification to. Click **Create Target** to create a new target for the Notification.

## Email Notifications {#email-notifications}

If you're a Cribl Stream admin, email notifications make it easy to receive alerts about any operational issues that require your attention, such as a particular Source or Destination condition or a pending license expiration.

In [Cribl Search](/search/), you can use [email Notifications](/search/notification-targets#email) to send alerts about specific conditions in the data.

An email Notification requires two things — a [configured Notification](#config-email-notifications) and an [email Notification target](/notifications-targets#email).

> Email Notifications require an Enterprise license.
{.box .info}


### Configuring Email Notifications {#config-email-notifications}

When you create a Notification for an email target, specify the recipients of the message, the subject line, and the contents of the message.

![Email Notification target](email-notification-target-config-UI.png)
{scale="70%" border="true"}

**Target**: The ID of the email target to which you want to send the Notification. 

Click **Add target** to add an existing target. 

Click **Create target** to create a new target.

If the Notification already has a designated target, you can change change the selection by clicking the drop-down.

When an email notification target is selected, the following additional fields appear: 

**To**: The email address of the recipient.

**Add cc**: When enabled, reveals a field where you can enter the addresses of additional recipients.

**Add bcc**: When enabled, reveals a field where you can enter the addresses of additional recipients that do not appear in the Notification email.

Cribl Stream does not limit the total number of recipients for a Notification, but your email service might set a limit.

**Subject**: The subject line of the email Notification. You can use [variables](#email-vars) in the subject line.

**Message**: The content of an email Notification. You can use [variables](#email-vars) in the body of the email message.

For information on configuring email Notification targets, see [Email](/notifications-targets#email).

#### Email Notification Variables {#email-vars}

Email variables are placeholders in the email template that get replaced with actual values when the email is sent. These variables can be common variables (like `condition`, `worker_group`, `timestamp`, etc.), event-specific variables, or condition-specific variables. 

You can use a variety of [general-use](#general-vars) and [special-purpose ](#special-vars) variables to configure email Notifications. Insert a variable name between two braces preceded by a `$`. For example: `${cribl_notification}`.

##### General-Use Email Variables {#general-vars}

Variable | Description
-----|-----|
`workspace` | Workspace name (Cribl.Cloud only).
`organization` | Organization ID (Cribl.Cloud only).
`timestamp` | Timestamp when the email is sent. For example: `2019-08-04 18:22:24 UTC`.
`cribl_notification` | User-defined notification ID.

##### Special-Purpose Email Variables {#special-vars}

Variable | Usage | Example
----- | ----- | -----
`input` | Type and name of a Source or Collector. | `syslog:in_syslog_1`
`output` | Type and name of a Destination. | `webhook:out_webhook_1`
`bytes` | Quantity of data triggering the specified Notification. | `0`
`starttime` | Start time of an Event (in Epoch seconds). | `1706747185`
`endtime` | End time of an Event (in Epoch seconds). | `1706747294`
`health` | [Health metric](/internal-metrics#health) of the specified Source or Destination. | `0`
`_raw` | The `_raw` field of the Event triggering the Notification. | `_raw (Source ${name} in group ${__workerGroup} traffic volume greater than ${dataVolume} in ${timeWindow})`
`backpressure_type` | Backpressure behavior.  | `1=BLOCKING`, `2=DROPPING`
`queue_usage` | Percentage of capacity set in the **Usage threshold** field for a [**Persistent Queue Usage** Notification](#pq). | `90`

### Metadata  {#metadata}

Metadata fields are user-defined fields that are included in the notification payload. They do not appear in the email message.

Click **Add field** here to add custom metadata fields to your Notifications in the form of key-value pairs:

**Name**: Enter a name for this custom field.

**Value**: Enter a JavaScript expression that defines this field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

> Once you've saved your Notifications, you can see Notification events specific to this Destination on the Destination config modal's **Events** tab. (When you set [Source-state](#source) Notifcations, a corresponding **Events** tab is available on Sources' and Collectors' config modals.) For a comprehensive view of all Notification events, see the systemwide [Events Tab](#events-tab).
>
{.box .success}

### Troubleshooting Email Notifications {#troubleshooting-email-notifications}

The section details the troubleshooting steps you can take if an email notification fails to reach its intended recipient.

#### Test the Email Notification Target

An email notification can fail if the target is misconfigured. You can test your email notification target by following this procedure:

1. Open the target (in **Manage** > **Notifications** > **Targets**). 
2. Click **Test Target**.
3. Add one or more email addresses to the **Test Target** modal and click **Send Test Email**.
4. Check the designated inbox to verify receipt of the test message. If it does not arrive in the designated inbox, review the target configuration.

#### Check Notification Service Logs

A failed email notification leaves a log entry. You can examine logs by following this procedure:

1. Select **Monitoring** > **Logs**.
2. Open the **Logs** drop-down and select **Leader** > **Notifications Service**.
3. Examine any logs that have errors.

#### Check Notifications

Cribl Stream stores Notifications. You can check them by following this procedure:

1. Select **Monitoring** > **Notifications**.
2. Select the **cribl_notification** field.
3. Search for **cribl_notification** fields corresponding to the Notification ID of the failed Notification.

##  Managing Notifications  {#managing}

The **Notifications** page provides global display and controls for all your configured Notifications, targets, and triggered Events – across all Sources, Collectors, Destinations, and all Worker Groups. To access this page: From the top nav, select **Manage** > **Notifications**.



###  Notifications Tab  {#notifications-tab}

This tab lists all your configured [Source‑state](#source) and [Unhealthy Destination](#unhealthy) Notifications, across all integrations, along with any configured [license-expiration](#license) Notifications. You can't create new Notifications here, but you can disable or delete existing Notifications. You can also click on any Notification's row to modify its configuration.

![Notifications tab](notifications-tab.png)
{border="true"}

###  Targets Tab  {#targets-tab}

This tab is where you centrally configure and manage targets that are available across Cribl Stream – to all Sources, Destinations, and license-based Notifications. See [Configuring Targets](notifications-targets) for details.


