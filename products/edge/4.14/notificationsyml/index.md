# notifications.yml


`notifications.yml` contains settings for [Notifications](notifications).

```yaml {title="notifications.yml"}
# Notification Targets - Configuration for notification targets
targets:
  # Output ID - Unique identifier for this notification target
  # [string; required]
  id:
  # Output Type - Type of notification target
  # [string; required]
  type:
  # System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
  # [array; default: cribl_host]
  systemFields:
  # Webhook configuration (webhook type only)
  # URL - Webhook endpoint URL
  # [string; required for webhook]
  url:
  # Format - Data format for webhook payload
  # [string; default: ndjson for webhook]
  format:
  # Method - HTTP method for webhook requests
  # [string; default: POST for webhook]
  method:
  # Authentication type - Authentication method for webhook requests
  # [string; default: none for webhook]
  authType:
  # Token - Authentication token (when authType is token)
  # [string]
  token:
  # Username - Username for basic authentication (when authType is basic)
  # [string]
  username:
  # Password - Password for basic authentication (when authType is basic)
  # [string]
  password:
  # Source expression - Expression to evaluate on events to generate output notifications
  # [string]
  srcExpression:
  # Slack configuration (slack type only)
  # Slack webhook URL - Slack incoming webhook URL
  # [string; required for slack]
  slackUrl:
  # PagerDuty configuration (pager_duty type only)
  # Routing key - PagerDuty integration key
  # [string; required for pager_duty]
  routingKey:
  # Severity - Alert severity level
  # [string; required for pager_duty]
  severity:
  # Group - Alert group identifier
  # [string]
  group:
  # Class - Alert class identifier  
  # [string]
  class:
  # Component - Alert component identifier
  # [string]
  component:
  # SNS configuration (sns type only)
  # Topic ARN - AWS SNS topic ARN
  # [string; required for sns]
  topicArn:
  # Region - AWS region
  # [string; required for sns]
  region:
  # AWS authentication method - Method for AWS authentication
  # [string; default: auto for sns]
  awsAuthenticationMethod:
  # Destination type - Type of SNS destination
  # [string; required for sns]
  destinationType:
  # Topic type - SNS topic type
  # [string; default: standard for sns]
  topicType:
  # Message group ID - Message group ID for FIFO topics
  # [string; required for sns]
  messageGroupId:
  # Allow list - List of allowed phone numbers or email addresses
  allowList:
  # Assume role ARN - ARN of the role to assume
  # [string]
  assumeRoleArn:
  # Enable assume role - Whether to enable role assumption
  # [boolean]
  enableAssumeRole:
  # SMTP configuration (smtp type only)
  # Host - SMTP server hostname
  # [string; required for smtp]
  host:
  # Port - SMTP server port
  # [number; required for smtp]
  port:
  # SMTP username - Username for SMTP authentication
  # [string; required for smtp]
  smtpUsername:
  # SMTP password - Password for SMTP authentication
  # [string; required for smtp]
  smtpPassword:
  # From - Sender email address
  # [string; required for smtp]
  from:
  # TLS - TLS configuration for SMTP
  tls:
    # Reject unauthorized - Whether to reject unauthorized certificates
    # [boolean; default: true]
    rejectUnauthorized:
    # Minimum TLS version - Minimum TLS version
    # [string]
    minVersion:
    # Maximum TLS version - Maximum TLS version
    # [string]
    maxVersion:
  # Router configuration (router type only)
  # Routes - Routing rules for notifications
  routes:
  # Bulletin message configuration (bulletin_message type only)
  # Message - Bulletin message content
  # [string]
  message:
  # Notifications log configuration (notifications_log type only)
  # Log notifications to internal log
  logNotifications:
# Notifications - Individual notification configurations
notifications:
  # Notification ID - Unique identifier for this notification
  # [string; required]
  id:
  # Disabled - Whether this notification is disabled
  # [boolean; default: false]
  disabled:
  # Condition - When to trigger this notification
  # [string; required]
  condition:
  # Notification targets - Targets to send any Notifications to
  targets:
  # Target configuration - Configuration for specific targets
  targetConfigs:
    # Notification target ID - Unique identifier for the notification target
    # [string; required]
    id:
    # Configuration - Target-specific configuration (SMTP type only)
    conf:
      # Subject - Email subject
      # [string]
      subject:
      # Message - Email body content. For more about Notifications and the available template variables,
      # see Notifications documentation.
      # [string]
      body:
      # Email recipient - Email recipient configuration
      # [required]
      emailRecipient:
        # To - Recipients' email addresses
        # [string; required]
        to:
        # Cc - Cc: Recipients' email addresses
        # [string]
        cc:
        # Bcc - Bcc: Recipients' email addresses
        # [string]
        bcc:
  # Condition-specific configs - Additional configuration specific to the condition
  conf:
  # Metadata - Fields to add to notifications from this condition
  metadata:
```
