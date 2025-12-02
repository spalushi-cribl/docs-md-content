# logger.yml


`logger.yml` maintains logging levels and redactions, per channel. In the UI, you can configure these at **Settings** > **Global** > **System > Logging**.

```yaml {title="logger.yml"}
# Logger Configuration ID
# [string]
id:
# Default redact fields - List of default field names to redact in internal logs
defaultRedactFields:
# Additional redact fields - List of additional field names to redact in internal logs
redactFields:
# Custom redact string - Literal redaction string to apply on selected field values. Overrides default.
# [string]
redactLabel:
# Log channels - Configuration for individual log channels
channels:
# TTL configuration - Time-to-live configuration for log levels
ttlConfig:
# Max log file size - Maximum log file size in bytes
# [number]
maxSizeBytes:
# Rate limit - Rate limit for log messages (0 to disable)
# [number]
limitRate:
```