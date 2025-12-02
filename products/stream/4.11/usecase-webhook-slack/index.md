# Slack/Webhook Integration


You can configure Cribl Stream to send simple Notifications to Slack via a Webhook URL. These Notifications arrive in Slack as messages in a channel that you choose. For more information, see [our Slack Notifications topic](notifications-targets#slack).

However, if you have specific event data such as alert-based or conformance data that you want to route to Slack for prompt attention, you can use Cribl Stream's [Webhook Destination](destinations-webhook) to route or filter this data. 

You can also enhance your messages with more advanced formatting. Consult the [Slack documentation](https://api.slack.com/messaging/webhooks#advanced_message_formatting) for detailed information.

> Cribl Stream does not currently support the use of special characters in Slack notifications sent via a Webhook Destination.
{.box .info}

Creating a Slack/Webhook integration is a three-step process:

1. [Create a Slack demo app](#demo).
1. [Configure the Webhook Destination](#config-webhook).
1. [Verify that Slack is receiving messages](#test).

## Create a Slack Demo App {#demo}

1. Create a Slack app by following these [Slack instructions on sending messages with incoming webhooks](https://api.slack.com/messaging/webhooks).
1. Test the cURL command supplied when setting up the Slack app. For example:

    ```shell
    curl -X POST 'https://hooks.slack.com/...' \
    -H 'Content-Type: application/json' \
    -d '{
      "text": "Hello, World!"
    }'
    ```

## Configure a Webhook Destination {#config-webhook}

Configure a Webhook Destination in Cribl Stream as described in [Webhook](destinations-webhook). However, you must override some of the default settings:

- In **General Settings**, use the **URL** provided for your Slack demo app.
- In **General Settings** > **Optional Settings**, set **Format** to `Custom`.
- In **General Settings** > **Optional Settings**, set **Source expression** to `` `{"text":"${_raw}"}` `` (with literal backticks).
- In **Advanced Settings**:
   - Toggle **Validate server certs** off.
   - Toggle **Compress** off.
   - Set **Max events per request** to `1`.

## Verify That Slack Is Receiving Messages {#test}

To verify that Slack is receiving message, send a test message, using the **Test** UI. For example:

```json
[
  {
    "_raw": "Hello, World!"
  }
]
```

If the integration is working correctly, this test message will arrive in the Slack channel you're using for your demo app.