# outputs.yml


`outputs.yml` contains configuration settings for Cribl Edge [Destinations](destinations).

```yaml {title="outputs.yml"}
outputs: # [object] 
  default_output: # [object] 
    type: # [string] Output Type
    defaultId: # [string,null] Default Output ID - ID of the default output. This will be used whenever a nonexistent/deleted output is referenced.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  webhook_output: # [object] 
    type: # [string] Output Type
    method: # [string] Method - The method to use when sending events
    format: # [string] Format - How to format events before sending out

    # -------------- if format is custom ---------------

    customSourceExpression: # [string] Source expression - Expression to evaluate on events to generate output. Example: `raw=${_raw}`. See [Cribl Docs](https://docs.cribl.io/stream/destinations-webhook#custom-format) for other examples. If empty, the full event is sent as stringified JSON.
    customDropWhenNull: # [boolean] Drop when null - Whether to drop events when the source expression evaluates to null
    customEventDelimiter: # [string] Event delimiter - Delimiter string to insert between individual events. Defaults to newline character.
    customContentType: # [string] Content type - Content type to use for request. Defaults to application/x-ndjson. Any content types set in Advanced Settings > Extra HTTP headers will override this entry.
    customPayloadExpression: # [string] Batch expression - Expression specifying how to format the payload for each batch. To reference the events to send, use the `${events}` variable. Example expression: `{ "items" : [${events}] }` would send the batch inside a JSON object.

    # --------------------------------------------------------


    # -------------- if format is advanced ---------------

    advancedContentType: # [string] Content type - HTTP content-type header value
    formatEventCode: # [string] Format inbound event - Custom JavaScript code to format incoming event data accessible through the __e variable. The formatted content is added to (__e['__eventOut']) if available. Otherwise, the original event is serialized as JSON. Caution: This function is evaluated in an unprotected context, allowing you to execute almost any JavaScript code.
    formatPayloadCode: # [string] Format outbound payload - Optional JavaScript code to format the payload sent to the Destination. The payload, containing a batch of formatted events, is accessible through the __e['payload'] variable. The formatted payload is returned in the __e['__payloadOut'] variable. Caution: This function is evaluated in an unprotected context, allowing you to execute almost any JavaScript code.

    # --------------------------------------------------------

    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 512000] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events. You can also add headers dynamically on a per-event basis in the __headers field, as explained in [Cribl Docs](https://docs.cribl.io/stream/destinations-webhook/#internal-fields).
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication type - Authentication method to use for the HTTP request

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    loginUrl: # [string] Login URL - URL for OAuth
    secretParamName: # [string] OAuth Secret parameter name - Secret parameter name to pass in request body
    secret: # [string] OAuth secret - Secret parameter value to pass in request body
    tokenAttributeName: # [string] Token attribute name - Name of the auth token attribute in the OAuth response. Can be top-level (e.g., 'token'); or nested, using a period (e.g., 'data.token').
    spacer: # [null] 
    authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header value to pass in requests. The value `${token}` is used to reference the token obtained from authentication, e.g.: `Bearer ${token}`.
    tokenTimeoutSecs: # [number; minimum: 1, maximum: 300000] Refresh interval (secs.) - How often the OAuth token should be refreshed.
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Edge will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Edge will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    loadBalanced: # [boolean] Load balancing - Enable for optimal performance. Even if you have one hostname, it can expand to multiple IPs. If disabled, consider enabling round-robin DNS.

    # -------------- if loadBalanced is false ---------------

    url: # [string] Webhook URL - URL of a webhook endpoint to send events to, such as http://localhost:10200
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    urls: # [array] Webhook URLs
      - url: # [string] Webhook LB URL - URL of a webhook endpoint to send events to, such as http://localhost:10200
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  sentinel_output: # [object] 
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 100, maximum: 1000] Body size limit (KB) - Maximum size (KB) of the request body (defaults to the API's maximum limit of 1000 KB)
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events. You can also add headers dynamically on a per-event basis in the __headers field, as explained in [Cribl Docs](https://docs.cribl.io/stream/destinations-webhook/#internal-fields).
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    loginUrl: # [string] [required] Login URL - URL for OAuth
    secret: # [string] [required] OAuth secret - Secret parameter value to pass in request body
    spacer: # [null] 
    client_id: # [string] [required] Client ID - JavaScript expression to compute the Client ID for the Azure application. Can be a constant.
    scope: # [string] Scope - Scope to pass in the OAuth request
    type: # [string] Output Type
    endpointURLConfiguration: # [string] Endpoint configuration - Enter the data collection endpoint URL or the individual ID

    # -------------- if endpointURLConfiguration is url ---------------

    url: # [string] URL - URL to send events to. Can be overwritten by an event's __url field.

    # --------------------------------------------------------


    # -------------- if endpointURLConfiguration is ID ---------------

    dcrID: # [string] Data collection rule ID - Immutable ID for the Data Collection Rule (DCR)
    dceEndpoint: # [string] Data collection endpoint - Data collection endpoint (DCE) URL. In the format: `https://<Endpoint-Name>-<Identifier>.<Region>.ingest.monitor.azure.com`
    streamName: # [string] Stream name - The name of the stream (Sentinel table) in which to store the events

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  devnull_output: # [object] 
    type: # [string] Output Type
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  syslog_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Protocol - The network protocol to use for sending out syslog messages

    # -------------- if protocol is tcp ---------------

    loadBalanced: # [boolean] Load balancing - For optimal performance, enable load balancing even if you have one hostname, as it can expand to multiple IPs.  If this setting is disabled, consider enabling round-robin DNS.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # --------------------------------------------------------


    # -------------- if protocol is udp ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number; maximum: 65535] Port - The port to connect to on the provided host
    maxRecordSize: # [number; minimum: 1, maximum: 65535] Record size limit - Maximum size of syslog messages. Make sure this value is less than or equal to the MTU to avoid UDP packet fragmentation.
    udpDnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if the destination is an IP address. A value of 0 means every message sent will incur a DNS lookup.

    # --------------------------------------------------------

    facility: # [integer] Facility - Default value for message facility. Will be overwritten by value of __facility if set. Defaults to user.
    severity: # [integer] Severity - Default value for message severity. Will be overwritten by value of __severity if set. Defaults to notice.
    appName: # [string] App name - Default name for device or application that originated the message. Defaults to Cribl, but will be overwritten by value of __appname if set.
    messageFormat: # [string] Message format - The syslog message format depending on the receiver's support
    timestampFormat: # [string] Timestamp format - Timestamp format to use when serializing event's time field
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    octetCountFraming: # [boolean] Octet count framing - Prefix messages with the byte count of the message. If disabled, no prefix will be set, and the message will be appended with a \n.
    logFailedRequests: # [boolean] Log failed requests to disk - Use to troubleshoot issues with sending data
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  splunk_output: # [object] 
    type: # [string] Output Type
    host: # [string] Address - The hostname of the receiver
    port: # [number; maximum: 65535] [required] Port - The port to connect to on the provided host
    nestedFields: # [string] Nested field serialization - How to serialize nested fields into index-time fields
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    enableMultiMetrics: # [boolean] Output multiple metrics - Output metrics in multiple-metric format in a single event. Supported in Splunk 8.0 and above.
    enableACK: # [boolean] Minimize in-flight data loss - Check if indexer is shutting down and stop sending data. This helps minimize data loss during shutdown.

    # -------------- if enableACK is false ---------------

    maxFailedHealthChecks: # [number] Failed health check limit - Maximum number of times healthcheck can fail before we close connection. If set to 0 (disabled), and the connection to Splunk is forcibly closed, some data loss might occur.

    # --------------------------------------------------------

    logFailedRequests: # [boolean] Log failed requests to disk - Use to troubleshoot issues with sending data
    maxS2Sversion: # [string] Max S2S version - The highest S2S protocol version to advertise during handshake

    # -------------- if maxS2Sversion is v4 ---------------

    compress: # [string] Compression - Controls whether the sender should send compressed data to the server. Select 'Disabled' to reject compressed connections or 'Always' to ignore server's configuration and send compressed data.

    # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
    authToken: # [string] Auth token - Shared secret token to use when establishing a connection to a Splunk indexer.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  splunk_lb_output: # [object] 
    type: # [string] Output Type
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes
    maxConcurrentSenders: # [number] Connection limit - Maximum number of concurrent connections (per Worker Process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.
    nestedFields: # [string] Nested field serialization - How to serialize nested fields into index-time fields
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    enableMultiMetrics: # [boolean] Output multiple metrics - Output metrics in multiple-metric format in a single event. Supported in Splunk 8.0 and above.
    enableACK: # [boolean] Minimize in-flight data loss - Check if indexer is shutting down and stop sending data. This helps minimize data loss during shutdown.

    # -------------- if enableACK is false ---------------

    maxFailedHealthChecks: # [number] Failed health check limit - Maximum number of times healthcheck can fail before we close connection. If set to 0 (disabled), and the connection to Splunk is forcibly closed, some data loss might occur.

    # --------------------------------------------------------

    logFailedRequests: # [boolean] Log failed requests to disk - Use to troubleshoot issues with sending data
    maxS2Sversion: # [string] Max S2S version - The highest S2S protocol version to advertise during handshake

    # -------------- if maxS2Sversion is v4 ---------------

    compress: # [string] Compression - Controls whether the sender should send compressed data to the server. Select 'Disabled' to reject compressed connections or 'Always' to ignore server's configuration and send compressed data.

    # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    indexerDiscovery: # [boolean] Indexer Discovery - Automatically discover indexers in indexer clustering environment.

    # -------------- if indexerDiscovery is true ---------------

    indexerDiscoveryConfigs: # [object]  - List of configurations to set up indexer discovery in Splunk Indexer clustering environment.
      site: # [string] [required] Site - Clustering site of the indexers from where indexers need to be discovered. In case of single site cluster, it defaults to 'default' site.
      masterUri: # [string] Cluster manager URI - Full URI of Splunk cluster manager (scheme://host:port). Example: https://managerAddress:8089
      refreshIntervalSec: # [number; minimum: 60, maximum: 86400] [required] Refresh period - Time interval, in seconds, between two consecutive indexer list fetches from cluster manager
      rejectUnauthorized: # [boolean] Validate cluster manager certificates - During indexer discovery, reject cluster manager certificates that are not authorized by the system's CA. Disable to allow untrusted (for example, self-signed) certificates.
      authTokens: # [array] Authentication tokens - Tokens required to authenticate to cluster manager for indexer discovery
        - authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
      authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
      authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthorized access is permitted.

      # -------------- if authType is manual ---------------


      # --------------------------------------------------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # -------------- if authType is secret ---------------


      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if indexerDiscovery is false ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    hosts: # [array] Destinations - Set of Splunk indexers to load-balance data to.
      - host: # [string] Address - The hostname of the receiver
        port: # [number; maximum: 65535] Port - The port to connect to on the provided host
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP); otherwise, uses the global TLS settings.
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes
    maxConcurrentSenders: # [number] Connection limit - Maximum number of concurrent connections (per Worker Process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    senderUnhealthyTimeAllowance: # [number; maximum: 60000] Endpoint health fluctuation time allowance (ms) - How long (in milliseconds) each LB endpoint can report blocked before the Destination reports unhealthy, blocking the sender. (Grace period for fluctuations.) Use 0 to disable; max 1 minute.
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
    authToken: # [string] Auth token - Shared secret token to use when establishing a connection to a Splunk indexer.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  splunk_hec_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Enable for optimal performance. Even if you have one hostname, it can expand to multiple IPs. If disabled, consider enabling round-robin DNS.

    # -------------- if loadBalanced is false ---------------

    url: # [string] Splunk HEC Endpoint - URL to a Splunk HEC endpoint to send events to, e.g., http://localhost:8088/services/collector/event
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    urls: # [array] Splunk HEC Endpoints
      - url: # [string] HEC Endpoint - URL to a Splunk HEC endpoint to send events to, e.g., http://localhost:8088/services/collector/event
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes

    # --------------------------------------------------------

    nextQueue: # [string] Next Processing Queue - In the Splunk app, define which Splunk processing queue to send the events after HEC processing.
    tcpRouting: # [string] Default _TCP_ROUTING - In the Splunk app, set the value of _TCP_ROUTING for events that do not have _ctrl._TCP_ROUTING set.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 2097152] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    enableMultiMetrics: # [boolean] Output multi-metrics - Output metrics in multiple-metric format, supported in Splunk 8.0 and above to allow multiple metrics in a single event.
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] HEC Auth token - Splunk HEC authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] HEC Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  tcpjson_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number; maximum: 65535] Port - The port to connect to on the provided host

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    hosts: # [array] Destinations - Set of hosts to load-balance data to
      - host: # [string] Address - The hostname of the receiver
        port: # [number; maximum: 65535] Port - The port to connect to on the provided host
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP); otherwise, uses the global TLS settings.
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes
    maxConcurrentSenders: # [number] Connection limit - Maximum number of concurrent connections (per Worker Process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending
    logFailedRequests: # [boolean] Log failed requests to disk - Use to troubleshoot issues with sending data
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tokenTTLMinutes: # [number; minimum: 1, maximum: 60] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60
    sendHeader: # [boolean] Send auth token in initial record - Upon connection, send a header-like record containing the auth token and other metadata.This record will not contain an actual event – only subsequent records will.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
    authToken: # [string] Auth token - Optional authentication token to include as part of the connection header

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  wavefront_output: # [object] 
    type: # [string] Output Type
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] Auth token - WaveFront API authentication token (see [here](https://docs.wavefront.com/wavefront_api.html#generating-an-api-token))

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    domain: # [string] Domain name - WaveFront domain name, e.g. "longboard"
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  signalfx_output: # [object] 
    type: # [string] Output Type
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] Auth token - SignalFx API access token (see [here](https://docs.signalfx.com/en/latest/admin-guide/tokens.html#working-with-access-tokens))

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    realm: # [string] Realm - SignalFx realm name, e.g. "us0". For a complete list of available SignalFx realm names, please check [here](https://docs.splunk.com/observability/en/get-started/service-description.html#sd-regions).
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  filesystem_output: # [object] 
    type: # [string] Output Type
    destPath: # [string] Output location - Final destination for the output files
    stagePath: # [string] Staging location - Filesystem location in which to buffer files before compressing and moving to final destination. Use performant, stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value – if present – otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 1800] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 1800] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  s3_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] S3 bucket name - Name of the destination S3 bucket. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`
    region: # [string] Region - Region where the S3 bucket is located
    awsSecretKey: # [string] Secret key - Secret key. This value can be a constant or a JavaScript expression. Example: `${C.env.SOME_SECRET}`)
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - This value can be a constant or a JavaScript expression (`${C.env.SOME_ACCESS_KEY}`)

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    destPath: # [string] Key prefix - Prefix to append to files before uploading. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `myKeyPrefix-${C.vars.myVar}`
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    serverSideEncryption: # [string] Server-side encryption for uploaded objects

    # -------------- if serverSideEncryption is aws:kms ---------------

    kmsKeyId: # [string] KMS key ID - ID or ARN of the KMS customer-managed key to use for encryption

    # --------------------------------------------------------

    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value – if present – otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 86400] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 86400] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts upload limit - Maximum number of parts to upload in parallel per file. Minimum part size is 5MB.
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    maxClosingFilesToBackpressure: # [number; minimum: 10, maximum: 4200] Staging file limit - Maximum number of files that can be waiting for upload before backpressure is applied
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  azure_blob_output: # [object] 
    type: # [string] Output Type
    containerName: # [string] Container name - The Azure Blob Storage container name. Name can include only lowercase letters, numbers, and hyphens. For dynamic container names, enter a JavaScript expression within quotes or backtickss, to be evaluated at initialization. The expression can evaluate to a constant value and can reference Global Variables, such as `myContainer-${C.env["CRIBL_WORKER_ID"]}`.
    createContainer: # [boolean] Create container - Create the configured container in Azure Blob Storage if it does not already exist
    destPath: # [string] Blob prefix - Root directory prepended to path before uploading. Value can be a JavaScript expression enclosed in quotes or backticks, to be evaluated at initialization. The expression can evaluate to a constant value and can reference Global Variables, such as `myBlobPrefix-${C.env["CRIBL_WORKER_ID"]}`.
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files before compressing and moving to final destination. Use performant and stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts limit - Maximum number of parts to upload in parallel per file
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value – if present – otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 1800] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 1800] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    authType: # [string] Authentication method

    # -------------- if authType is manual ---------------

    connectionString: # [string] Connection string - Enter your Azure Storage account connection string. If left blank, Stream will fall back to env.AZURE_STORAGE_CONNECTION_STRING.

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Connection string (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is clientSecret ---------------

    storageAccountName: # [string] Storage account name - The name of your Azure storage account
    tenantId: # [string] Tenant ID - The service principal's tenant ID
    clientId: # [string] Client ID - The service principal's client ID
    azureCloud: # [string] Azure Cloud - The Azure cloud to use. Defaults to Azure Public Cloud.
    endpointSuffix: # [string] Endpoint suffix - Endpoint suffix for the service URL. Takes precedence over the Azure Cloud setting. Defaults to core.windows.net.
    clientTextSecret: # [string] Client secret (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is clientCert ---------------

    storageAccountName: # [string] Storage account name - The name of your Azure storage account
    tenantId: # [string] Tenant ID - The service principal's tenant ID
    clientId: # [string] Client ID - The service principal's client ID
    azureCloud: # [string] Azure Cloud - The Azure cloud to use. Defaults to Azure Public Cloud.
    endpointSuffix: # [string] Endpoint suffix - Endpoint suffix for the service URL. Takes precedence over the Azure Cloud setting. Defaults to core.windows.net.
    certificate: # [object] 
      certificateName: # [string] Certificate - The certificate you registered as credentials for your app in the Azure portal

    # --------------------------------------------------------

    storageClass: # [string] Blob access tier
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  azure_data_explorer_output: # [object] 
    type: # [string] Output Type
    clusterUrl: # [string] Cluster base URI - The base URI for your cluster. Typically, `https://<cluster>.<region>.kusto.windows.net`.
    database: # [string] [required] Database name - Name of the database containing the table where data will be ingested
    table: # [string] [required] Table name - Name of the table to ingest data into
    validateDatabaseSettings: # [boolean] Validate database settings - When saving or starting the Destination, validate the database name and credentials; also validate table name, except when creating a new table. Disable if your Azure app does not have both the Database Viewer and the Table Viewer role.
    ingestMode: # [string] Ingestion mode

    # -------------- if ingestMode is batching ---------------

    ingestUrl: # [string] Ingestion service URI - The ingestion service URI for your cluster. Typically, `https://ingest-<cluster>.<region>.kusto.windows.net`.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    isMappingObj: # [boolean] Add mapping object - Send a JSON mapping object instead of specifying an existing named data mapping
    format: # [string] Data format - Format of the output data
    stagePath: # [string] Staging location - Filesystem location in which to buffer files before compressing and moving to final destination. Use performant and stable storage.
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 1800] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 1800] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts limit - Maximum number of parts to upload in parallel per file
    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushImmediately: # [boolean] Flush immediately - Bypass the data management service's aggregation mechanism
    retainBlobOnSuccess: # [boolean] Retain blob on success - Prevent blob deletion after ingestion is complete
    extentTags: # [array] Extent tags - Strings or tags associated with the extent (ingested data shard)
      - prefix: # [string] Prefix (optional)
        value: # [string] Value
    ingestIfNotExists: # [array] Enforce uniqueness via tag values - Prevents duplicate ingestion by verifying whether an extent with the specified ingest-by tag already exists
      - value: # [string] Value
    reportLevel: # [string] Report level - Level of ingestion status reporting. Defaults to FailuresOnly.
    reportMethod: # [string] Report method - Target of the ingestion status reporting. Defaults to Queue.
    additionalProperties: # [array] Additional fields - Optionally, enter additional configuration properties to send to the ingestion service
      - key: # [string] Key
        value: # [string] Value

    # --------------------------------------------------------


    # -------------- if ingestMode is streaming ---------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    format: # [string] Data format - Format of the output data
    compress: # [string] [required] Compression - Data compression format to apply to HTTP content before it is delivered
    mappingRef: # [string] Data mapping - Enter the name of a data mapping associated with your target table. Or, if incoming event and target table fields match exactly, you can leave the field empty.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 4096] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request

    # --------------------------------------------------------

    oauthEndpoint: # [string] [required] Microsoft Entra ID authentication endpoint - Endpoint used to acquire authentication tokens from Azure
    tenantId: # [string] [required] Tenant ID - Directory ID (tenant identifier) in Azure Active Directory
    clientId: # [string] [required] Client ID - client_id to pass in the OAuth request parameter
    scope: # [string] [required] Scope - Scope to pass in the OAuth request parameter
    oauthType: # [string] [required] Authentication method - The type of OAuth 2.0 client credentials grant flow to use

    # -------------- if oauthType is clientSecret ---------------

    clientSecret: # [string] Client secret - The client secret that you generated for your app in the Azure portal

    # --------------------------------------------------------


    # -------------- if oauthType is clientTextSecret ---------------

    textSecret: # [string] Client secret (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if oauthType is certificate ---------------

    certificate: # [object] 
      certificateName: # [string] Certificate - The certificate you registered as credentials for your app in the Azure portal

    # --------------------------------------------------------

    advancedSettingsNote: # [null] 
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  azure_logs_output: # [object] 
    type: # [string] Output Type
    logType: # [string] Log Type - The Log Type of events sent to this LogAnalytics workspace. Defaults to `Cribl`. Use only letters, numbers, and `_` characters, and can't exceed 100 characters. Can be overwritten by event field __logType.
    resourceId: # [string] Resource ID - Optional Resource ID of the Azure resource to associate the data with. Can be overridden by the __resourceId event field. This ID populates the _ResourceId property, allowing the data to be included in resource-centric queries. If the ID is neither specified nor overridden, resource-centric queries will omit the data.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] 
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    apiUrl: # [string] DNS name of API endpoint - The DNS name of the Log API endpoint that sends log data to a Log Analytics workspace in Azure Monitor. Defaults to .ods.opinsights.azure.com. Cribl Edge will add a prefix and suffix to construct a URI in this format: <https://<Workspace_ID><your_DNS_name>/api/logs?api-version=<API version>.
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter workspace ID and workspace key directly, or select a stored secret
    workspaceId: # [string] Workspace ID - Azure Log Analytics Workspace ID. See Azure Dashboard Workspace > Advanced settings.
    workspaceKey: # [string] Workspace key - Azure Log Analytics Workspace Primary or Secondary Shared Key. See Azure Dashboard Workspace > Advanced settings.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    keypairSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  kinesis_output: # [object] 
    type: # [string] Output Type
    streamName: # [string] Stream Name - Kinesis stream name to send events to.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] [required] Region - Region where the Kinesis stream is located
    endpoint: # [string] Endpoint - Kinesis stream service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to Kinesis stream-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing Kinesis stream requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for Kinesis stream - Use Assume Role credentials to access Kinesis stream
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    concurrency: # [number; minimum: 1, maximum: 32] Put request concurrency - Maximum number of ongoing put requests before blocking.
    maxRecordSizeKB: # [number; minimum: 1, maximum: 10240] Record size limit (KB, uncompressed) - Maximum size (KB) of each individual record before compression. For uncompressed or non-compressible data 1MB is the max recommended size
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    compression: # [string] Compression - Compression type to use for records

    # -------------- if compression is none ---------------

    asNdjson: # [boolean] Send batched - Batch events into a single record as NDJSON

    # --------------------------------------------------------

    useListShards: # [boolean] ListShards API - Provides higher stream rate limits, improving delivery speed and reliability by minimizing throttling. See the [ListShards API](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListShards.html) documentation for details.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  honeycomb_output: # [object] 
    type: # [string] Output Type
    dataset: # [string] Dataset name - Name of the dataset to send events to – e.g., observability
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    team: # [string] API key - Team API key where the dataset belongs

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  azure_eventhub_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Brokers - List of Event Hubs Kafka brokers to connect to, eg. yourdomain.servicebus.windows.net:9093. The hostname can be found in the host portion of the primary or secondary connection string in Shared Access Policies.
    topic: # [string] [required] Event Hub name - The name of the Event Hub (Kafka Topic) to publish events. Can be overwritten using field __topicOut.
    ack: # [integer] Acknowledgments - Control the number of required acknowledgments
    format: # [string] Record data format - Format to use to serialize events before writing to the Event Hubs Kafka brokers
    maxRecordSizeKB: # [number; minimum: 1] Record size limit (KB, uncompressed) - Maximum size of each record batch before compression. Setting should be < message.max.bytes settings in Event Hubs brokers.
    flushEventCount: # [number; minimum: 1, maximum: 10000] Events-per-batch limit - Maximum number of events in a batch before forcing a flush
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Edge can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism

      # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another trusted CA (such as the system's)

      # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  google_chronicle_output: # [object] 
    type: # [string] Output Type
    apiVersion: # [string] API version

    # -------------- if apiVersion is v1 ---------------

    authenticationMethod: # [string] Authentication method

    # --------------------------------------------------------


    # -------------- if apiVersion is v2 ---------------

    authenticationMethod: # [string] Authentication method

    # --------------------------------------------------------

    authenticationMethod: # [string] Authentication method

    # -------------- if authenticationMethod is manual ---------------

    apiKey: # [string] API key - Organization's API key in Google SecOps

    # --------------------------------------------------------


    # -------------- if authenticationMethod is secret ---------------

    apiKeySecret: # [string] API key (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authenticationMethod is serviceAccount ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right.

    # --------------------------------------------------------


    # -------------- if authenticationMethod is serviceAccountSecret ---------------

    serviceAccountCredentialsSecret: # [string] Service account credentials (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    logFormatType: # [string] Send events as

    # -------------- if logFormatType is unstructured ---------------

    extraLogTypes: # [array] Custom log types - Custom log types. If the value "Custom" is selected in the setting "Default log type" above, the first custom log type in this table will be automatically selected as default log type.
      - logType: # [string] Log Type
        description: # [string] Description
    logType: # [string] Default log type - Default log type value to send to SecOps. Can be overwritten by event field __logType.
    logTextField: # [string] Log text field - Name of the event field that contains the log text to send. If not specified, Stream sends a JSON representation of the whole event.
    customerId: # [string] Customer ID - Unique identifier (UUID) corresponding to a particular SecOps instance. Provided by your SecOps representative.
    namespace: # [string] Namespace - User-configured environment namespace to identify the data domain the logs originated from. Use namespace as a tag to identify the appropriate data domain for indexing and enrichment functionality. Can be overwritten by event field __namespace.
    customLabels: # [array] Custom labels - Custom labels to be added to every batch 
      - key: # [string] Key
        value: # [string] Value

    # --------------------------------------------------------

    region: # [string] Region - Regional endpoint to send events to
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1, maximum: 1024] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  google_cloud_storage_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] Bucket name - Name of the destination bucket. This value can be a constant or a JavaScript expression that can only be evaluated at init time. Example of referencing a Global Variable: `myBucket-${C.vars.myVar}`.
    region: # [string] [required] Region - Region where the bucket is located
    endpoint: # [string] [required] Endpoint - Google Cloud Storage service endpoint
    signatureVersion: # [string] Signature version - Signature version to use for signing Google Cloud Storage requests
    awsAuthenticationMethod: # [string] Authentication method

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - HMAC access key. This value can be a constant or a JavaScript expression, such as `${C.env.GCS_ACCESS_KEY}`.
    awsSecretKey: # [string] Secret - HMAC secret. This value can be a constant or a JavaScript expression, such as `${C.env.GCS_SECRET}`.

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    destPath: # [string] Key prefix - Prefix to append to files before uploading. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `myKeyPrefix-${C.vars.myVar}`
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value – if present – otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 1800] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 1800] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  google_cloud_logging_output: # [object] 
    type: # [string] Output Type
    logLocationType: # [string] Log location type

    # -------------- if logLocationType is project ---------------

    logLocationExpression: # [string] [required] Project ID expression - JavaScript expression to compute the value of the project ID with which log entries should be associated.

    # --------------------------------------------------------


    # -------------- if logLocationType is organization ---------------

    logLocationExpression: # [string] [required] Organization ID expression - JavaScript expression to compute the value of the organization ID with which log entries should be associated.

    # --------------------------------------------------------


    # -------------- if logLocationType is billingAccount ---------------

    logLocationExpression: # [string] [required] Billing account ID expression - JavaScript expression to compute the value of the billing account ID with which log entries should be associated.

    # --------------------------------------------------------


    # -------------- if logLocationType is folder ---------------

    logLocationExpression: # [string] [required] Folder ID expression - JavaScript expression to compute the value of the folder ID with which log entries should be associated.

    # --------------------------------------------------------

    logNameExpression: # [string] [required] Log name expression - JavaScript expression to compute the value of the log name.
    payloadFormat: # [string] Payload format - Format to use when sending payload. Defaults to Text.

    # -------------- if payloadFormat is text ---------------

    payloadExpression: # [string] Payload text expression - JavaScript expression to compute the value of the payload. Must evaluate to a JavaScript string value. If an invalid value is encountered it will result in the default value instead. Defaults to a JSON string representation of the event.

    # --------------------------------------------------------


    # -------------- if payloadFormat is json ---------------

    payloadExpression: # [string] Payload object expression - JavaScript expression to compute the value of the payload. Must evaluate to a JavaScript object value. If an invalid value is encountered it will result in the default value instead. Defaults to the entire event.

    # --------------------------------------------------------

    logLabels: # [array] Log labels - Labels to apply to the log entry
      - label: # [string] Label - Label name
        valueExpression: # [string] Value - JavaScript expression to compute the label's value.
    resourceTypeExpression: # [string] Resource type expression - JavaScript expression to compute the value of the managed resource type field. Must evaluate to one of the valid values [here](https://cloud.google.com/logging/docs/api/v2/resource-list#resource-types). Defaults to "global".
    resourceTypeLabels: # [array] Resource labels - Labels to apply to the managed resource. These must correspond to the valid labels for the specified resource type (see [here](https://cloud.google.com/logging/docs/api/v2/resource-list#resource-types)). Otherwise, they will be dropped by Google Cloud Logging.
      - label: # [string] Label - Label name
        valueExpression: # [string] Value - JavaScript expression to compute the label's value.
    severityExpression: # [string] Severity expression - JavaScript expression to compute the value of the severity field. Must evaluate to one of the severity values supported by Google Cloud Logging [here](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logseverity) (case insensitive). Defaults to "DEFAULT".
    insertIdExpression: # [string] Insert ID expression - JavaScript expression to compute the value of the insert ID field.
    googleAuthMethod: # [string] Google authentication method - Choose Auto to use Google Application Default Credentials (ADC), Manual to enter Google service account credentials directly, or Secret to select or create a stored secret that references Google service account credentials.

    # -------------- if googleAuthMethod is manual ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right.

    # --------------------------------------------------------


    # -------------- if googleAuthMethod is secret ---------------

    secret: # [string] Service account credentials (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Events-per-request limit - Max number of events to include in the request body. Default is 0 (unlimited).
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it.
    throttleRateReqPerSec: # [integer; maximum: 2000] Throttle request rate - Maximum number of requests to limit to per second.
    requestMethodExpression: # [string] Request method expression - A JavaScript expression that evaluates to the HTTP request method as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    requestUrlExpression: # [string] Request URL expression - A JavaScript expression that evaluates to the HTTP request URL as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    requestSizeExpression: # [string] Request size expression - A JavaScript expression that evaluates to the HTTP request size as a string, in int64 format. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    statusExpression: # [string] Request status expression - A JavaScript expression that evaluates to the HTTP request method as a number. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    responseSizeExpression: # [string] Response size expression - A JavaScript expression that evaluates to the HTTP response size as a string, in int64 format. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    userAgentExpression: # [string] Request user agent expression - A JavaScript expression that evaluates to the HTTP request user agent as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    remoteIpExpression: # [string] Remote IP expression - A JavaScript expression that evaluates to the HTTP request remote IP as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    serverIpExpression: # [string] Server IP expression - A JavaScript expression that evaluates to the HTTP request server IP as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    refererExpression: # [string] Referer expression - A JavaScript expression that evaluates to the HTTP request referer as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    latencyExpression: # [string] Latency expression - A JavaScript expression that evaluates to the HTTP request latency, formatted as <seconds>.<nanoseconds>s (for example, 1.23s). See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    cacheLookupExpression: # [string] Cache lookup expression - A JavaScript expression that evaluates to the HTTP request cache lookup as a boolean. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    cacheHitExpression: # [string] Cache hit expression - A JavaScript expression that evaluates to the HTTP request cache hit as a boolean. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    cacheValidatedExpression: # [string] Cache validated with origin server expression - A JavaScript expression that evaluates to the HTTP request cache validated with origin server as a boolean. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    cacheFillBytesExpression: # [string] Cache fill bytes expression - A JavaScript expression that evaluates to the HTTP request cache fill bytes as a string, in int64 format. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    protocolExpression: # [string] Protocol expression - A JavaScript expression that evaluates to the HTTP request protocol as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#httprequest) for details.
    idExpression: # [string] ID expression - A JavaScript expression that evaluates to the log entry operation ID as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentryoperation) for details.
    producerExpression: # [string] Producer expression - A JavaScript expression that evaluates to the log entry operation producer as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentryoperation) for details.
    firstExpression: # [string] First expression - A JavaScript expression that evaluates to the log entry operation first flag as a boolean. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentryoperation) for details.
    lastExpression: # [string] Last expression - A JavaScript expression that evaluates to the log entry operation last flag as a boolean. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentryoperation) for details.
    fileExpression: # [string] File expression - A JavaScript expression that evaluates to the log entry source location file as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentrysourcelocation) for details.
    lineExpression: # [string] Line expression - A JavaScript expression that evaluates to the log entry source location line as a string, in int64 format. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentrysourcelocation) for details.
    functionExpression: # [string] Function expression - A JavaScript expression that evaluates to the log entry source location function as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logentrysourcelocation) for details.
    uidExpression: # [string] UID expression - A JavaScript expression that evaluates to the log entry log split UID as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logsplit) for details.
    indexExpression: # [string] Index expression - A JavaScript expression that evaluates to the log entry log split index as a number. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logsplit) for details.
    totalSplitsExpression: # [string] Total splits expression - A JavaScript expression that evaluates to the log entry log split total splits as a number. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logsplit) for details.
    traceExpression: # [string] Trace expression - A JavaScript expression that evaluates to the REST resource name of the trace being written as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry) for details.
    spanIdExpression: # [string] Span ID expression - A JavaScript expression that evaluates to the ID of the cloud trace span associated with the current operation in which the log is being written as a string. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry) for details.
    traceSampledExpression: # [string] Trace sampled expression - A JavaScript expression that evaluates to the the sampling decision of the span associated with the log entry. See the [documentation](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry) for details.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  google_pubsub_output: # [object] 
    type: # [string] Output Type
    topicName: # [string] Topic ID - ID of the topic to send events to.
    createTopic: # [boolean] Create topic - If enabled, create topic if it does not exist.
    orderedDelivery: # [boolean] Ordered delivery - If enabled, send events in the order they were added to the queue. For this to work correctly, the process receiving events must have ordering enabled.
    region: # [string] Region - Region to publish messages to. Select 'default' to allow Google to auto-select the nearest region. When using ordered delivery, the selected region must be allowed by message storage policy.
    googleAuthMethod: # [string] Google authentication method - Choose Auto to use Google Application Default Credentials (ADC), Manual to enter Google service account credentials directly, or Secret to select or create a stored secret that references Google service account credentials.

    # -------------- if googleAuthMethod is manual ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right.

    # --------------------------------------------------------


    # -------------- if googleAuthMethod is secret ---------------

    secret: # [string] Service account credentials (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    batchSize: # [number; minimum: 1, maximum: 10000] Batch size - The maximum number of items the Google API should batch before it sends them to the topic.
    batchTimeout: # [number; minimum: 1, maximum: 100000] Batch timeout (ms) - The maximum amount of time, in milliseconds, that the Google API should wait to send a batch (if the Batch size is not reached).
    maxQueueSize: # [number; minimum: 1] Queue size limit - Maximum number of queued batches before blocking.
    maxRecordSizeKB: # [number; minimum: 1, maximum: 256] Batch size limit (KB) - Maximum size (KB) of batches to send.
    maxInProgress: # [number; minimum: 1, maximum: 100] Concurrent request limit - The maximum number of in-progress API requests before backpressure is applied.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  exabeam_output: # [object] 
    bucket: # [string] Bucket name - Name of the destination bucket. A constant or a JavaScript expression that can only be evaluated at init time. Example of referencing a JavaScript Global Variable: `myBucket-${C.vars.myVar}`.
    region: # [string] [required] Region - Region where the bucket is located
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    endpoint: # [string] [required] Endpoint - Google Cloud Storage service endpoint
    signatureVersion: # [string] Signature version - Signature version to use for signing Google Cloud Storage requests
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 1800] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 1800] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    type: # [string] Output Type
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    encodedConfiguration: # [string] Exabeam connection string - Enter an encoded string containing Exabeam configurations
    collectorInstanceId: # [string] [required] Collector instance ID - ID of the Exabeam Collector where data should be sent. Example: 11112222-3333-4444-5555-666677778888

    siteName: # [string] Site name - Constant or JavaScript expression to create an Exabeam site name. Values that aren't successfully evaluated will be treated as string constants.
    siteId: # [string] Site ID - Exabeam site ID. If left blank, Cribl Edge will use the value of the Exabeam site name.
    timezoneOffset: # [string] Timezone offset
    awsApiKey: # [string] Access key - HMAC access key. Can be a constant or a JavaScript expression, such as `${C.env.GCS_ACCESS_KEY}`.
    awsSecretKey: # [string] Secret - HMAC secret. Can be a constant or a JavaScript expression, such as `${C.env.GCS_SECRET}`.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  kafka_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Bootstrap servers - Enter each Kafka bootstrap server you want to use. Specify hostname and port, e.g., mykafkabroker:9092, or just hostname, in which case Cribl Edge will assign port 9092.
    topic: # [string] [required] Topic - The topic to publish events to. Can be overridden using the __topicOut field.
    ack: # [integer] Acknowledgments - Control the number of required acknowledgments.
    format: # [string] Record data format - Format to use to serialize events before writing to Kafka.

    # -------------- if format is protobuf ---------------

    protobufLibraryId: # [string] Definition set - Select a set of Protobuf definitions for the events you want to send

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending to Kafka
    maxRecordSizeKB: # [number; minimum: 1] Record size limit (KB, uncompressed) - Maximum size of each record batch before compression. The value must not exceed the Kafka brokers' message.max.bytes setting.
    flushEventCount: # [number; minimum: 1, maximum: 10000] Events-per-batch limit - The maximum number of events you want the Destination to allow in a batch before forcing a flush
    flushPeriodSec: # [number] Flush period (sec) - The maximum amount of time you want the Destination to wait before forcing a flush. Shorter intervals tend to result in smaller batches being sent.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for accessing the Confluent Schema Registry. Example: http://localhost:8081. To connect over TLS, use https instead of http.
      connectionTimeout: # [number; minimum: 1000, maximum: 60000] Connection timeout (ms) - Maximum time to wait for a Schema Registry connection to complete successfully
      requestTimeout: # [number; minimum: 1000, maximum: 60000] Request timeout (ms) - Maximum time to wait for the Schema Registry to respond to a request
      maxRetries: # [number; maximum: 100] Retry limit - Maximum number of times to try fetching schemas from the Schema Registry
      auth: # [object]  - Credentials to use when authenticating with the schema registry using basic HTTP authentication
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

        # --------------------------------------------------------

      tls: # [object] TLS settings (client side)
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
        servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
        certificateName: # [string] Certificate - The name of the predefined certificate
        caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
        privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
        certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
        passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
        minVersion: # [string] Minimum TLS version
        maxVersion: # [string] Maximum TLS version

        # --------------------------------------------------------

      defaultKeySchemaId: # [number] Default key schema ID - Used when __keySchemaIdOut is not present, to transform key values, leave blank if key transformation is not required by default.
      defaultValueSchemaId: # [number] Default value schema ID - Used when __valueSchemaIdOut is not present, to transform _raw, leave blank if value transformation is not required by default.

      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Edge can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism

      # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  confluent_cloud_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Bootstrap servers - List of Confluent Cloud bootstrap servers to use, such as yourAccount.confluent.cloud:9092.
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    topic: # [string] [required] Topic - The topic to publish events to. Can be overridden using the __topicOut field.
    ack: # [integer] Acknowledgments - Control the number of required acknowledgments.
    format: # [string] Record data format - Format to use to serialize events before writing to Kafka.

    # -------------- if format is protobuf ---------------

    protobufLibraryId: # [string] Definition set - Select a set of Protobuf definitions for the events you want to send

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending to Kafka
    maxRecordSizeKB: # [number; minimum: 1] Record size limit (KB, uncompressed) - Maximum size of each record batch before compression. The value must not exceed the Kafka brokers' message.max.bytes setting.
    flushEventCount: # [number; minimum: 1, maximum: 10000] Events-per-batch limit - The maximum number of events you want the Destination to allow in a batch before forcing a flush
    flushPeriodSec: # [number] Flush period (sec) - The maximum amount of time you want the Destination to wait before forcing a flush. Shorter intervals tend to result in smaller batches being sent.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for accessing the Confluent Schema Registry. Example: http://localhost:8081. To connect over TLS, use https instead of http.
      connectionTimeout: # [number; minimum: 1000, maximum: 60000] Connection timeout (ms) - Maximum time to wait for a Schema Registry connection to complete successfully
      requestTimeout: # [number; minimum: 1000, maximum: 60000] Request timeout (ms) - Maximum time to wait for the Schema Registry to respond to a request
      maxRetries: # [number; maximum: 100] Retry limit - Maximum number of times to try fetching schemas from the Schema Registry
      auth: # [object]  - Credentials to use when authenticating with the schema registry using basic HTTP authentication
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

        # --------------------------------------------------------

      tls: # [object] TLS settings (client side)
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
        servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
        certificateName: # [string] Certificate - The name of the predefined certificate
        caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
        privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
        certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
        passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
        minVersion: # [string] Minimum TLS version
        maxVersion: # [string] Maximum TLS version

        # --------------------------------------------------------

      defaultKeySchemaId: # [number] Default key schema ID - Used when __keySchemaIdOut is not present, to transform key values, leave blank if key transformation is not required by default.
      defaultValueSchemaId: # [number] Default value schema ID - Used when __valueSchemaIdOut is not present, to transform _raw, leave blank if value transformation is not required by default.

      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Edge can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism

      # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  msk_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Bootstrap servers - Enter each Kafka bootstrap server you want to use. Specify hostname and port, e.g., mykafkabroker:9092, or just hostname, in which case Cribl Edge will assign port 9092.
    topic: # [string] [required] Topic - The topic to publish events to. Can be overridden using the __topicOut field.
    ack: # [integer] Acknowledgments - Control the number of required acknowledgments.
    format: # [string] Record data format - Format to use to serialize events before writing to Kafka.

    # -------------- if format is protobuf ---------------

    protobufLibraryId: # [string] Definition set - Select a set of Protobuf definitions for the events you want to send

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending to Kafka
    maxRecordSizeKB: # [number; minimum: 1] Record size limit (KB, uncompressed) - Maximum size of each record batch before compression. The value must not exceed the Kafka brokers' message.max.bytes setting.
    flushEventCount: # [number; minimum: 1, maximum: 10000] Events-per-batch limit - The maximum number of events you want the Destination to allow in a batch before forcing a flush
    flushPeriodSec: # [number] Flush period (sec) - The maximum amount of time you want the Destination to wait before forcing a flush. Shorter intervals tend to result in smaller batches being sent.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for accessing the Confluent Schema Registry. Example: http://localhost:8081. To connect over TLS, use https instead of http.
      connectionTimeout: # [number; minimum: 1000, maximum: 60000] Connection timeout (ms) - Maximum time to wait for a Schema Registry connection to complete successfully
      requestTimeout: # [number; minimum: 1000, maximum: 60000] Request timeout (ms) - Maximum time to wait for the Schema Registry to respond to a request
      maxRetries: # [number; maximum: 100] Retry limit - Maximum number of times to try fetching schemas from the Schema Registry
      auth: # [object]  - Credentials to use when authenticating with the schema registry using basic HTTP authentication
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

        # --------------------------------------------------------

      tls: # [object] TLS settings (client side)
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
        servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
        certificateName: # [string] Certificate - The name of the predefined certificate
        caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
        privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
        certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
        passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
        minVersion: # [string] Minimum TLS version
        maxVersion: # [string] Maximum TLS version

        # --------------------------------------------------------

      defaultKeySchemaId: # [number] Default key schema ID - Used when __keySchemaIdOut is not present, to transform key values, leave blank if key transformation is not required by default.
      defaultValueSchemaId: # [number] Default value schema ID - Used when __valueSchemaIdOut is not present, to transform _raw, leave blank if value transformation is not required by default.

      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Edge can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
    awsAuthenticationMethod: # [string] [required] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] [required] Region - Region where the MSK cluster is located
    endpoint: # [string] Endpoint - MSK cluster service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to MSK cluster-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing MSK cluster requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for MSK - Use Assume Role credentials to access MSK
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  elastic_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Enable for optimal performance. Even if you have one hostname, it can expand to multiple IPs. If disabled, consider enabling round-robin DNS.

    # -------------- if loadBalanced is false ---------------

    url: # [string] Bulk API URL or Cloud ID - The Cloud ID or URL to an Elastic cluster to send events to. Example: http://elastic:9200/_bulk
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    urls: # [array] Bulk API URLs
      - url: # [string] URL - The URL to an Elastic node to send events to. Example: http://elastic:9200/_bulk
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes

    # --------------------------------------------------------

    index: # [string] Index or data stream - Index or data stream to send events to. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be overwritten by an event's __index field.
    docType: # [string] Type - Document type to use for events. Can be overwritten by an event's __type field.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 102400] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    extraParams: # [array] Extra parameters
      - name: # [string] Field Name
        value: # [string] Field Value
    auth: # [object] 
      disabled: # [boolean] Authentication Disabled

      # -------------- if disabled is false ---------------

      authType: # [string] Authentication method - Enter credentials directly, or select a stored secret

      # --------------------------------------------------------

    elasticVersion: # [string] Elastic version - Optional Elasticsearch version, used to format events. If not specified, will auto-discover version.
    elasticPipeline: # [string] Elastic pipeline - Optional Elasticsearch destination pipeline
    includeDocId: # [boolean] Include document _id - Include the `document_id` field when sending events to an Elastic TSDS (time series data stream)
    writeAction: # [string] Write action - Action to use when writing events. Must be set to `Create` when writing to a data stream.
    retryPartialErrors: # [boolean] Retry partial errors - Retry failed events when a bulk request to Elastic is successful, but the response body returns an error for one or more events in the batch
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  elastic_cloud_output: # [object] 
    type: # [string] Output Type
    url: # [string] Cloud ID - Enter Cloud ID of the Elastic Cloud environment to send events to
    index: # [string] [required] Data stream or index - Data stream or index to send events to. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be overwritten by an event's __index field.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 102400] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    extraParams: # [array] Extra parameters - Extra parameters to use in HTTP requests
      - name: # [string] Field Name
        value: # [string] Field Value
    auth: # [object] 
      disabled: # [boolean] Authentication Disabled

      # -------------- if disabled is false ---------------

      authType: # [string] Authentication method - Enter credentials directly, or select a stored secret

      # --------------------------------------------------------

    elasticPipeline: # [string] Elastic pipeline - Optional Elastic Cloud Destination pipeline
    includeDocId: # [boolean] Include document _id - Include the `document_id` field when sending events to an Elastic TSDS (time series data stream)
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  newrelic_output: # [object] 
    type: # [string] Output Type
    region: # [string] Region - Which New Relic region endpoint to use.

    # -------------- if region is Custom ---------------

    customUrl: # [string] 

    # --------------------------------------------------------

    logType: # [string] Log type - Name of the logtype to send with events, e.g.: observability, access_log. The event's 'sourcetype' field (if set) will override this value.
    messageField: # [string] Log message field - Name of field to send as log message value. If not present, event will be serialized and sent as JSON.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1, maximum: 1024] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - New Relic API key. Can be overridden using __newRelic_apiKey field.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  newrelic_events_output: # [object] 
    type: # [string] Output Type
    region: # [string] Region - Which New Relic region endpoint to use.

    # -------------- if region is Custom ---------------

    customUrl: # [string] 

    # --------------------------------------------------------

    accountId: # [string] Account ID - New Relic account ID
    eventType: # [string] [required] Event type - Default eventType to use when not present in an event. For more information, see [here](https://docs.newrelic.com/docs/telemetry-data-platform/custom-data/custom-events/data-requirements-limits-custom-event-data/#reserved-words).
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1, maximum: 1024] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - New Relic API key. Can be overridden using __newRelic_apiKey field.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  influxdb_output: # [object] 
    type: # [string] Output Type
    url: # [string] Write API URL - URL of an InfluxDB cluster to send events to, e.g., http://localhost:8086/write
    useV2API: # [boolean] Use v2 API - The v2 API can be enabled with InfluxDB versions 1.8 and later.

    # -------------- if useV2API is false ---------------

    database: # [string] Database - Database to write to.

    # --------------------------------------------------------


    # -------------- if useV2API is true ---------------

    bucket: # [string] Bucket - Bucket to write to.
    org: # [string] Organization - Organization ID for this bucket.

    # --------------------------------------------------------

    timestampPrecision: # [string] Timestamp precision - Sets the precision for the supplied Unix time values. Defaults to milliseconds.
    dynamicValueFieldName: # [boolean] Dynamic value fields - Enabling this will pull the value field from the metric name. E,g, 'db.query.user' will use 'db.query' as the measurement and 'user' as the value field.
    valueFieldName: # [string] Value field name - Name of the field in which to store the metric when sending to InfluxDB. If dynamic generation is enabled and fails, this will be used as a fallback.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 51200] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication type - InfluxDB authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    loginUrl: # [string] Login URL - URL for OAuth
    secretParamName: # [string] OAuth Secret parameter name - Secret parameter name to pass in request body
    secret: # [string] OAuth secret - Secret parameter value to pass in request body
    tokenAttributeName: # [string] Token attribute name - Name of the auth token attribute in the OAuth response. Can be top-level (e.g., 'token'); or nested, using a period (e.g., 'data.token').
    spacer: # [null] 
    authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header value to pass in requests. The value `${token}` is used to reference the token obtained from authentication, e.g.: `Bearer ${token}`.
    tokenTimeoutSecs: # [number; minimum: 1, maximum: 300000] Refresh interval (secs.) - How often the OAuth token should be refreshed.
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Edge will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Edge will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  cloudwatch_output: # [object] 
    type: # [string] Output Type
    logGroupName: # [string] Log group name - CloudWatch log group to associate events with
    logStreamName: # [string] [required] Log stream prefix - Prefix for CloudWatch log stream name. This prefix will be used to generate a unique log stream name per cribl instance, for example: myStream_myHost_myOutputId
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] [required] Region - Region where the CloudWatchLogs is located
    endpoint: # [string] Endpoint - CloudWatchLogs service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to CloudWatchLogs-compatible endpoint.
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for CloudWatchLogs - Use Assume Role credentials to access CloudWatchLogs
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    maxQueueSize: # [number; minimum: 1, maximum: 32] Queue size limit - Maximum number of queued batches before blocking
    maxRecordSizeKB: # [number; minimum: 1, maximum: 10240] Record size limit (KB, uncompressed) - Maximum size (KB) of each individual record before compression. For non compressible data 1MB is the max recommended size
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  minio_output: # [object] 
    type: # [string] Output Type
    endpoint: # [string] [required] MinIO endpoint - MinIO service url (e.g. http://minioHost:9000)
    bucket: # [string] MinIO bucket name - Name of the destination MinIO bucket. This value can be a constant or a JavaScript expression that can only be evaluated at init time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - This value can be a constant or a JavaScript expression (`${C.env.SOME_ACCESS_KEY}`)

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key. This value can be a constant or a JavaScript expression, such as `${C.env.SOME_SECRET}`).
    region: # [string] Region - Region where the MinIO service/cluster is located
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    destPath: # [string] Key prefix - Root directory to prepend to path before uploading. Enter a constant, or a JavaScript expression enclosed in quotes or backticks.
    signatureVersion: # [string] Signature version - Signature version to use for signing MinIO requests
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    serverSideEncryption: # [string] Server-side encryption - Server-side encryption for uploaded objects
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates)
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value – if present – otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 86400] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 86400] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts limit - Maximum number of parts to upload in parallel per file. Minimum part size is 5MB.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  statsd_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is udp ---------------

    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if the destination is an IP address. A value of 0 means every batch sent will incur a DNS lookup.

    # --------------------------------------------------------


    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number; minimum: 1, maximum: 65535] [required] Port - Destination port.
    mtu: # [number; minimum: 1, maximum: 65535] Record size limit (bytes) - When protocol is UDP, specifies the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - When protocol is TCP, specifies how often buffers should be flushed, resulting in records sent to the destination.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  statsd_ext_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is udp ---------------

    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if the destination is an IP address. A value of 0 means every batch sent will incur a DNS lookup.

    # --------------------------------------------------------


    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number; minimum: 1, maximum: 65535] [required] Port - Destination port.
    mtu: # [number; minimum: 1, maximum: 65535] Record size limit (bytes) - When protocol is UDP, specifies the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - When protocol is TCP, specifies how often buffers should be flushed, resulting in records sent to the destination.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  graphite_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is udp ---------------

    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if the destination is an IP address. A value of 0 means every batch sent will incur a DNS lookup.

    # --------------------------------------------------------


    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number; minimum: 1, maximum: 65535] [required] Port - Destination port.
    mtu: # [number; minimum: 1, maximum: 65535] Record size limit (bytes) - When protocol is UDP, specifies the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - When protocol is TCP, specifies how often buffers should be flushed, resulting in records sent to the destination.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  router_output: # [object] 
    type: # [string] Output Type
    rules: # [array] Rules - Event routing rules
      - filter: # [string] Filter Expression - JavaScript expression to select events to send to output
        output: # [string] Output - Output to send matching events to
        description: # [string] Description - Description of this rule's purpose
        final: # [boolean] Final - Flag to control whether to stop the event from being checked against other rules
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  sns_output: # [object] 
    type: # [string] Output Type
    topicArn: # [string] Topic ARN - The ARN of the SNS topic to send events to. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. E.g., 'https://host:port/myQueueName'. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`
    messageGroupId: # [string] [required] Message Group ID - Messages in the same group are processed in a FIFO manner. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    maxRetries: # [number] Maximum number of retries - Maximum number of retries before the output returns an error. Note that not all errors are retryable. The retries use an exponential backoff policy.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - Region where the SNS is located
    endpoint: # [string] Endpoint - SNS service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to SNS-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing SNS requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for SNS - Use Assume Role credentials to access SNS
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  sqs_output: # [object] 
    type: # [string] Output Type
    queueName: # [string] Queue Name - The name, URL, or ARN of the SQS queue to send events to. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    queueType: # [string] [required] Queue Type - The queue type used (or created). Defaults to Standard.
    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    messageGroupId: # [string] Message Group ID - This parameter applies only to FIFO queues. The tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are processed in a FIFO manner. Use event field __messageGroupId to override this value.
    createQueue: # [boolean] Create Queue - Create queue if it does not exist.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - AWS Region where the SQS queue is located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - SQS service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to SQS-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing SQS requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for SQS - Use Assume Role credentials to access SQS
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    maxQueueSize: # [number; minimum: 1] Queue size limit - Maximum number of queued batches before blocking.
    maxRecordSizeKB: # [number; minimum: 1, maximum: 256] Record size limit (KB) - Maximum size (KB) of batches to send. Per the SQS spec, the max allowed value is 256 KB.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    maxInProgress: # [number; minimum: 1, maximum: 100] Concurrent request limit - The maximum number of in-progress API requests before backpressure is applied.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  snmp_output: # [object] 
    type: # [string] Output Type
    hosts: # [array] SNMP Trap Destinations - One or more SNMP destinations to forward traps to
      - host: # [string] Address - Destination host
        port: # [number; maximum: 65535] Port - Destination port, default is 162
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if all destinations are IP addresses. A value of 0 means every trap sent will incur a DNS lookup.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  sumo_logic_output: # [object] 
    type: # [string] Output Type
    url: # [string] API URL - Sumo Logic HTTP collector URL to which events should be sent
    customSource: # [string] Custom source name - Override the source name configured on the Sumo Logic HTTP collector. This can also be overridden at the event level with the __sourceName field.
    customCategory: # [string] Custom source category - Override the source category configured on the Sumo Logic HTTP collector. This can also be overridden at the event level with the __sourceCategory field.
    format: # [string] Data format - Preserve the raw event format instead of JSONifying it
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1, maximum: 1024] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  datadog_output: # [object] 
    type: # [string] Output Type
    contentType: # [string] Send logs as - The content type to use when sending logs
    message: # [string] Message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    source: # [string] Source - Name of the source to send with logs. When you send logs as JSON objects, the event's 'source' field (if set) will override this value.
    host: # [string] Host - Name of the host to send with logs. When you send logs as JSON objects, the event's 'host' field (if set) will override this value.
    service: # [string] Service - Name of the service to send with logs. When you send logs as JSON objects, the event's '__service' field (if set) will override this value.
    tags: # [array of strings] Datadog tags - List of tags to send with logs, such as 'env:prod' and 'env_staging:east'
    batchByTags: # [boolean] Batch by tags - Batch events by API key and the ddtags field on the event. When disabled, batches events only by API key. If incoming events have high cardinality in the ddtags field, disabling this setting may improve Destination performance.
    allowApiKeyFromEvents: # [boolean] Allow API key from events - Allow API key to be set from the event's '__agent_api_key' field
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
    severity: # [string] Severity - Default value for message severity. When you send logs as JSON objects, the event's '__severity' field (if set) will override this value.
    site: # [string] Datadog site - Datadog site to which events should be sent

    # -------------- if site is custom ---------------

    customUrl: # [string] 

    # --------------------------------------------------------

    sendCountersAsCount: # [boolean] Send counter metrics as 'count' - If not enabled, Datadog will transform 'counter' metrics to 'gauge'. [Learn more about Datadog metrics types.](https://docs.datadoghq.com/metrics/types/?tab=count)
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - Organization's API key in Datadog

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  grafana_cloud_output: # [object] 
    type: # [string] Output Type
    lokiUrl: # [string] Loki URL - The endpoint to send logs to, such as https://logs-prod-us-central1.grafana.net
    prometheusUrl: # [string] Prometheus URL - The remote_write endpoint to send Prometheus metrics to, such as https://prometheus-blocks-prod-us-central1.grafana.net/api/prom/push
    message: # [string] Logs message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    messageFormat: # [string] Message format - Format to use when sending logs to Loki (Protobuf or JSON)

    # -------------- if messageFormat is json ---------------

    compress: # [boolean] Compress - Compress the payload body before sending. Applies only to JSON payloads; the Protobuf variant for both Prometheus and Loki are snappy-compressed by default.

    # --------------------------------------------------------

    labels: # [array] Logs labels - List of labels to send with logs. Labels define Loki streams, so use static labels to avoid proliferating label value combinations and streams. Can be merged and/or overridden by the event's __labels field. Example: '__labels: {host: "cribl.io", level: "error"}'
      - name: # [string] Name
        value: # [string] Value
    metricRenameExpr: # [string] Metrics renaming expression - JavaScript expression that can be used to rename metrics. For example, name.replace(/\./g, '_') will replace all '.' characters in a metric's name with the supported '_' character. Use the 'name' global variable to access the metric's name. You can access event fields' values via __e.<fieldName>.
    prometheusAuth: # [object] 
      authType: # [string] Authentication type

      # -------------- if authType is token ---------------

      token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. Example: <your-username>:<your-api-key>

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # --------------------------------------------------------


      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for authentication
      password: # [string] Password - Password (API key in Grafana Cloud domain) for authentication

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

      # --------------------------------------------------------

    lokiAuth: # [object] 
      authType: # [string] Authentication type

      # -------------- if authType is token ---------------

      token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. Example: <your-username>:<your-api-key>

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # --------------------------------------------------------


      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for authentication
      password: # [string] Password - Password (API key in Grafana Cloud domain) for authentication

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

      # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking. Warning: Setting this value > 1 can cause Loki and Prometheus to complain about entries being delivered out of order.
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited). Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Maximum time between requests. Small values can reduce the payload size below the configured 'Max record size' and 'Max events per request'. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  loki_output: # [object] 
    type: # [string] Output Type
    url: # [string] Loki URL - The endpoint to send logs to
    message: # [string] Logs message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    messageFormat: # [string] Message format - Format to use when sending logs to Loki (Protobuf or JSON)

    # -------------- if messageFormat is json ---------------

    compress: # [boolean] Compress - Compress the payload body before sending

    # --------------------------------------------------------

    labels: # [array] Logs labels - List of labels to send with logs. Labels define Loki streams, so use static labels to avoid proliferating label value combinations and streams. Can be merged and/or overridden by the event's __labels field. Example: '__labels: {host: "cribl.io", level: "error"}'
      - name: # [string] Name
        value: # [string] Value
    authType: # [string] Authentication type

    # -------------- if authType is token ---------------

    token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. Example: <your-username>:<your-api-key>

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for authentication
    password: # [string] Password - Password (API key in Grafana Cloud domain) for authentication

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking. Warning: Setting this value > 1 can cause Loki to complain about entries being delivered out of order.
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Defaults to 0 (unlimited). Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Maximum time between requests. Small values can reduce the payload size below the configured 'Max record size' and 'Max events per request'. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  prometheus_output: # [object] 
    type: # [string] Output Type
    url: # [string] Remote Write URL - The endpoint to send metrics to
    metricRenameExpr: # [string] Metric renaming expression - JavaScript expression that can be used to rename metrics. For example, name.replace(/\./g, '_') will replace all '.' characters in a metric's name with the supported '_' character. Use the 'name' global variable to access the metric's name. You can access event fields' values via __e.<fieldName>.
    sendMetadata: # [boolean] Send metadata - Generate and send metadata (`type` and `metricFamilyName`) requests

    # -------------- if sendMetadata is true ---------------

    metricsFlushPeriodSec: # [number] Metadata flush period (sec) - How frequently metrics metadata is sent out. Value cannot be smaller than the base Flush period set above.

    # --------------------------------------------------------

    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication type - Remote Write authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    loginUrl: # [string] Login URL - URL for OAuth
    secretParamName: # [string] OAuth Secret parameter name - Secret parameter name to pass in request body
    secret: # [string] OAuth secret - Secret parameter value to pass in request body
    tokenAttributeName: # [string] Token attribute name - Name of the auth token attribute in the OAuth response. Can be top-level (e.g., 'token'); or nested, using a period (e.g., 'data.token').
    spacer: # [null] 
    authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header value to pass in requests. The value `${token}` is used to reference the token obtained from authentication, e.g.: `Bearer ${token}`.
    tokenTimeoutSecs: # [number; minimum: 1, maximum: 300000] Refresh interval (secs.) - How often the OAuth token should be refreshed.
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Edge will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Edge will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  ring_output: # [object] 
    type: # [string] Output Type
    format: # [string] Data format - Format of the output data.
    partitionExpr: # [string] Partitioning expression - JS expression to define how files are partitioned and organized. If left blank, Cribl Stream will fallback on event.__partition.
    maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
    maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
    compress: # [string] Data compression format
    destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  open_telemetry_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Protocol - Select a transport option for OpenTelemetry

    # -------------- if protocol is http ---------------

    httpTracesEndpointOverride: # [string] Traces endpoint override - If you want to send traces to the default `{endpoint}/v1/traces` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpMetricsEndpointOverride: # [string] Metrics endpoint override - If you want to send metrics to the default `{endpoint}/v1/metrics` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpLogsEndpointOverride: # [string] Logs endpoint override - If you want to send logs to the default `{endpoint}/v1/logs` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpCompress: # [string] Compression - Type of compression to apply to messages sent to the OpenTelemetry endpoint
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.

    # --------------------------------------------------------


    # -------------- if protocol is grpc ---------------

    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    keepAliveTime: # [number; minimum: 1] Keep alive time (seconds) - How often the sender should ping the peer to keep the connection open
    metadata: # [array] Metadata - List of key-value pairs to send with each gRPC request. Value supports JavaScript expressions that are evaluated just once, when the destination gets started. To pass credentials as metadata, use 'C.Secret'.
      - key: # [string] Key
        value: # [string] Value
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    compress: # [string] Compression - Type of compression to apply to messages sent to the OpenTelemetry endpoint

    # --------------------------------------------------------

    endpoint: # [string] Endpoint - The endpoint where OTel events will be sent. Enter any valid URL or an IP address (IPv4 or IPv6; enclose IPv6 addresses in square brackets). Unspecified ports will default to 4317, unless the endpoint is an HTTPS-based URL or TLS is enabled, in which case 443 will be used.
    otlpVersion: # [string] OTLP version - The version of OTLP Protobuf definitions to use when structuring data to send
    authType: # [string] Authentication type - OpenTelemetry authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    loginUrl: # [string] Login URL - URL for OAuth
    secretParamName: # [string] OAuth Secret parameter name - Secret parameter name to pass in request body
    secret: # [string] OAuth secret - Secret parameter value to pass in request body
    tokenAttributeName: # [string] Token attribute name - Name of the auth token attribute in the OAuth response. Can be top-level (e.g., 'token'); or nested, using a period (e.g., 'data.token').
    spacer: # [null] 
    authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header value to pass in requests. The value `${token}` is used to reference the token obtained from authentication, e.g.: `Bearer ${token}`.
    tokenTimeoutSecs: # [number; minimum: 1, maximum: 300000] Refresh interval (secs.) - How often the OAuth token should be refreshed.
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Edge will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Edge will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  service_now_output: # [object] 
    type: # [string] Output Type
    endpoint: # [string] Endpoint - The endpoint where ServiceNow events will be sent. Enter any valid URL or an IP address (IPv4 or IPv6; enclose IPv6 addresses in square brackets)
    tokenSecret: # [string] [required] Auth token (text secret) - Select or create a stored text secret
    authTokenName: # [string] Auth token name
    otlpVersion: # [string] [required] OTLP version - The version of OTLP Protobuf definitions to use when structuring data to send
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    protocol: # [string] [required] Protocol - Select a transport option for OpenTelemetry

    # -------------- if protocol is http ---------------

    httpTracesEndpointOverride: # [string] Traces endpoint override - If you want to send traces to the default `{endpoint}/v1/traces` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpMetricsEndpointOverride: # [string] Metrics endpoint override - If you want to send metrics to the default `{endpoint}/v1/metrics` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpLogsEndpointOverride: # [string] Logs endpoint override - If you want to send logs to the default `{endpoint}/v1/logs` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpCompress: # [string] Compression - Type of compression to apply to messages sent to the OpenTelemetry endpoint
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.

    # --------------------------------------------------------


    # -------------- if protocol is grpc ---------------

    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    keepAliveTime: # [number; minimum: 1] Keep alive time (seconds) - How often the sender should ping the peer to keep the connection open
    metadata: # [array] Metadata - List of key-value pairs to send with each gRPC request. Value supports JavaScript expressions that are evaluated just once, when the destination gets started. To pass credentials as metadata, use 'C.Secret'.
      - key: # [string] Key
        value: # [string] Value
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    compress: # [string] Compression - Type of compression to apply to messages sent to the OpenTelemetry endpoint

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  dataset_output: # [object] 
    type: # [string] Output Type
    messageField: # [string] Message field - Name of the event field that contains the message or attributes to send. If not specified, all of the event's non-internal fields will be sent as attributes.
    excludeFields: # [array of strings] Exclude fields - Fields to exclude from the event if the Message field is either unspecified or refers to an object. Ignored if the Message field is a string. If empty, we send all non-internal fields.
    serverHostField: # [string] Server/host field - Name of the event field that contains the `serverHost` identifier. If not specified, defaults to `cribl_<outputId>`.
    timestampField: # [string] Timestamp field - Name of the event field that contains the timestamp. If not specified, defaults to `ts`, `_time`, or `Date.now()`, in that order.
    defaultSeverity: # [string] Severity - Default value for event severity. If the `sev` or `__severity` fields are set on an event, the first one matching will override this value.
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    site: # [string] DataSet site - DataSet site to which events should be sent

    # -------------- if site is custom ---------------

    customUrl: # [string] 

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 6144] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - A 'Log Write Access' API key for the DataSet account

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  cribl_tcp_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number; maximum: 65535] Port - The port to connect to on the provided host

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    hosts: # [array] Destinations - Set of hosts to load-balance data to
      - host: # [string] Address - The hostname of the receiver
        port: # [number; maximum: 65535] Port - The port to connect to on the provided host
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (if not an IP); otherwise, uses the global TLS settings.
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes
    maxConcurrentSenders: # [number] Connection limit - Maximum number of concurrent connections (per Worker Process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending
    logFailedRequests: # [boolean] Log failed requests to disk - Use to troubleshoot issues with sending data
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    connectionTimeout: # [number] Connection timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tokenTTLMinutes: # [number; minimum: 1, maximum: 60] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60
    excludeFields: # [array of strings] Exclude fields - Fields to exclude from the event. By default, all internal fields except `__output` are sent. Example: `cribl_pipe`, `c*`. Wildcards supported.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  cribl_http_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - For optimal performance, enable load balancing even if you have one hostname, as it can expand to multiple IPs. If this setting is disabled, consider enabling round-robin DNS.

    # -------------- if loadBalanced is false ---------------

    url: # [string] Cribl endpoint - URL of a Cribl Worker to send events to, such as http://localhost:10200
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    urls: # [array] Cribl Worker endpoints
      - url: # [string] Cribl Endpoint - URL of a Cribl Worker to send events to, such as http://localhost:10200
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes

    # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certificates that are not authorized by a CA in the CA certificate path, or by another 
                    trusted CA (such as the system's). Defaults to Enabled. Overrides the toggle from Advanced Settings, when also present.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    tokenTTLMinutes: # [number; minimum: 1, maximum: 60] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires. Valid values are between 1 and 60.
    excludeFields: # [array of strings] Exclude fields - Fields to exclude from the event. By default, all internal fields except `__output` are sent. Example: `cribl_pipe`, `c*`. Wildcards supported.
    compression: # [string] Compression - Codec to use to compress the data before sending
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  humio_hec_output: # [object] 
    type: # [string] Output Type
    url: # [string] [required] LogScale endpoint - URL to a CrowdStrike Falcon LogScale endpoint to send events to. Examples: https://cloud.us.humio.com/api/v1/ingest/hec for JSON and https://cloud.us.humio.com/api/v1/ingest/hec/raw for raw
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 32768] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    format: # [string] Request format - When set to JSON, the event is automatically formatted with required fields before sending. When set to Raw, only the event's `_raw` value is sent.
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] LogScale auth token - CrowdStrike Falcon LogScale authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] LogScale auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  crowdstrike_next_gen_siem_output: # [object] 
    type: # [string] Output Type
    url: # [string] [required] Next-Gen SIEM endpoint - URL provided from a CrowdStrike data connector. 
