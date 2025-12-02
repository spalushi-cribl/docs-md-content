# Slack Notifications


Send Slack Notifications about your [scheduled searches](scheduled-searches).

---

You can send Notifications to a Slack channel, using Slack
[Incoming Webhooks](https://api.slack.com/messaging/webhooks).


> You can also use a Webhook Destination to send specific event data to Slack for a team's attention.
> For more information, see our [Slack/Webhook integration topic](/stream/usecase-webhook-slack).
{.box .success}

First, in your Slack workspace, use a [Slack app](https://api.slack.com/messaging/webhooks) to enable incoming webhooks.

Then, in the **New Target** modal, select **Slack** to expose the following additional options on the modal's (single)
**General Settings** left tab:

## General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in the **Target ID** column of the
**Targets** tab. You can't change it later, so make sure you like it.

## Configuration {#slack-target-config}

**Webhook URL**: Add the full URL of your Slack Incoming Webhook. For example:
`https://hooks.slack.com/services/T00000000/B00000000/YOUR_DUMMY_SECRET_HERE`.

## Retries {#slack-retries}

**Honor Retry-After header**: When toggled on and the `retry-after`
[header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Search honors any
`retry-after` header that specifies a delay, up to a maximum of 20 seconds. Search always ignores `retry-after` headers
that specify a delay longer than 20 seconds.

Search will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).

When toggled off (default), Search ignores all `retry-after` headers.

**Settings for failed HTTP requests**: Automatically retries after unsuccessful response status codes, such as `429`
(Too Many Requests) or `503` (Service Unavailable). Selecting **Add Setting** reveals a table where you can add retry
parameters for individual failed HTTP requests. These include:

- **HTTP status code**: The individual status code for which the retry parameters apply.
- **Pre-backoff interval (ms)**: How long, in milliseconds, Search should wait before initiating backoff. The maximum
  interval is 600,000 ms (10 minutes).
- **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means Search will retry after 2
  seconds, then 4 seconds, then 8 seconds, and so forth.
- **Backoff limit (ms)**: The maximum backoff interval, in milliseconds, Search should apply. Default (and minimum) is
  10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

**Retry timed-out HTTP requests**: Toggle on if you want Search to automatically retries HTTP requests that have timed out.
Defaults to off. Enabling this option exposes the following settings:

- **Pre-backoff interval (ms)**: How long, in milliseconds, Search should wait before initiating backoff. Maximum
  interval is `600,000 ms` (10 minutes).
- **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means Search will retry after 2
  seconds, then 4 seconds, then 8 seconds, and so forth.
- **Backoff limit (ms)**: The maximum backoff interval, in milliseconds, Search should apply. Default (and minimum) is
  10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

After you've set and saved all of the above target configuration, you can set up [Notifications](notifications) that use the new Slack target as their destination.

## Include Search Results with Slack Notifications {#results}

You can send Slack Notifications that include a subset of search results in the Notification payload. Once you've configured your Slack target, you specify this option separately on each Notification that uses the target.

To set this up:

1. Set up a Slack Notification target, as outlined above.
1. Open a [scheduled search](scheduled-searches), and select **Notifications**.
1. [Configure the Notifications](notifications#configure), using the Slack Notification target you've
   created.
1. Within the configuration, enable **Include search results**.

Take the following into account:

- The results table can accommodate up to 100 rows and 20 columns. If it's larger, Cribl Search will truncate it.
- To make the results more legible, you can limit the number of fields sent (use the [`project`](project) operator) and the length of each field (use the [`trim`](trim) function).


> ##### Security Risk
> Embedding unverified or unsanitized search results into an AWS SNS Notification payload can pose unintended security risks.
> Cribl recommends reviewing result sets before including results with Notifications.
{.box .warning}
