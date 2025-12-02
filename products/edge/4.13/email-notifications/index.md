# Email Notifications


If you're a Cribl Edge admin, email notifications make it easy to receive alerts about any operational issues that require your attention, such as a particular Source or Destination condition or a pending license expiration.

An email Notification requires two things — a [configured Notification](#config-email-notifications) and an [email Notification target](/notifications-targets#email). Cribl.Cloud Organizations using [certain plan/license tiers](https://cribl.io/pricing/) also have access to a [preconfigured email Notification target](email-notification-targets#default-cloud-target).

Email Notifications do not have an unsubscribe option. Recipients who do not want to receive particular email Notifications should contact their Cribl Edge admins.

Email Notifications are sent on a no-reply basis.

## Configure Email Notifications {#config-email-notifications}

When you create a Notification for an email target, specify the recipients of the message, the subject line, and the contents of the message.

![Email Notification with target selected](email-notification-target-config-UI.png)
{scale="70%" border="true"}

**Target**: The ID of the email target to which you want to send the Notification.

Select **Add target** to add an existing target.

Select **Create target** to create a new target. For information on configuring email Notification targets, see [Email Notification Targets](/notifications-targets#email).

If the Notification already has a designated target, you can change the selection by selecting the drop-down.

When an email Notification target is selected, the following additional fields appear:

**To**: The email address of the recipient.

**Add cc**: When enabled, reveals a field where you can enter the addresses of additional recipients.

**Add bcc**: When enabled, reveals a field where you can enter the addresses of additional recipients that do not appear in the Notification email.

Cribl Edge does not limit the total number of recipients for a Notification, but your email service might set a limit.

**Subject**: The subject line of the email Notification. You can use [variables](#email-vars) in the subject line.

**Message**: The content of an email Notification. You can use [variables](#email-vars) in the body of the email message.

### Email Notification Variables {#email-vars}

Email variables are placeholders in the email template that get replaced with actual values when the email is sent. These variables can be:

- General-use variables, like `condition`, `worker_group`, or `timestamp`
- Special-purpose variables

You can use a variety of [general-use](#general-vars) and [special-purpose ](#special-vars) variables in the subject or message body of an email Notification. Insert a variable name between two braces preceded by a `$`. For example: `${cribl_notification}`.

#### General-Use Email Variables {#general-vars}

| Variable | Description |
| -----|----- |
| `workspace` | Workspace name (Cribl.Cloud only). |
| `organization` | Organization ID (Cribl.Cloud only). |
| `timestamp` | Timestamp when the email is sent. For example: `2019-08-04 18:22:24 UTC`. |
| `cribl_notification` | User-defined notification ID. |

#### Special-Purpose Email Variables {#special-vars}

| Variable | Usage | Notification type | Example |
| ----- | ----- | ----- | ----- |
| `_raw` | The `_raw` field of the Event triggering the Notification. | <div style="width: 200px"><ul><li>License expiration</li><li>Cribl Search</li></ul></div>| `Your Cribl license expires in 3 days on 2019-08-04 18:22:24 UTC. Please update your license to continue processing data after 2019-08-04 18:22:24 UTC.` |
| `__worker_group` | Name of the Worker Group triggering the Notification. | <ul><li>Destination Backpressure</li><li>High data volume</li><li>Low data volume</li><li>No data</li><li>Persistent Queue Usage</li><li>Unhealthy Destination</li></ul> | `default` |
| `timewindow` | Metric evaluation period used for Notification. | <ul><li>Destination Backpressure</li><li>High data volume</li><li>Low data volume</li><li>No data</li><li>Persistent Queue Usage</li><li>Unhealthy Destination</li></ul> | `60s` |
| `status` | Status of the Notification <br> Status of backpressure engagement. | <ul><li>Destination Backpressure</li><li>High data volume</li><li>Low data volume</li><li>No data</li><li>Persistent Queue Usage</li><li>Unhealthy Destination</li></ul> | `OK`, `ALARM` <br> `DISENGAGED`, `BLOCKING` or `DROPPING` |
| `input` | Type and name of a Source or Collector. | <ul><li>High data volume</li><li>Low data volume</li><li>No data</li></ul> | `syslog:in_syslog_1` |
| `output` | Type and name of a Destination. | <ul><li>Destination Backpressure</li><li>Persistent Queue Usage</li><li>Unhealthy Destination</li></ul> | `webhook:out_webhook_1` |
| `bytes` | Quantity of data triggering the specified Notification. | <ul><li>High data volume</li><li>Low data volume</li><li>No data</li></ul> | `0` |
| `dataVolume` | Data volume which triggered the Notification. Accepts numerals with units like KB, MB, and so on. | <ul><li>High data volume</li><li>Low data volume</li></ul> | `1G` |
| `health` | [Health metric](/internal-metrics#health) of the specified Source or Destination. | <ul><li>Unhealthy Destination</li></ul> | `0` |
| `queue_usage` | Percentage of capacity set in the **Usage threshold** field. | <ul><li>Persistent Queue Usage</li></ul> | `90` |
| `usage` | The actual usage, in percent. | <ul><li>Persistent Queue Usage</li></ul> | `50` |
| `usageThreshold` | The usage threshold, in percent. | <ul><li>Persistent Queue Usage</li></ul> | `50` |

## Troubleshoot Email Notifications {#troubleshooting-email-notifications}

The section details the troubleshooting steps you can take if an email Notification fails to reach its intended recipient.

### Test the Email Notification Target

An email Notification can fail if the target is misconfigured. You can test your email Notification target by following this procedure:

1. Open the target (in **Notifications** > **Targets**).
2. Select **Test Target**.
3. Add one or more email addresses to the **Test Target** modal and select **Send Test Email**.
4. Check the designated inbox to verify receipt of the test message. If it does not arrive in the designated inbox, review the target configuration.

### Check Notification Service Logs

A failed email Notification leaves a log entry. You can examine logs by following this procedure:

1. Select **Monitoring** > **Logs**.
2. In the drop-down on the left, select **Leader** > **Notifications Service**.
3. Examine any logs that have errors.

### Check Notifications

Cribl Edge stores Notifications. You can check them by following this procedure:

1. Select **Monitoring** > **Notifications**.
2. In the search field, search for **cribl_notification** fields that correspond to the Notification ID of the failed Notification.