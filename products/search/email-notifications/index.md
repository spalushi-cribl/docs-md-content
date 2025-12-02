# Email Notifications


Send email Notifications to alert on your [scheduled searches](scheduled-searches).

---

If you're a Cribl Search Admin, email Notifications make it easy for you (or
designated other people) to receive alerts about operational issues that merit
your attention, such as a scheduled search's execution details.

An email Notification requires two things – a [configured Notification](#config-notifications) and an [email Notification target](#targets). Organizations on an Enterprise plan also have access to a [preconfigured email Notification target](#default-cloud-target).

Email Notifications do not provide an unsubscribe option. Recipients who do not
want to receive particular email Notifications should contact their Cribl Search
Admins.

Email Notifications are sent on a no-reply basis.

## Email Notification Targets {#targets}

You can send Notifications by email, using an SMTP server of your choice. Once you have configured the email Notification target, you can specify the recipients, and customize the subject line and message content. For details, see [Configure Email Notifications](#config-notifications).

To add an email Notification target in Cribl Search, do one of the following:

- Open a saved or scheduled search, then select **Notifications**. Toggle **Send notifications** on, if
  necessary, then configure the target.
- From the Cribl Search sidebar, select **Settings**, then **Search**, then **Notification Targets**, then **Add Target**.

If you have an Enterprise plan, you can use the preconfigured [default Email Notification target](#default-cloud-target).

### General Settings {#general}

**Target ID**: Enter a unique name to identify this email Notification target.

### Configuration {#configuration}

**Address**: Identify the SMTP server by its hostname or IP address.

**Port**: Set the SMTP port. Use port `587` for SMTP Secure (SMTPS). You can also use port `25`. Use `465` when SSL/TLS
is enabled. You can also use port `2525` if your email service provider supports this port as a backup when other ports
are blocked by a network provider or a
firewall.

**From**: Identify the email address of the sender.

**Encryption type**: Specify the encryption type used to secure SMTP communication. Options include:

- **STARTTLS**: Select this option to start the connection as plaintext, then upgrade it to a TLS-encrypted one if the
  server supports it. If the server doesn't support it, the connection remains plaintext.
  
  > **STARTTLS** upgrades the connection to be encrypted but does not authenticate the server. Take additional steps to prevent man-in-the-middle attacks.
  >
  {.box .warning}
- **Require STARTTLS**: Select this option to require TLS. If the server doesn't support a secure connection, the
  connection will be dropped.
- **TLS (SMTPS)**: Select this option to use an encrypted connection from the start without requiring a subsequent
  connection upgrade.
- **None**: Select this option to use a plaintext connection.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA
certificate path** or by another trusted CA (such as the system's CA).

### Authentication

**Username**: The authentication principal (if required).

**Password**: The authentication credential (if required).

### Test Email Notification Targets {#test}

For arrival tests and log indicators, see [Test the Email Notification Target](common-issues#test-target).

### Default Email Notification Target for Cribl.Cloud Enterprise Orgs {#default-cloud-target}

For Cribl.Cloud Enterprise orgs, a default email Notification target is available for Cribl Stream, Edge, and Search.
You can use this target to route any email Notification to any valid email address.

This target appears in the **Notifications** > **Target** modal as `system_email`. You cannot modify or remove this
target.

The `system_email` Notification target is managed by Cribl. It will be disabled in the unlikely event of abuse. If this
target is disabled for a Workspace, a log entry will appear in the Notifications Service logs. Select **Monitoring** >
**Logs** > **Notifications Service** to view this log.


> The `system_email` email Notification target is available only for Organizations on an Enterprise plan. For details, see [Pricing](https://cribl.io/pricing/).
{.box .info}

#### Send Domain for Cribl.Cloud Email Notifications {#sending-domain}

Every Cribl.Cloud Organization and [Workspace](workspaces) has a unique address for its email Notification target. Messages sent using
this target will have a sender's address in the form `do-not-reply@<workspaceName>-<organizationId>.criblcloud.email`. To ensure
delivery, recipients should add the `criblcloud.email` domain to their email allowlists.

## Configure Email Notifications {#config-notifications}

To create an email Notification:

1. Create a [scheduled search](scheduled-searches). 

  > To schedule an existing search: Save it, then open its configuration modal from the **Saved Searches** tab.

  > On that modal's **Schedule** left tab, enable **Run on schedule**. 

2. On the **Notifications** left tab, enable **Send notifications** to unlock the options listed below.

   |Field|Description|
   |-----|-----------|
   |**When...**|Use these  controls to define the condition that will send the Notification (see [Notification Triggers](notifications#triggers)).|
   |**Send Notification to**|Select an existing email Notification target from the drop-down. Or select **Create** to define a new target (see [Email Notification Targets](#targets)).|

  ![Email Notification with target selected](search-email-notification-target-config-UI.png)
  {scale="90%"}

Once you've selected an email target, the following fields are available.

|Field|Description|
|-----|-----------|
|**To**|Email address(es) of one or more primary recipients.|
|**Cc field**|Toggle this on to expose a field where you can enter additional recipients' addresses.|
|**Bcc field**|Toggle this on to expose a field where you can enter blind-copy recipients' addresses.|
|**Include search results**|For details about this option, see [Include Search Results With Email Notifications](#results).|
|**Subject**|The subject line of the email Notification. You can use [variables](#email-vars) here.|
|**Message**|The body of the email Notification. You can use [variables](#email-vars) here.|

> Cribl Search does not limit the total number of recipients of an email Notification, but your email service might set a limit.
> 
{.box .info}

### Email Notification Variables {#email-vars}

Email variables are placeholders in the email template, which Cribl Search replaces with actual values when each email is sent. You can use these variables in the Notification's **Subject** or **Message** (body) field. Insert a variable between pairs of double braces, in this format:
`{{timestamp}}`.

| Variable | Description |
| -------- | ----------- | 
| `resultSet` | Array containing results of the search. |
| `savedQueryId` | ID of the saved search that triggered the Notification. |
| `searchId` | ID of the search job. |
| `searchResultsUrl` | URL corresponding to the search job results. |
| `notificationId` | ID of this Notification (autogenerated). |
| `timestamp` | Date when this Notification was triggered. |
| `tenantId` | ID of the Cribl Organization. |

### Include Search Results With Email Notifications {#results}

Your email Notifications can include a subset of the search results that you're alerting on. You can choose to include the results as a table within each message body, or as an attached CSV or JSON file.

To configure this:

1. Set up an [email Notification target](#targetss).
1. Open a [scheduled search](scheduled-searches), and select **Notifications**.
1. [Configure the Notifications](notifications#configure), using the email Notification target you've created.
1. As you configure, toggle on the **Include search results** slider.
1. Select one of the following:
   - **Inline table**, to include the results within the email body. If you choose this option, see the considerations in the [next section](#inline-table).
   - **CSV**, to include the results as a CSV attachment.
   - **JSON**, to include the results as a JSON attachment.

> If you select either attachment option, each attachment will automatically be assigned a unique file name, concatenating the following elements: the search ID, an epoch timestamp (representing when the search job was created), a random six-character string, and the appropriate file extension.

### Inline Table Notifications {#inline-table}

If you select the **Inline table** option, keep these details in mind:

The results table can accommodate up to 100 rows and 20 columns. If a table exceeds this size, Cribl Search will truncate it.

In the table, text will wrap. To make the results more legible, limit the number of fields sent (use the [`project`](project) operator) and the length of each field (use the [`trim`](trim) function).


 > ##### Security Risk
 > Embedding unverified or unsanitized search results into an email can pose unintended security risks.
 > Cribl recommends reviewing or sanitizing the results prior to including them.
 {.box .warning}

## Troubleshoot Email Notifications {#troubleshooting-email-notifications}

For troubleshooting, see [Common Issues and Resolutions](common-issues#troubleshooting-email-notifications).
