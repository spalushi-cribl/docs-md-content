# notifications.yml


`notifications.yml` contains settings for [Notifications](notifications).

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  default: # [object] 
    defaultId: # [string] ID of the default notification.
    type: # [string] Type of the default notification.
  system_notifications: # [object] 
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    severity: # [string] Notification severity. One of: error | warn | info | fatal.
    text: # [string] Text of the notification message.
    title: # [string] Title of the notification message.
    onbackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

## Notification Target Settings

Specific configuration keys differ per [Notifications target type](notifications-targets).

### Webhook Notification Target

[Webhook Notification targets](webhook-notification-targets) use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  <notification-target-id>: # [object]
    systemFields: # [array] List of fields to automatically add to events.
    method: # [string; default: POST] Method to use when sending events.
    format: # [string; default: NDJSON] Specifies how to format events before sending out.
    keepAliveTime: # [boolean] When false, connection is closed immediately after sending the outgoing request.
    concurrency: # [number] Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number; default: 0 (unlimited)] Maximum number of events to include in the request body.
    compress: # [boolean] When true, payload body is compressed before sending.
    rejectUnauthorized: # [boolean; default: false] Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (for example, the system's CA).
    timeoutSec: # [number] Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Maximum time between requests.
    useRoundRobinDns: # [boolean] When true, round-robin DNS lookup is used.
    failedRequestLoggingMode: # [string; default: none] Determines which data should be logged when a request fails. All headers are redacted by default, except those listed under `safeHeaders`.
    safeHeaders: # [array of strings] List of headers that are safe to log in plain text.
    responseRetrySettings: # [array]
      - initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
        backoffRate: # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
        maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
        httpStatus: # [number] The HTTP response status code that will trigger retries.
    timeoutRetrySettings: # [object]
      timeoutRetry: # [boolean] When true, the request is retried on timeout.
      initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
      backoffRate:  # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
      maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
    responseHonorRetryAfterHeader: # [boolean] When true, honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request.
    authType: # [string] Authentication method. One of: token | basic | none.
    advancedContentType: # [string; default: application/json] HTTP content type header value.
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    url: # [string] URL to send events to.
    username: # [string] Username for Basic authentication.
    password: # [string] Password for Basic authentication.
    token: # [string] Bearer token to include in the authorization header when Auth token authentication is enabled.
    formatEventCode: # [string] Custom JavaScript code to format incoming event data accessible through the __e variable.
    formatPayloadCode: # [string] Optional JavaScript code to format the payload sent to the Destination.
    onBackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

### PagerDuty Notification Target 

[PagerDuty Notification targets](pager-duty-notification-targets) use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  <notification-target-id>: # [object]
    systemFields: # [array] List of fields to automatically add to events.
    component: # [string; default: logstream] Optional, default component value.
    severity: # [string; default: info] Notification severity. One of: error | warn | info | critical.
    responseRetrySettings: # [array]
      - initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
        backoffRate: # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
        maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
        httpStatus: # [number] The HTTP response status code that will trigger retries.
    timeoutRetrySettings: # [object]
      timeoutRetry: # [boolean] When true, the request is retried on timeout.
      initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
      backoffRate:  # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
      maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
    responseHonorRetryAfterHeader: # [boolean] When true, honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request.
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    routingKey: # [string] 32-character integration key for an integration on a service or global ruleset.
    group: # [string] Optional, default group value.
    class: # [string] Optional, default class value.
    onBackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

### Slack Notification Target

[Slack Notification targets](slack-notification-targets) use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  <notification-target-id>: # [object]
    systemFields: # [array] List of fields to automatically add to events.
    responseRetrySettings: # [array]
      - initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
        backoffRate: # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
        maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
        httpStatus: # [number] The HTTP response status code that will trigger retries.
    timeoutRetrySettings: # [object]
      timeoutRetry: # [boolean] When true, the request is retried on timeout.
      initialBackoff: # [number; max: 600000 (10 minutes)] Delay, in milliseconds, before initiating backoff.
      backoffRate:  # [number] Base for exponential backoff. A value of 2 (default) means retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
      maxBackoff: # [number; default: 10000, min: 10000, max: 180000] Maximum backoff interval, in milliseconds.
    responseHonorRetryAfterHeader: # [boolean] When true, honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request.
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    url: # [string] Slack webhook URL to send events to.
    onBackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

