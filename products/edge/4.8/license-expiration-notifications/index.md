# License-Expiration Notifications


To prevent interruptions in data throughput, you can configure a Notification that will be triggered two weeks before your Cribl Edge [paid license](licensing#license-types) expires, and then again upon expiration. (If the two-week Notification is cleared from the **💬 Messages** tab between those dates, but the license has not been extended, it will trigger again.) 

## License-Expiration Notifications for Webhook Targets

 If you are sending a license-expiration Notification to a Webhook Notification target, you can include a variety of expression fields in the target's **Source expression**. For more information, see:

- [Fields Common to All Notification Types](webhook-notification-targets#common-fields)
- [License Expiration](webhook-notification-targets#license-exp)

##  Configuring License-Expiration Notifications  {#configuring-license}

1. From the top nav, select **Settings** > (**Global Settings** >) **Licensing**. 
1. Click **Add Expiration Notification** to access the **License Notifications** modal.

![License Notifications modal](license-notifications-modal.png)
{border="true"}

This modal shows existing license Notifications. 

To modify a license-expiration Notification, click anywhere on its row. 

To delete a Notification, click **Delete**. 

To view license-expiration events, click **View Events**.

Click **Add Notification** to to access the **New Notification** modal shown below.

![Configuring a license expiration Notification](license-notifications-config-modal.png)
{border="true"}

This **New Notification** modal provides [General](#general-l), [Configuration](#configuration-l), and [Metadata](#metadata-l) tabs.

###  General  {#general-l}

**ID**: Enter a unique ID for this Notification. Notifications are enabled by default, but you can disable the Notification by setting **Enabled** to `No`.

## Configuration {#configuration-l}

**When**: This modal's triggering condition is locked to `License Expiration`.

**Send notification to**: This section contains a list of the targets receiving this Notification. The **Default target** is always locked to `System Messages`. 

Click **Add Target** for each additional existing target that you want to send this Notification to. Click **Create Target** to create a new target for the Notification.

### Metadata {#metadata-l}

You can enter user-defined fields called *metadata*, which Cribl Edge includes in the notification payload. See [Metadata](notifications#metadata) for more information.