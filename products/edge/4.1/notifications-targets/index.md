# Configuring Targets


To add a new Notification target from the [Manage Notifications](notifications#managing) page's **Targets** tab:

1. Click **Add Target** to open the **New Target** modal shown below.
1. Give this target a unique **Target ID**.
1. Set the **Target type** to either [PagerDuty](#pagerduty) or [Webhook](#webhook). Then configure the target according to the corresponding section below.

![Adding a new target (composite screenshot)](targets-tab-new-modal-composite.4db2aeaf84.png)

> Notifications require an Enterprise or Standard [license](licensing#license-types), without which all the target configuration options described on this page will be hidden or disabled in Cribl Edge's UI.
>
{.box .info}

## PagerDuty Targets  {#pagerduty}

This option sends Cribl Edge Notifications to [PagerDuty](https://developer.pagerduty.com/docs/get-started/getting-started/), a real-time incident response platform, using Cribl Edge's native integration with the PagerDuty API. Select **Target type**: `PagerDuty` to expose the following additional options on the modal's (single) **General Settings** left tab:

**Routing key**: Enter your 32-character Integration key on a PagerDuty service or global ruleset.

**Group**: Optionally, specify a PagerDuty default group to assign to Cribl Edge Notifications.

**Class**: Optionally, specify a PagerDuty default class to assign to Cribl Edge Notifications.

**Component**: Optionally, a PagerDuty default component value to assign to Cribl Edge Notification. (This field is prefilled with `logstream`.)

**Severity**: Set the default message severity for events sent to PagerDuty. Defaults to `info`; you can instead select `error`, `warning`, or `critical`. (Will be overridden by the `__severity` value, if set.)

## Webhook Targets  {#webhook}

With this option, you can send Cribl Edge Notifications to an arbitrary webhook. Select **Target type**: `Webhook` to expose multiple left tabs, with the following configuration options:

### General Settings

The added options that appear on this first left tab are:

**URL**: The endpoint that should receive Cribl Edge Notification events.

> To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
>
{.box .info}

**Method**: Select the appropriate HTTP verb for requests: `POST` (the default), `PUT`, or `PATCH`.

**Format**: Specifies how to format Notification events before sending them to the endpoint. Select one of the following:

- `NDJSON` (newline-delimited JSON, the default).
- `JSON Array`.
- `Custom`, which exposes these additional fields:
  - {{< id `source-expression` >}} **Source expression**: JavaScript expression whose evaluation shapes the event that Cribl Edge sends to the endpoint. E.g.: `notification=${_raw}`. For other fields you can use, see [Expression Fields](#fields). If empty, Cribl Edge will send the full Notification event as stringified JSON.
  - **Drop when null**: Toggle to `Yes` if you want to drop events where the above **Source expression** evaluates to `null`.
  - **Content type**: Defaults to `application/x‑ndjson`. You can substitute a different content type for requests sent to the endpoint. This entry will be overridden by any content types set in this modal's **Advanced Settings** tab > **Extra HTTP Headers** section.

### Authentication

Select one of the following options for authentication:
- **None**: Don't use authentication.
- **Auth token**: Use HTTP token authentication. In the resulting **Token** field, enter the bearer token that must be included in the HTTP authorization header.
- **Basic**: In the resulting **Username** and **Password** fields, enter HTTP Basic authentication credentials.

### Processing Settings

The options on this left tab are identical to those on the [Webhook Destination](destinations-webhook#processing-settings)'s **Processing Settings** tab, with two exceptions: 
* The default **System fields** entry here is `cribl_host`.
* You cannot specify a post-processing Pipeline here.

### Advanced Settings

The options on this left tab are identical to those on the [Webhook Destination](destinations-webhook#advanced-settings)'s **Advanced Settings** tab.

### Expression Fields {#fields}

When building the [Source expression](#source-expression), you can use the following fields:

#### Fields Common to All Notification Types {#common-fields}

- `starttime`: Beginning of the time bucket where this condition was reported. All Notifications have this.
- `endtime`: End of the time bucket where this condition was reported. All Notifications have this.
- `_time`: Timestamp when this Notification was created. All Notifications have this.
- `cribl_host`: Hostname of the (physical or virtual) machine on which this Notification was created. All Destination and Source Notifications have this.
- `cribl_notification`: Configured name/ID of this Notification. All Destination and Source Notifications have this.
- `origin_metadata`: Object containing metadata about the Notification's origin, with the following fields for all Destination Notifications:
  - `type`: "output".
  - `id`: ID of the affected Destination.
  - `subType`: Destination's type (where applicable).  
- `origin_metadata`: Object containing metadata about the Notification's origin, with the following fields for all Source Notifications:
  - `type`: "input".
  - `id`: ID of the affected Source.
  - `subType`: Source's type (where applicable).

#### Unhealthy Destination

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `output`: Output ID of the affected Destination.
- `_raw`: "Destination `${output}` [in group `${__worker_group}`] is unhealthy".
- `_metric`: "health.outputs".



#### Destination Backpresssure Activated

- `backpressure_type`: `1` for Block, `2` for Drop.
- `output`: Output ID of the affected Destination.
- `_raw`: "Backpressure ([dropping|blocking]) is engaged for destination `${output}` [in group `${__worker_group}`]".
- `_metric`: "backpressure.outputs".

#### Source High Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume greater than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

#### Source Low Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume less than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

#### Source No Data Received

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `_time`: Timestamp when this Notification was created.
- `input`: Input ID of the affected Source.
- `_raw`: "Source ${input} [in group ${__worker_group}] had no data for ${timeWindow}".
- `_metric`: "total.in_bytes"

#### License Expiration

- `severity`: One of "warn" or "fatal".
- `title`: One of: "License expiring soon, data will stop flowing." Or: "License has expired. Data flow has been stopped.".
- `text`: One of: "License will expire on `${expirationDate}`, no external inputs will be read
  after that time. Please contact sales@cribl.io to renew your license." Or: "License has expired."