### AWS SNS Notification Target

[AWS SNS Notification targets](aws-sns-notification-targets) use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  <notification-target-id>: # [object]
    systemFields: # [array] List of fields to automatically add to events.
    awsAuthenticationMethod: # [string; default: auto] AWS authentication method. Choose Auto to use IAM roles. One of: auto | manual | secret.
    signatureVersion: # [string; default: v4] Signature version to use for signing requests. One of: v2 | v4.
    reuseConnections: # [boolean; default: true] Reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean; default: true] Reject certificates that cannot be verified against a valid CA, such as self-signed certificates.
    enableAssumeRole: # [boolean; default: false] Use Assume Role credentials to access te service.
    durationSeconds: # [number; minimum: 900, maximum 43200, default: 3600] Duration of the assumed role's session, in seconds.
    allowlist: # [array] List of allowed phone numbers, can take wildcards. This is not enforced if the notification is sent to topic.
    destinationType: # [string] The type of destination to send Notifications to. One of: phoneNumber | topic.
    topicType: # [string] Type of the topic selected in AWS SNS. One of: standard | fifo.
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    topicArn: # [string] The default ARN of the SNS topic to send notifications to.
    region: # [string] Region where the SNS is located.
    messageGroupId: # [string] JavaScript expression which evaluates to a constant value indicating the group. Messages in the same group are processed in a FIFO manner.
    awsSecret: # [string] AWS secret.
    onBackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

### Email Notification Target

[Email Notification targets](email-notification-targets) use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
targets: # [object] 
  <notification-target-id>: # [object]
    systemFields: # [array] List of fields to automatically add to events.
    port: # [number] SMTP port.
    encryptionOption: # [string] Encryption type to secure SMTP communication. One of: STARTTLS | STARTTLS_REQUIRED | TLS | NONE.
    tls: # [object]
      rejectUnauthorized: # [boolean; default: true] Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's).
      minVersion: # [string] Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version to accept from connections.
    type: # [string] Notification type. One of: default | webhook | bulletin_message | router | notifications_log | pager_duty | slack | sns | smtp.
    host: # [string] SMTP server hostname/DNS name or IP address.
    from: # [string] Sender email address.
    username: # [string] Username for the sender email.
    password: # [string] Password for the sender email.
    onBackpressure: # [string] Whether to block, drop, or queue events when all receivers are exerting backpressure.
```

## Notifications

Individual Notifications use the following configuration:

```yaml {title="$CRIBL_HOME/default/cribl/notifications.yml"}
notifications: # [object] 
  <notification-id>: # [object] 
    disabled: # [boolean] Whether the Notification is enabled.
    targets: # [array] Identifiers of Notification targets to send to.
    conf: # [object] 
      name: # [string] Name of the Source or Destination that triggers the Notification.
      timeWindow: # [string] Time window for notifications to be sent out. Default: 60s.
      notifyOnResolution: # [boolean] When true, notification is sent only when condition starts and resolves. When false, notification is sent for every event. Default: true.
      dataVolume: # [string] On low of high volume Notifications: Data volume threshold below which a notification is triggered. Accepts numerals with units like KB, MB, and so on (for example: 4GB.)
      usageThreshold: # [number] On persistent queue Notifications: Persistent queue utilization above this percent value will trigger notifications. Min: 0, max: 99, Default: 90.
    condition: # [string] Identifier of the condition that trigger the Notification. One of: no-data | high-volume | low-volume | persistent-queue-usage | persistent-queue-usage-source | unhealthy-dest | license-expiration | backpressure-dest.
    targetConfigs: # [array] Configuration of each Notification target.
      - id: # [string] ID of the Notification target.
        conf: # [object] 
          emailRecipient: # [object] 
            to: # [string] Email address of the recipient.
            ccEnabled: # [boolean] Whether cc and bcc are enabled.
            cc: # [string] Email address of the cc recipient.
            bcc: # [string] email address off the bss recipient.
          subject: # [string] Subject of the email.
          body: # [string] Body of the email.
```
