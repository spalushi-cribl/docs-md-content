# PagerDuty Notifications


Send PagerDuty Notifications about your [scheduled searches](scheduled-searches).

---

You can send Notifications to [PagerDuty](https://developer.pagerduty.com/docs/get-started/getting-started/), a
real-time incident response platform, using Search's native integration with the PagerDuty API. Select Select
**PagerDuty** in the **New Target** modal to expose the following additional options on the modal's (single)
**General Settings** left tab:

## General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in the **Target ID** column of the
**Targets** tab. It can't be changed later, so make sure you like it.

## Configuration

**Routing key**: Enter your 32-character Integration key on a PagerDuty service or global ruleset.

**Group**: Optionally, specify a PagerDuty default group to assign to Cribl Search Notifications.

**Class**: Optionally, specify a PagerDuty default class to assign to Cribl Search Notifications.

**Component**: Optionally, a PagerDuty default component value to assign to Cribl Search Notification. (This field is
prefilled with `logstream`.)

**Severity**: Set the default message severity for events sent to PagerDuty. Defaults to `info`; you can instead select
`error`, `warning`, or `critical`. (Will be overridden by the `__severity` value, if set.)

## Retries {#pagerduty-retries}

**Honor Retry-After header**: Whether to honor a
[`Retry-After` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After), provided that the header
specifies a delay no longer than 180 seconds. Search limits the delay to 180 seconds even if the `Retry-After` header
specifies a longer delay. When enabled, any `Retry-After` header received takes precedence over all other options
configured in the **Retries** section. When disabled, all `Retry-After` headers are ignored.

**Settings for failed HTTP requests**: When you want to automatically retry requests that receive particular HTTP
response status codes, use these settings to list those response codes.

For any HTTP response status codes that are not explicitly configured for retries, Search applies the following rules:

| Status Code                                                     | Action             |
| --------------------------------------------------------------- | ------------------ |
| Greater than or equal to `400` and less than or equal to `500`. | Drop the request.  |
| Greater than `500`.                                             | Retry the request. |

Upon receiving a response code that's on the list, Search first waits for a set time interval called the **Pre-backoff
interval** and then begins retrying the request. Time between retries increases based on an
[exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) whose base is the **Backoff
multiplier**, until the backoff multiplier reaches the **Backoff limit (ms)**. At that point, Search continues retrying
the request without increasing the time between retries any further.

By default, this Destination has no response codes configured for automatic retries. For each response code you want to
add to the list, select **Add Setting** and configure the following settings:

- **HTTP status code**: A response code that indicates a failed request, for example `429 (Too Many Requests)` or
  `503 (Service Unavailable)`.
- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to
  `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Search
  will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Search should apply for its final retry, in milliseconds. Default
  (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

**Retry timed-out HTTP requests**: When you want to automatically retry requests that have timed out, toggle this
control on to display the following settings for configuring retry behavior:

- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to
  `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Search
  will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Search should apply for its final retry, in milliseconds. Default
  (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

After you've set and saved all of the above target configuration, you can set up [Notifications](notifications) that use the new PagerDuty target as their destination.

## Include Search Results with PagerDuty Notifications {#results}

You can send PagerDuty Notifications that include a subset of search results in the Notification payload. Once you've configured your PagerDuty target, you specify this option separately on each Notification that uses the target.

To set this up:

1. Set up a PagerDuty Notification target, as outlined above.
1. Open a [scheduled search](scheduled-searches), and select **Notifications**.
1. [Configure the Notifications](notifications#configure), using the PagerDuty Notification target you've
   created.
1. Within the configuration, enable **Include search results**.

Take the following into account:

- The results table can accommodate up to 100 rows and 20 columns. If it's larger, Cribl Search will truncate it.
- To make the results more legible, you can limit the number of fields sent (use the [`project`](project) operator) and the length of each field (use the [`trim`](trim) function).


> ##### Security Risk
> Embedding unverified or unsanitized search results into an AWS SNS Notification payload can pose unintended security risks.
> Cribl recommends reviewing result sets before including results with Notifications.
{.box .warning}