Example: https://ingest.<region>.crowdstrike.com/api/ingest/hec/<connection-id>/v1/services/collector
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 32768] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    format: # [string] Request format - When set to JSON, the event is automatically formatted with required fields before sending. When set to Raw, only the event's `_raw` value is sent.
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] Next-Gen SIEM authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Next-Gen SIEM authentication token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  dl_s3_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] S3 bucket name - Name of the destination S3 bucket. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`
    region: # [string] Region - Region where the S3 bucket is located
    awsSecretKey: # [string] Secret key - Secret key. This value can be a constant or a JavaScript expression. Example: `${C.env.SOME_SECRET}`)
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - This value can be a constant or a JavaScript expression (`${C.env.SOME_ACCESS_KEY}`)

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    destPath: # [string] Key prefix - Prefix to append to files before uploading. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `myKeyPrefix-${C.vars.myVar}`
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    serverSideEncryption: # [string] Server-side encryption for uploaded objects

    # -------------- if serverSideEncryption is aws:kms ---------------

    kmsKeyId: # [string] KMS key ID - ID or ARN of the KMS customer-managed key to use for encryption

    # --------------------------------------------------------

    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    format: # [string] Data format - Format of the output data

    # -------------- if format is json ---------------

    compress: # [string] Compression - Data compression format to apply to HTTP content before it is delivered
    compressionLevel: # [string] Compression level - Compression level to apply before moving files to final destination

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 86400] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 86400] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts upload limit - Maximum number of parts to upload in parallel per file. Minimum part size is 5MB.
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    maxClosingFilesToBackpressure: # [number; minimum: 10, maximum: 4200] Staging file limit - Maximum number of files that can be waiting for upload before backpressure is applied
    partitioningFields: # [array of strings] Partition by fields - List of fields to partition the path by, in addition to time, which is included automatically. The effective partition will be YYYY/MM/DD/HH/<list/of/fields>.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  security_lake_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] S3 bucket name - Name of the destination S3 bucket. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`
    region: # [string] [required] Region - Region where the Amazon Security Lake is located.
    awsSecretKey: # [string] Secret key
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - This value can be a constant or a JavaScript expression (`${C.env.SOME_ACCESS_KEY}`)

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    endpoint: # [string] Endpoint - Amazon Security Lake service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to Amazon Security Lake-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing Amazon Security Lake requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] [required] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    serverSideEncryption: # [string] Server-side encryption for uploaded objects

    # -------------- if serverSideEncryption is aws:kms ---------------

    kmsKeyId: # [string] KMS key ID - ID or ARN of the KMS customer-managed key to use for encryption

    # --------------------------------------------------------

    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 86400] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 86400] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts upload limit - Maximum number of parts to upload in parallel per file. Minimum part size is 5MB.
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    maxClosingFilesToBackpressure: # [number; minimum: 10, maximum: 4200] Staging file limit - Maximum number of files that can be waiting for upload before backpressure is applied
    accountId: # [string] [required] Account ID - ID of the AWS account whose data the Destination will write to Security Lake. This should have been configured when creating the Amazon Security Lake custom source.
    customSource: # [string] [required] Custom source name - Name of the custom source configured in Amazon Security Lake
    automaticSchema: # [boolean] Automatic schema - Automatically calculate the schema based on the events of each Parquet file generated

    # -------------- if automaticSchema is false ---------------

    parquetSchema: # [string] Parquet schema - To add a new schema, navigate to Processing > Knowledge > Parquet Schemas

    # --------------------------------------------------------

    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that some reader implementations use Data page V2's attributes to work more efficiently, while others ignore it.
    parquetRowGroupLength: # [number; minimum: 1, maximum: 67108864] Group row limit - The number of rows that every group will contain. The final group can contain a smaller number of rows.
    parquetPageSize: # [string] Page size - Target memory size for page segments, such as 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Log up to 3 rows that Cribl Edge skips due to data mismatch
    keyValueMetadata: # [array] Metadata (optional) - The metadata of files the Destination writes will include the properties you add here as key-value pairs. Useful for tagging. Examples: "key":"OCSF Event Class", "value":"9001"
      - key: # [string] Key
        value: # [string] Value
    enableStatistics: # [boolean] Write statistics - Statistics profile an entire file in terms of minimum/maximum values within data, numbers of nulls, etc. You can use Parquet tools to view statistics.
    enableWritePageIndex: # [boolean] Write page indexes - One page index contains statistics for one data page. Parquet readers use statistics to enable page skipping.
    enablePageChecksum: # [boolean] Write page checksum - Parquet tools can use the checksum of a Parquet page to verify data integrity
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  cribl_lake_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] S3 bucket name - Name of the destination S3 bucket. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`
    region: # [string] Region - Region where the S3 bucket is located
    awsSecretKey: # [string] Secret key - Secret key. This value can be a constant or a JavaScript expression. Example: `${C.env.SOME_SECRET}`)
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    stagePath: # [string] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant and stable storage.
    addIdToStagePath: # [boolean] Add output ID - Add the Output ID value to staging location
    destPath: # [string] Lake dataset - Lake dataset to send the data to.
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects
    storageClass: # [string] Storage class - Storage class to select for uploaded objects
    serverSideEncryption: # [string] Server-side encryption for uploaded objects

    # -------------- if serverSideEncryption is aws:kms ---------------

    kmsKeyId: # [string] KMS key ID - ID or ARN of the KMS customer-managed key to use for encryption

    # --------------------------------------------------------

    removeEmptyDirs: # [boolean] Remove empty staging directories - Remove empty staging directories after moving files

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number; minimum: 10, maximum: 86400] Staging cleanup period - How frequently, in seconds, to clean up empty directories

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant)
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`).
    maxFileSizeMB: # [number; minimum: 5, maximum: 1024] File size limit (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxOpenFiles: # [number; minimum: 10, maximum: 2000] Open file limit - Maximum number of files to keep open concurrently. When exceeded, Cribl Edge will close the oldest open files and move them to the final output location.
    headerLine: # [string] Header line - If set, this line will be written to the beginning of each output file
    writeHighWaterMark: # [number; minimum: 16, maximum: 4096] Writing high watermark (KB) - Buffer size used to write to a file
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure
    deadletterEnabled: # [boolean] Enable dead-lettering - If a file fails to move to its final destination after the maximum number of retries, move it to a designated directory to prevent further errors

    # -------------- if deadletterEnabled is true ---------------

    deadletterPath: # [string] Dead-letter location - Storage location for files that fail to reach their final destination after maximum retries are exceeded
    maxRetryNum: # [number; minimum: 1] Retry limit - The maximum number of times a file will attempt to move to its final destination before being dead-lettered

    # --------------------------------------------------------

    onDiskFullBackpressure: # [string] Disk space protection - How to handle events when disk space is below the global 'Min free disk space' limit
    maxFileOpenTimeSec: # [number; minimum: 10, maximum: 86400] File open time limit (sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number; minimum: 5, maximum: 86400] Idle time limit (sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    verifyPermissions: # [boolean] Verify if bucket exists - Disable if you can access files within the bucket but not the bucket itself
    maxClosingFilesToBackpressure: # [number; minimum: 10, maximum: 4200] Staging file limit - Maximum number of files that can be waiting for upload before backpressure is applied
    awsAuthenticationMethod: # [string] 
    format: # [string] 
    maxConcurrentFileParts: # [number; minimum: 1, maximum: 10] Concurrent file parts limit - Maximum number of parts to upload in parallel per file. Minimum part size is 5MB.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  disk_spool_output: # [object] 
    type: # [string] Output Type
    timeWindow: # [string] Bucket time span - Time period for grouping spooled events. Default is 10m.
    maxDataSize: # [string] Data size limit - Maximum disk space that can be consumed before older buckets are deleted. Examples: 420MB, 4GB. Default is 1GB.
    maxDataTime: # [string] Data age limit - Maximum amount of time to retain data before older buckets are deleted. Examples: 2h, 4d. Default is 24h.
    compress: # [string] Compression - Data compression format. Default is gzip.
    partitionExpr: # [string] Partitioning expression - JavaScript expression defining how files are partitioned and organized within the time-buckets. If blank, the event's __partition property is used and otherwise, events go directly into the time-bucket directory.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  click_house_output: # [object] 
    type: # [string] Output Type
    url: # [string] [required] URL - URL of the ClickHouse instance. Example: http://localhost:8123/
    authType: # [string] Authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    loginUrl: # [string] Login URL - URL for OAuth
    secretParamName: # [string] OAuth Secret parameter name - Secret parameter name to pass in request body
    secret: # [string] OAuth secret - Secret parameter value to pass in request body
    tokenAttributeName: # [string] Token attribute name - Name of the auth token attribute in the OAuth response. Can be top-level (e.g., 'token'); or nested, using a period (e.g., 'data.token').
    spacer: # [null] 
    authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header value to pass in requests. The value `${token}` is used to reference the token obtained from authentication, e.g.: `Bearer ${token}`.
    tokenTimeoutSecs: # [number; minimum: 1, maximum: 300000] Refresh interval (secs.) - How often the OAuth token should be refreshed.
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Edge will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Edge will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------


    # -------------- if authType is sslUserCertificate ---------------

    sqlUsername: # [string] Username - Username for certificate authentication

    # --------------------------------------------------------

    database: # [string] ClickHouse database
    tableName: # [string] [required] ClickHouse table - Name of the ClickHouse table where data will be inserted. Name can contain letters (A-Z, a-z), numbers (0-9), and the character "_", and must start with either a letter or the character "_".
    format: # [string] Format - Data format to use when sending data to ClickHouse. Defaults to JSON Compact.
    mappingType: # [string] Mapping type - How event fields are mapped to ClickHouse columns.

    # -------------- if mappingType is automatic ---------------

    excludeMappingFields: # [array of strings] Exclude fields - Fields to exclude from sending to ClickHouse

    # --------------------------------------------------------


    # -------------- if mappingType is custom ---------------

    describeTable: # [string] Retrieve table columns - Retrieves the table schema from ClickHouse and populates the Column Mapping table
    columnMappings: # [array] Column Mapping
      - columnName: # [string] Column name - Name of the column in ClickHouse that will store field value
        columnType: # [string] Column type - Type of the column in the ClickHouse database
        columnValueExpression: # [string] Column value - JavaScript expression to compute value to be inserted into ClickHouse table

    # --------------------------------------------------------

    asyncInserts: # [boolean] Async inserts - Collect data into batches for later processing. Disable to write to a ClickHouse table immediately.

    # -------------- if asyncInserts is true ---------------

    waitForAsyncInserts: # [boolean] Wait for async inserts - Cribl will wait for confirmation that data has been fully inserted into the ClickHouse database before proceeding. Disabling this option can increase throughput, but Cribl won’t be able to verify data has been completely inserted.

    # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate - The name of the predefined certificate
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 10240] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    dumpFormatErrorsToDisk: # [boolean] Log last schema mismatch - Log the most recent event that fails to match the table schema
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  xsiam_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Enable for optimal performance. Even if you have one hostname, it can expand to multiple IPs. If disabled, consider enabling round-robin DNS.

    # -------------- if loadBalanced is false ---------------

    url: # [string] XSIAM endpoint - XSIAM endpoint URL to send events to, such as https://api-{tenant external URL}/logs/v1/event
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames
    urls: # [array] XSIAM Endpoints
        weight: # [number] Load Weight - Assign a weight (>0) to each endpoint to indicate its traffic-handling capability
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (seconds) - The interval in which to re-resolve any hostnames and pick up destinations from A records
    loadBalanceStatsPeriodSec: # [number; minimum: 10] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 100, maximum: 10000] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # -------------- if authType is token ---------------

    token: # [string] Auth token - XSIAM authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    throttleRateReqPerSec: # [integer; maximum: 2000] Throttle request rate limit - Maximum number of requests to limit to per second
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  netflow_output: # [object] 
    type: # [string] Output Type
    hosts: # [array] NetFlow Destinations - One or more NetFlow destinations to forward events to
      - host: # [string] Address - Destination host
        port: # [number; maximum: 65535] Port - Destination port, default is 2055
    dnsResolvePeriodSec: # [number; maximum: 86400] DNS resolution period (sec) - How often to resolve the destination hostname to an IP address. Ignored if all destinations are IP addresses. A value of 0 means every datagram sent will incur a DNS lookup.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  dynatrace_http_output: # [object] 
    type: # [string] Output Type
    method: # [string] Method - The method to use when sending events
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 5000] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number; maximum: 50000] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events. You can also add headers dynamically on a per-event basis in the __headers field, as explained in [Cribl Docs](https://docs.cribl.io/stream/destinations-webhook/#internal-fields).
      - name: # [string] Field Name
        value: # [string] Field Value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    authType: # [string] Authentication type

    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    format: # [string] Format - How to format events before sending. Defaults to JSON. Plaintext is not currently supported.
    endpoint: # [string] [required] Endpoint

    # -------------- if endpoint is cloud ---------------

    environmentId: # [string] Environment ID - ID of the environment to send to

    # --------------------------------------------------------


    # -------------- if endpoint is activeGate ---------------

    activeGateDomain: # [string] ActiveGate domain - ActiveGate domain with Log analytics collector module enabled. For example https://{activeGate-domain}:9999/e/{environment-id}/api/v2/logs/ingest.
    environmentId: # [string] Environment ID - ID of the environment to send to

    # --------------------------------------------------------


    # -------------- if endpoint is manual ---------------

    url: # [string] URL - URL to send events to. Can be overwritten by an event's __url field.

    # --------------------------------------------------------

    telemetryType: # [string] [required] Telemetry type
    totalMemoryLimitKB: # [number] Buffer memory limit (KB) - Maximum total size of the batches waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  dynatrace_otlp_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] [required] Protocol - Select a transport option for Dynatrace

    # -------------- if protocol is http ---------------

    httpTracesEndpointOverride: # [string] Traces endpoint override - If you want to send traces to the default `{endpoint}/v1/traces` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpMetricsEndpointOverride: # [string] Metrics endpoint override - If you want to send metrics to the default `{endpoint}/v1/metrics` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpLogsEndpointOverride: # [string] Logs endpoint override - If you want to send logs to the default `{endpoint}/v1/logs` endpoint, leave this field empty; otherwise, specify the desired endpoint
    httpCompress: # [string] Compression - Type of compression to apply to messages sent to the OpenTelemetry endpoint
    keepAlive: # [boolean] Keep alive - Disable to close the connection immediately after sending the outgoing request
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable round-robin DNS lookup. When a DNS server returns multiple addresses, Cribl Edge will cycle through them in the order returned. For optimal performance, consider enabling this setting for non-load balanced destinations.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.

    # --------------------------------------------------------

    endpoint: # [string] Endpoint - The endpoint where Dynatrace events will be sent. Enter any valid URL or an IP address (IPv4 or IPv6; enclose IPv6 addresses in square brackets)
    otlpVersion: # [string] [required] OTLP version - The version of OTLP Protobuf definitions to use when structuring data to send
    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 4096] Body size limit - Maximum size (in KB) of the request body. The maximum payload size is 4 MB. If this limit is exceeded, the entire OTLP message is dropped
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    endpointType: # [string] [required] Endpoint type - Select the type of Dynatrace endpoint configured
    tokenSecret: # [string] [required] Auth token (text secret) - Select or create a stored text secret
    authTokenName: # [string] Api-Token name
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
  sentinel_one_ai_siem_output: # [object] 
    type: # [string] Output Type
    region: # [string] Region - The SentinelOne region to send events to. In most cases you can find the region by either looking at your SentinelOne URL or knowing what geographic region your SentinelOne instance is contained in.

    # -------------- if region is Custom ---------------

    baseUrl: # [string] Base AI SIEM endpoint URL - Base URL of the endpoint used to send events to, such as https://<Your-S1-Tenant>.sentinelone.net. Must begin with http:// or https://, can include a port number, and no trailing slashes. Matches pattern: ^https?://[a-zA-Z0-9.-]+(:[0-9]+)?$.

    # --------------------------------------------------------

    endpoint: # [string] [required] AI SIEM endpoint Path - Regional endpoint used to send events to, such as /services/collector/event or /services/collector/raw

    # -------------- if endpoint is /services/collector/event ---------------

    hostExpression: # [string] serverHost expression - Define serverHost for events using a JavaScript expression. You must enclose text constants in quotes (such as, 'myServer').
    sourceExpression: # [string] logFile expression - Define logFile for events using a JavaScript expression. You must enclose text constants in quotes (such as, 'myLogFile.txt').
    sourceTypeExpression: # [string] parser expression - Define the parser for events using a JavaScript expression. This value helps parse data into AI SIEM. You must enclose text constants in quotes (such as, 'dottedJson'). For custom parsers, substitute 'dottedJson' with your parser's name.
    dataSourceCategoryExpression: # [string] dataSource.category expression - Define the dataSource.category for events using a JavaScript expression. This value helps categorize data and helps enable extra features in SentinelOne AI SIEM. You must enclose text constants in quotes. The default value is 'security'.
    dataSourceNameExpression: # [string] dataSource.name expression - Define the dataSource.name for events using a JavaScript expression. This value should reflect the type of data being inserted into AI SIEM. You must enclose text constants in quotes (such as, 'networkActivity' or 'authLogs').
    dataSourceVendorExpression: # [string] dataSource.vendor expression - Define the dataSource.vendor for events using a JavaScript expression. This value should reflect the vendor of the data being inserted into AI SIEM. You must enclose text constants in quotes (such as, 'Cisco' or 'Microsoft').
    eventTypeExpression: # [string] event.type expression - Optionally, define the event.type for events using a JavaScript expression. This value acts as a label, grouping events into meaningful categories. You must enclose text constants in quotes (such as, 'Process Creation' or 'Network Connection').

    # --------------------------------------------------------


    # -------------- if endpoint is /services/collector/raw ---------------

    host: # [string] serverHost expression - Define the serverHost for events using a JavaScript expression. This value will be passed to AI SIEM. You must enclose text constants in quotes (such as, 'myServerName').
    source: # [string] logFile - Specify the logFile value to pass as a parameter to SentinelOne AI SIEM. Don't quote this value. The default is cribl.
    sourceType: # [string] parser - Specify the sourcetype parameter for SentinelOne AI SIEM, which determines the parser. Don't quote this value. For custom parsers, substitute hecRawParser with your parser's name. The default is hecRawParser.
    dataSourceCategory: # [string] dataSource.category - Specify the dataSource.category value to pass as a parameter to SentinelOne AI SIEM. This value helps categorize data and enables additional features. Don't quote this value. The default is security.
    dataSourceName: # [string] dataSource.name - Specify the dataSource.name value to pass as a parameter to AI SIEM. This value should reflect the type of data being inserted. Don't quote this value. The default is cribl.
    dataSourceVendor: # [string] dataSource.vendor - Specify the dataSource.vendorvalue to pass as a parameter to AI SIEM. This value should reflect the vendor of the data being inserted. Don't quote this value. The default is cribl.
    eventType: # [string] event.type - Specify the event.type value to pass as an optional parameter to AI SIEM. This value acts as a label, grouping events into meaningful categories like Process Creation, File Modification, or Network Connection. Don't quote this value. By default, this field is empty.

    # --------------------------------------------------------

    concurrency: # [number; minimum: 1, maximum: 32] Request concurrency - Maximum number of ongoing requests before blocking
    maxPayloadSizeKB: # [number; minimum: 1024, maximum: 2097152] Body size limit (KB) - Maximum size, in KB, of the request body
    maxPayloadEvents: # [number] Events-per-request limit - Maximum number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Compress the payload body before sending
    rejectUnauthorized: # [boolean] Validate server certs - Reject certificates not authorized by a CA in the CA certificate path or by another trusted CA (such as the system's). 
        Enabled by default. When this setting is also present in TLS Settings (Client Side), 
        that value will take precedence.
    timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Request timeout - Amount of time, in seconds, to wait for a request to complete before canceling it
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Body size limit.
    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    failedRequestLoggingMode: # [string] Failed request logging mode - Data to log when a request fails. All headers are redacted by default, unless listed as safe headers below.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # -------------- if authType is manual ---------------

    token: # [string] AI SIEM API Key - In the SentinelOne Console select Policy & Settings then select the Singularity AI SIEM section, API Keys will be at the bottom. Under Log Access Keys select a Write token and copy it here

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] AI SIEM API Key (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    responseRetrySettings: # [array] Settings for failed HTTP requests - Automatically retry after unsuccessful response status codes, such as 429 (Too Many Requests) or 503 (Service Unavailable)
      - httpStatus: # [number; minimum: 100, maximum: 599] HTTP status code - The HTTP response status code that will trigger retries
        initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
        backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
        maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).
    timeoutRetrySettings: # [object] 
      timeoutRetry: # [boolean] Retry timed-out HTTP requests

      # -------------- if timeoutRetry is true ---------------

      initialBackoff: # [number; maximum: 600000] Pre-backoff interval (ms) - How long, in milliseconds, Cribl Stream should wait before initiating backoff. Maximum interval is 600,000 ms (10 minutes).
      backoffRate: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. A value of 2 (default) means Cribl Stream will retry after 2 seconds, then 4 seconds, then 8 seconds, etc.
      maxBackoff: # [number; minimum: 10000, maximum: 180000] Backoff limit (ms) - The maximum backoff interval, in milliseconds, Cribl Stream should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

      # --------------------------------------------------------

    responseHonorRetryAfterHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) no longer than 180 seconds after the retry request. Cribl Edge limits the delay to 180 seconds, even if the Retry-After header specifies a longer delay. When enabled, takes precedence over user-configured retry options. When disabled, all Retry-After headers are ignored.
    onBackpressure: # [string] Backpressure behavior - How to handle events when all receivers are exerting backpressure

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.)
    pqMaxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data
    pqOnBackpressure: # [string] Queue-full behavior - How to handle events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqMode: # [string] Mode - In Error mode, PQ writes events to the filesystem if the Destination is unavailable. In Backpressure mode, PQ writes events to the filesystem when it detects backpressure from the Destination. In Always On mode, PQ always writes events to the filesystem.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output
    systemFields: # [array of strings] System fields - Fields to automatically add to events, such as cribl_pipe. Supports wildcards.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Edge
```