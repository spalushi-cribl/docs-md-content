# inputs.yml


`inputs.yml` contains settings for configuring inputs into Cribl.

```yaml {title="$CRIBL_HOME/default/cribl/inputs.yml"}
inputs: # [object] 
  collection_input: # [object] 
    type: # [string] Input Type
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    preprocess: # [object] 
      disabled: # [boolean] Disabled - Enable Custom Command
      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  kafka_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Brokers - List of Kafka brokers to use, eg. localhost:9092
    topics: # [array of strings] [required] Topic - Topic to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Kafka Source to only a single topic.
    groupId: # [string] Group ID - Specifies the consumer group this instance belongs to, default is 'Cribl'.
    fromBeginning: # [boolean] From beginning - Whether to start reading from earliest available data, relevant only during initial subscription.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled - Enable Schema Registry

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for access to the Confluent Schema Registry, i.e: http://localhost:8081
      tls: # [object] TLS settings (client side)
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
        servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
        certificateName: # [string] Certificate name - The name of the predefined certificate.
        caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
        privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
        certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
        passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
        minVersion: # [string] Minimum TLS version - Minimum TLS version to use when connecting
        maxVersion: # [string] Maximum TLS version - Maximum TLS version to use when connecting

        # --------------------------------------------------------


      # --------------------------------------------------------

    connectionTimeout: # [number] Connection timeout (ms) - Maximum time to wait for a successful connection.
    requestTimeout: # [number] Request timeout (ms) - Maximum time to wait for a successful request.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled - Enable Authentication

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism - SASL authentication mechanism to use.

      # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate name - The name of the predefined certificate.
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to use when connecting
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to use when connecting

      # --------------------------------------------------------

    sessionTimeout: # [number] Session timeout (ms) - 
      Timeout used to detect client failures when using Kafka's group management facilities.
      If the client sends the broker no heartbeats before this timeout expires, 
      the broker will remove this client from the group, and will initiate a rebalance.
      Value must be between the broker's configured group.min.session.timeout.ms and group.max.session.timeout.ms.
      See details [here](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms).
    rebalanceTimeout: # [number] Rebalance timeout (ms) - 
      Maximum allowed time for each worker to join the group after a rebalance has begun.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See details [here](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms).
    heartbeatInterval: # [number] Heartbeat interval (ms) - 
      Expected time between heartbeats to the consumer coordinator when using Kafka's group management facilities.
      Value must be lower than sessionTimeout, and typically should not exceed 1/3 of the sessionTimeout value.
      See details [here](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms).
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  http_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthed access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    criblAPI: # [string] Cribl HTTP Event API - Absolute path on which to listen for the Cribl HTTP API requests. At the moment, only _bulk (default /cribl/_bulk) is available. Use empty string to disable.
    elasticAPI: # [string] Elasticsearch API endpoint (Bulk API) - Absolute path on which to listen for the Elasticsearch API requests. At the moment only _bulk (default /elastic/_bulk) is available. Use empty string to disable
    splunkHecAPI: # [string] Splunk HEC Endpoint - Absolute path on which listen for the Splunk HTTP Event Collector API requests. Use empty string to disable.
    splunkHecAcks: # [boolean] Splunk HEC Acks - Whether to enable Splunk HEC acknowledgements
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to establish a connection.
    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    authTokens: # [array] Auth tokens - Shared secrets to be provided by any Splunk forwarder. IfÂ empty, unauthed access is permitted.
      - token: # [string] Token - Shared secrets to be provided by any Splunk forwarder. IfÂ empty, unauthed access is permitted.
        description: # [string] Description - Optional token description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_search_input: # [object] 
    searchHead: # [string] Search head - Search head base URL, can be expression, default is https://localhost:8089.
    search: # [string] [required] Search - Enter Splunk search here. For example: 'index=myAppLogs level=error channel=myApp' OR '| mstats avg(myStat) as myStat WHERE index=myStatsIndex.'
    earliest: # [string] Earliest - The earliest time boundary for the search. Can be an exact or relative time. For example: '2022-01-14T12:00:00Z' or '-16m@m'
    latest: # [string] Latest - The latest time boundary for the search. Can be an exact or relative time. For example: '2022-01-14T12:00:00Z' or '-1m@m'
    cronSchedule: # [string] [required] Cron schedule - A cron schedule on which to run this job.
    endpoint: # [string] [required] Search endpoint - REST API used to create a search.
    outputMode: # [string] [required] Output mode - Format of the returned output
    endpointParams: # [array] Endpoint parameters - Optional request parameters to send to the endpoint.
      - name: # [string] Name - Parameter name
        value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
    endpointHeaders: # [array] Endpoint headers - Optional request headers to send to the endpoint.
      - name: # [string] Name - Header Name
        value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
    logLevel: # [string] Log level - Collector runtime log Level (verbosity).
    requestTimeout: # [number] Request timeout (seconds) - HTTP request inactivity timeout, use 0 to disable
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    keepAliveTime: # [number] Keep Alive Time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number] Worker Timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    authType: # [string] Authentication type - Splunk Search authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

    # --------------------------------------------------------

    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_hec_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    authTokens: # [array] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthed access is permitted
      - token: # [string] Token - Shared secret to be provided by any client (Authorization: <token>).
        description: # [string] Description - Optional token description
        metadata: # [array] Fields - Fields to add to events referencing this token.
        - name: # [string] Name - Field name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    splunkHecAPI: # [string] [required] Splunk HEC Endpoint - Absolute path on which to listen for the Splunk HTTP Event Collector API requests. This input supports the /event and /raw endpoints.
    metadata: # [array] Fields - Fields to add to every event. May be overridden by fields added at the token or request level.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    allowedIndexes: # [array of strings] Allowed Indexes - List values allowed in HEC event index field, allows wildcards. Leave blank to skip validation.
    splunkHecAcks: # [boolean] Splunk HEC Acks - Whether to enable Splunk HEC acknowledgements
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  azure_blob_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The storage account queue name blob notifications will be read from. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `myQueue-${C.vars.myVar}`
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    visibilityTimeout: # [number] Visibility timeout (secs) - The duration (in seconds) that the received messages are hidden from subsequent retrieve requests after being retrieved by a ReceiveMessage request.
    numReceivers: # [number] Num receivers - The Number of receiver processes to run, the higher the number the better throughput at the expense of CPU overhead
    maxMessages: # [number] Max messages - The maximum number of messages to return in a poll request. Azure storage queues never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 32.
    servicePeriodSecs: # [number] Service period (secs) - The duration (in seconds) which pollers should be validated and restarted if exited
    skipOnError: # [boolean] Skip file on error - Toggle to Yes to skip files that trigger a processing error. Defaults to No, which enables retries after processing errors.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    parquetChunkSizeMB: # [number] Max Parquet chunk size (MB) - Maximum file size for each Parquet chunk.
    parquetChunkDownloadTimeout: # [number] Parquet chunk download timeout (seconds) - The maximum time to wait for a Parquet file's chunk to be downloaded. Processing will end if a required chunk could not be downloaded within the time imposed by this setting.
    authType: # [string] Authentication method - Enter connection string directly, or select a stored secret
    connectionString: # [string] Connection string - Enter your Azure Storage account connection string. If left blank, Stream will fall back to env.AZURE_STORAGE_CONNECTION_STRING.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Connection string (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  elastic_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    elasticAPI: # [string] [required] Elasticsearch API endpoint - Absolute path on which to listen for Elasticsearch API requests. Defaults to /. _bulk will be appended automatically, e.g., /myPath becomes /myPath/_bulk. Requests can then be made to either /myPath/_bulk or /myPath/<myIndexName>/_bulk. Other entries are faked as success.
    authType: # [string] Authentication type - Elastic authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is authTokens ---------------

    authTokens: # [array of strings] Token - Bearer tokens to include in the authorization header

    # --------------------------------------------------------

    apiVersion: # [string] API Version - The API version to use for communicating with the server.

    # -------------- if apiVersion is custom ---------------

    customAPIVersion: # [string] Custom API Version - Custom version information to respond to requests

    # --------------------------------------------------------

    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    proxyMode: # [object] 
      enabled: # [boolean] Enable Proxy Mode - Enable proxying of non-bulk API requests to an external Elastic server. Enable this only if you understand the implications; see docs for more details.

      # -------------- if enabled is true ---------------

      url: # [string] Proxy URL - URL of the Elastic server to proxy non-bulk requests to, e.g., http://elastic:9200
      removeHeaders: # [array of strings] Remove headers - List of headers to remove from the request to proxy
      timeoutSec: # [number] Proxy request timeout - Amount of time, in seconds, to wait for a proxy request to complete before aborting it.
      authType: # [string] Authentication method - Enter credentials directly, or select a stored secret

      # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  confluent_cloud_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Brokers - List of Confluent Cloud brokers to use, eg. yourAccount.confluent.cloud:9092
    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
      servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
      certificateName: # [string] Certificate name - The name of the predefined certificate.
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to use when connecting
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to use when connecting

      # --------------------------------------------------------

    topics: # [array of strings] [required] Topic - Topic to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Kafka Source to only a single topic.
    groupId: # [string] Group ID - Specifies the consumer group this instance belongs to, default is 'Cribl'.
    fromBeginning: # [boolean] From beginning - Whether to start reading from earliest available data, relevant only during initial subscription.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled - Enable Schema Registry

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for access to the Confluent Schema Registry, i.e: http://localhost:8081
      tls: # [object] TLS settings (client side)
        disabled: # [boolean] Disabled

        # -------------- if disabled is false ---------------

        rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
        servername: # [string] Server name (SNI) - Server name for the SNI (Server Name Indication) TLS extension. It must be a host name, and not an IP address.
        certificateName: # [string] Certificate name - The name of the predefined certificate.
        caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
        privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
        certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
        passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
        minVersion: # [string] Minimum TLS version - Minimum TLS version to use when connecting
        maxVersion: # [string] Maximum TLS version - Maximum TLS version to use when connecting

        # --------------------------------------------------------


      # --------------------------------------------------------

    connectionTimeout: # [number] Connection timeout (ms) - Maximum time to wait for a successful connection.
    requestTimeout: # [number] Request timeout (ms) - Maximum time to wait for a successful request.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled - Enable Authentication

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism - SASL authentication mechanism to use.

      # --------------------------------------------------------

    sessionTimeout: # [number] Session timeout (ms) - 
      Timeout used to detect client failures when using Kafka's group management facilities.
      If the client sends the broker no heartbeats before this timeout expires, 
      the broker will remove this client from the group, and will initiate a rebalance.
      Value must be between the broker's configured group.min.session.timeout.ms and group.max.session.timeout.ms.
      See details [here](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms).
    rebalanceTimeout: # [number] Rebalance timeout (ms) - 
      Maximum allowed time for each worker to join the group after a rebalance has begun.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See details [here](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms).
    heartbeatInterval: # [number] Heartbeat interval (ms) - 
      Expected time between heartbeats to the consumer coordinator when using Kafka's group management facilities.
      Value must be lower than sessionTimeout, and typically should not exceed 1/3 of the sessionTimeout value.
      See details [here](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms).
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  grafana_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    prometheusAPI: # [string] Remote Write API endpoint - Absolute path on which to listen for Grafana Agent's Remote Write requests. Defaults to /api/prom/push, which will expand as: http://<yourâupstreamâURL>:<yourâport>/api/prom/push.
    lokiAPI: # [string] Logs API endpoint - Absolute path on which to listen for Loki logs requests. Defaults to /loki/api/v1/push, which will (in this example) expand as: 'http://<yourâupstreamâURL>:<yourâport>/loki/api/v1/push'.
    keepAliveTimeout: # [number] Keep alive timeout (seconds) - Maximum time to wait for additional data, after the last response was sent, before closing a socket connection. This can be very useful when Grafana Agent remote write's request frequency is high so, reusing connections, would help mitigating the cost of creating a new connection per request. Note that Grafana Agent's embedded Prometheus would attempt to keep connections open for up to 5 minutes.
    prometheusAuth: # [object] 
      authType: # [string] Authentication type - Remote Write authentication type

      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for Basic authentication
      password: # [string] Password - Password for Basic authentication

      # --------------------------------------------------------


      # -------------- if authType is token ---------------

      token: # [string] Token - Bearer token to include in the authorization header

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

      # --------------------------------------------------------

    lokiAuth: # [object] 
      authType: # [string] Authentication type - Loki logs authentication type

      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for Basic authentication
      password: # [string] Password - Password for Basic authentication

      # --------------------------------------------------------


      # -------------- if authType is token ---------------

      token: # [string] Token - Bearer token to include in the authorization header

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  loki_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    lokiAPI: # [string] [required] Logs API endpoint - Absolute path on which to listen for Loki logs requests. Defaults to /loki/api/v1/push, which will (in this example) expand as: 'http://<yourâupstreamâURL>:<yourâport>/loki/api/v1/push'.
    authType: # [string] Authentication type - Loki logs authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  prometheus_rw_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    prometheusAPI: # [string] [required] Remote Write API endpoint - Absolute path on which to listen for Prometheus requests. Defaults to /write, which will expand as: http://<yourâupstreamâURL>:<yourâport>/write.
    keepAliveTimeout: # [number] Keep alive timeout (seconds) - Maximum time to wait for additional data, after the last response was sent, before closing a socket connection. This can be very useful when Prometheus remote write's request frequency is high so, reusing connections, would help mitigating the cost of creating a new connection per request. Note that Prometheus would attempt to keep connections open for up to 5 minutes.
    authType: # [string] Authentication type - Remote Write authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  prometheus_input: # [object] 
    dimensionList: # [array of strings] Extra Dimensions - Other dimensions to include in events
    discoveryType: # [string] Discovery Type - Target discovery mechanism. Use static to manually enter a list of targets.

    # -------------- if discoveryType is static ---------------

    targetList: # [array of strings] Targets - List of Prometheus targets to pull metrics from. Values can be in URL or host[:port] format. For example: http://localhost:9090/metrics, localhost:9090, or localhost. In cases where just host[:port] is specified, the endpoint will resolve to 'http://host[:port]/metrics'.

    # --------------------------------------------------------


    # -------------- if discoveryType is dns ---------------

    nameList: # [array] DNS Names - List of DNS names to resolve
    recordType: # [string] Record Type - DNS Record type to resolve
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets

    # --------------------------------------------------------


    # -------------- if discoveryType is ec2 ---------------

    usePublicIp: # [boolean] Use Public IP - Use public IP address for discovered targets. Set to false if the private IP address should be used.
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePort: # [number] Metrics Port - The port number in the metrics URL for discovered targets.
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets
    searchFilter: # [array] Search Filter - EC2 Instance Search Filter
      - Name: # [string] Filter Name - Search filter attribute name, see: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html for more information. Attributes can be manually entered if not present in the drop down list
        Values: # [array of strings] Filter Values - Search Filter Values, if empty only "running" EC2 instances will be returned
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.
    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] Region - Region where the EC2 is located
    endpoint: # [string] Endpoint - EC2 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to EC2-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing EC2 requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    enableAssumeRole: # [boolean] Enable for EC2 - Use Assume Role credentials to access EC2
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role

    # --------------------------------------------------------

    interval: # [number] Poll Interval - How often in minutes to scrape targets for metrics, 60 must be evenly divisible by the value or save will fail.
    logLevel: # [string] [required] Log Level - Collector runtime Log Level
    keepAliveTime: # [number] Keep Alive Time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number] Worker Timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authType: # [string] Authentication method - Enter credentials directly, or select a stored secret
    username: # [string] Username - Username for Prometheus Basic authentication
    password: # [string] Password - Password for Prometheus Basic authentication

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  office365_mgmt_input: # [object] 
    type: # [string] Input Type
    tenantId: # [string] Tenant ID - Office 365 Azure Tenant ID
    appId: # [string] [required] App ID - Office 365 Azure Application ID
    timeout: # [number] Timeout (secs) - HTTP request inactivity timeout, use 0 to disable
    keepAliveTime: # [number] Keep Alive Time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number] Worker Timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    planType: # [string] [required] Subscription Plan - Office 365 subscription plan for your organization, typically Enterprise
    publisherIdentifier: # [string] Publisher Identifier - Optional Publisher Identifier to use in API requests, defaults to tenant id if not defined. For more information see [here](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-reference#start-a-subscription)
    contentConfig: # [array] Content Types - Enable Office 365 Management Activity API content types and polling intervals. Polling intervals are used to set up search date range and cron schedule, e.g.: */${interval} * * * *. Because of this, intervals entered must be evenly divisible by 60 to give a predictable schedule.
      - contentType: # [string] Content Type - Office 365 Management Activity API Content Type
        description: # [string] Interval Description - If interval type is minutes the value entered must evenly divisible by 60 or save will fail
        interval: # [number] Interval
        logLevel: # [string] Log Level - Collector runtime Log Level
        enabled: # [boolean] Enabled
    authType: # [string] Authentication method - Enter client secret directly, or select a stored secret
    clientSecret: # [string] Client secret - Office 365 Azure client secret

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Client secret (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  office365_service_input: # [object] 
    type: # [string] Input Type
    tenantId: # [string] Tenant ID - Office 365 Azure Tenant ID
    appId: # [string] [required] App ID - Office 365 Azure Application ID
    timeout: # [number] Timeout (secs) - HTTP request inactivity timeout, use 0 to disable
    keepAliveTime: # [number] Keep Alive Time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number] Worker Timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    contentConfig: # [array] Content Types - Enable Office 365 Service Communication API content types and polling intervals. Polling intervals are used to set up search date range and cron schedule, e.g.: */${interval} * * * *. Because of this, intervals entered for current and historical status must be evenly divisible by 60 to give a predictable schedule.
      - contentType: # [string] Content Type - Office 365 Services API Content Type
        description: # [string] Interval Description - If interval type is minutes the value entered must evenly divisible by 60 or save will fail
        interval: # [number] Interval
        logLevel: # [string] Log Level - Collector runtime Log Level
        enabled: # [boolean] Enabled
    authType: # [string] Authentication method - Enter client secret directly, or select a stored secret
    clientSecret: # [string] Client secret - Office 365 Azure client secret

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Client secret (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  office365_msg_trace_input: # [object] 
    url: # [string] Report URL - URL to use when retrieving report data.
    interval: # [number] [required] Poll interval - How often (in minutes) to run the report. Must divide evenly into 60 minutes to create a predictable schedule, or Save will fail.
    startDate: # [string] Date range start - Backward offset for the search range's head. (E.g.: -3h@h) Message Trace data is delayed; this parameter (with Date range end) compensates for delay and gaps.
    endDate: # [string] Date range end - Backward offset for the search range's tail. (E.g.: -2h@h) Message Trace data is delayed; this parameter (with Date range start) compensates for delay and gaps.
    logLevel: # [string] Log level - Log Level (verbosity) for collection runtime behavior.
    timeout: # [number] Timeout (secs) - HTTP request inactivity timeout. Maximum is 2400 (40 minutes); enter 0 to wait indefinitely.
    disableTimeFilter: # [boolean] Disable time filter - Disables time filtering of events when a date range is specified.
    authType: # [string] Authentication method - Select authentication method.

    # -------------- if authType is manual ---------------

    username: # [string] Username - Username to run Message Trace API call.
    password: # [string] Password - Password to run Message Trace API call.

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials.

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    clientSecret: # [string] Client secret - client_secret to pass in the OAuth request parameter.
    tenantId: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory.
    clientId: # [string] Client ID - client_id to pass in the OAuth request parameter.
    resource: # [string] Resource - Resource to pass in the OAuth request parameter.

    # --------------------------------------------------------


    # -------------- if authType is oauthSecret ---------------

    textSecret: # [string] Client secret - Select (or create) a secret that references your client_secret to pass in the OAuth request parameter.
    tenantId: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory.
    clientId: # [string] Client ID - client_id to pass in the OAuth request parameter.
    resource: # [string] Resource - Resource to pass in the OAuth request parameter.

    # --------------------------------------------------------

    keepAliveTime: # [number] Keep Alive Time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number] Worker Timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  eventhub_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Brokers - List of Event Hubs Kafka brokers to connect to, e.g., yourdomain.servicebus.windows.net:9093. The hostname can be found in the host portion of the primary or secondary connection string in Shared Access Policies.
    topics: # [array of strings] [required] Event Hub name - The name of the Event Hub (a.k.a. Kafka topic) to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Event Hubs Source to only a single topic.
    groupId: # [string] Group ID - Specifies the consumer group this instance belongs to, default is 'Cribl'.
    fromBeginning: # [boolean] From beginning - Whether to start reading from earliest available data, relevant only during initial subscription.
    connectionTimeout: # [number] Connection timeout (ms) - Maximum time to wait for a successful connection.
    requestTimeout: # [number] Request timeout (ms) - Maximum time to wait for a successful request.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled - Enable authentication.

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism - SASL authentication mechanism to use. PLAIN is the only mechanism currently supported for Event Hubs Kafka brokers.
      username: # [string] Username - The username for authentication. For Event Hubs, this should always be $ConnectionString.
      authType: # [string] Authentication method - Enter password directly, or select a stored secret

      # --------------------------------------------------------

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - For Event Hubs, this should always be false.

      # --------------------------------------------------------

    sessionTimeout: # [number] Session timeout (ms) - 
      Timeout (a.k.a session.timeout.ms in Kafka domain) used to detect client failures when using Kafka's group management facilities.
      If the client sends the broker no heartbeats before this timeout expires, the broker will remove this client from the group, and will initiate a rebalance.
      Value must be lower than rebalanceTimeout.
      See details [here](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    rebalanceTimeout: # [number] Rebalance timeout (ms) - 
      Maximum allowed time (a.k.a rebalance.timeout.ms in Kafka domain) for each worker to join the group after a rebalance has begun.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See details [here](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    heartbeatInterval: # [number] Heartbeat interval (ms) - 
      Expected time (a.k.a heartbeat.interval.ms in Kafka domain) between heartbeats to the consumer coordinator when using Kafka's group management facilities.
      Value must be lower than sessionTimeout, and typically should not exceed 1/3 of the sessionTimeout value.
      See details [here](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    minimizeDuplicates: # [boolean] Minimize duplicates - Enable feature to minimize duplicate events by only starting one consumer for each topic partition.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  exec_input: # [object] 
    disabled: # [boolean] Disabled - Enable/disable this input
    command: # [string] Command - Command to execute; supports Bourne shell syntax
    retries: # [number] Max retries - Maximum number of retry attempts in the event that the command fails.
    scheduleType: # [string] Schedule type - Select a schedule type; either an interval (in seconds) or a cron-style schedule.

    # -------------- if scheduleType is interval ---------------

    interval: # [number] Interval - Interval between command executions in seconds.

    # --------------------------------------------------------


    # -------------- if scheduleType is cronSchedule ---------------

    cronSchedule: # [string] Schedule - Cron schedule to execute the command on.

    # --------------------------------------------------------

    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    type: # [string] Input Type
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  firehose_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthed access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  google_pubsub_input: # [object] 
    type: # [string] Input Type
    topicName: # [string] Topic ID - ID of the topic to receive events from.
    subscriptionName: # [string] [required] Subscription ID - ID of the subscription to use when receiving events.
    createTopic: # [boolean] Create topic - If enabled, create topic if it does not exist
    createSubscription: # [boolean] Create subscription - If enabled, create subscription if it does not exist

    # -------------- if createSubscription is true ---------------

    orderedDelivery: # [boolean] Ordered delivery - If enabled, receive events in the order they were added to the queue. For this to work correctly, the process sending events must have ordering enabled.

    # --------------------------------------------------------

    region: # [string] Region - Region to retrieve messages from. Select 'default' to allow Google to auto-select the nearest region. When using ordered delivery, the selected region must be allowed by message storage policy.
    googleAuthMethod: # [string] Authentication Method - Google authentication method. Choose Auto to use environment variables PUBSUB_PROJECT and PUBSUB_CREDENTIALS.

    # -------------- if googleAuthMethod is manual ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right. As an alternative, you can use environment variables (see [here](https://googleapis.dev/ruby/google-cloud-pubsub/latest/file.AUTHENTICATION.html)).

    # --------------------------------------------------------


    # -------------- if googleAuthMethod is secret ---------------

    secret: # [string] Service account credentials (text secret) - Select (or create) a stored text secret

    # --------------------------------------------------------

    maxBacklog: # [number] Max backlog - If Destination exerts backpressure, this setting limits how many inbound events Stream will queue for processing before it stops retrieving events.
    requestTimeout: # [number] Request timeout (ms) - Pull request timeout, in milliseconds.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cribl_input: # [object] 
    type: # [string] Input Type
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cribl_tcp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cribl_http_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthed access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  tcpjson_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to establish a connection.
    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthed access is permitted.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  system_metrics_input: # [object] 
    interval: # [number] Polling interval - Time, in seconds, between consecutive metric collections. 
    host: # [object] 
      mode: # [string]  - Select level of detail for host metrics

      # -------------- if mode is custom ---------------

      custom: # [object] 
        system: # [object] 
          mode: # [string]  - Select the level of details for system metrics

          # -------------- if mode is custom ---------------

          processes: # [boolean] Process metrics - Generate metrics for the numbers of processes in various states

          # --------------------------------------------------------

        cpu: # [object] 
          mode: # [string]  - Select the level of details for CPU metrics

          # -------------- if mode is custom ---------------

          perCpu: # [boolean] Per CPU metrics - Generate metrics for each CPU
          detail: # [boolean] Detailed metrics - Generate metrics for all CPU states
          time: # [boolean] CPU time metrics - Generate raw, monotonic CPU time counters

          # --------------------------------------------------------

        memory: # [object] 
          mode: # [string]  - Select the level of details for memory metrics

          # -------------- if mode is custom ---------------

          detail: # [boolean] Detailed metrics - Generate metrics for all memory states

          # --------------------------------------------------------

        network: # [object] 
          mode: # [string]  - Select the level of details for network metrics

          # -------------- if mode is custom ---------------

          devices: # [array of strings] Interface filter - Network interfaces to include/exclude. E.g.: eth0, !lo, etc. All interfaces are included if this list is empty.
          perInterface: # [boolean] Per interface metrics - Generate separate metrics for each interface
          detail: # [boolean] Detailed metrics - Generate full network metrics

          # --------------------------------------------------------

        disk: # [object] 
          mode: # [string]  - Select the level of details for disk metrics

          # -------------- if mode is custom ---------------

          devices: # [array of strings] Device filter - Block devices to include/exclude. E.g.: sda*, !loop*, etc. Wildcards and ! (not) operators are supported. All devices are included if this list is empty.
          mountpoints: # [array of strings] Mountpoint filter - Filesystem mountpoints to include/exclude. E.g.: /, /home, !/proc*, !/tmp, etc. Wildcards and ! (not) operators are supported. All mountpoints are included if this list is empty.
          fstypes: # [array of strings] Filesystem type filter - Filesystem types to include/exclude. E.g.: ext4, !*tmpfs, !squashfs, etc. Wildcards and ! (not) operators are supported. All types are included if this list is empty.
          perDevice: # [boolean] Per device metrics - Generate separate metrics for each device
          detail: # [boolean] Detailed metrics - Generate full disk metrics

          # --------------------------------------------------------


      # --------------------------------------------------------

    container: # [object] 
      mode: # [string]  - Select the level of detail for container metrics

      # -------------- if mode is basic ---------------

      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------


      # -------------- if mode is all ---------------

      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------


      # -------------- if mode is custom ---------------

      filters: # [array] Container Filters - Containers matching any of these will be included. All are included if this is empty.
        - expr: # [string] Expression
      allContainers: # [boolean] All containers - Include stopped and paused containers
      perDevice: # [boolean] Per device metrics - Generate separate metrics for each device
      detail: # [boolean] Detailed metrics - Generate full container metrics
      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] persistence
      enable: # [boolean] Enable disk persistence - Persist metrics on disk

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Max data size - Maximum disk space allowed to be consumed (e.g., 420MB or 4GB). Once reached, older data will be deleted.
      maxDataTime: # [string] Max data age - Maximum amount of time to retain data (e.g., 2h or 4d). Once reached, older data will be deleted.
      compress: # [string] Compression - Select data compression format. Optional.
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>`

      # --------------------------------------------------------

    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  system_state_input: # [object] 
    interval: # [number] Polling interval - Time, in seconds, between consecutive state collections. 
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    collectors: # [object] 
      hostsfile: # [object] 
        enable: # [boolean] Enabled
    persistence: # [object] 
      enable: # [boolean] Enable disk persistence - Persist metrics on disk

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Max data size - Maximum disk space allowed to be consumed (e.g., 420MB or 4GB). Once reached, older data will be deleted.
      maxDataTime: # [string] Max data age - Maximum amount of time to retain data (e.g., 2h or 4d). Once reached, older data will be deleted.
      compress: # [string] Compression - Select data compression format. Optional.
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>`

      # --------------------------------------------------------

    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  windows_metrics_input: # [object] 
    interval: # [number] Polling interval - Time, in seconds, between consecutive metric collections. 
    host: # [object] 
      mode: # [string]  - Select level of detail for host metrics

      # -------------- if mode is custom ---------------

      custom: # [object] 
        system: # [object] 
          mode: # [string]  - Select the level of details for system metrics

          # -------------- if mode is custom ---------------

          detail: # [boolean] Detailed metrics - Generate metrics for all system information

          # --------------------------------------------------------

        cpu: # [object] 
          mode: # [string]  - Select the level of details for CPU metrics

          # -------------- if mode is custom ---------------

          perCpu: # [boolean] Per CPU metrics - Generate metrics for each CPU
          detail: # [boolean] Detailed metrics - Generate metrics for all CPU states
          time: # [boolean] CPU time metrics - Generate raw, monotonic CPU time counters

          # --------------------------------------------------------

        memory: # [object] 
          mode: # [string]  - Select the level of details for memory metrics

          # -------------- if mode is custom ---------------

          detail: # [boolean] Detailed metrics - Generate metrics for all memory states

          # --------------------------------------------------------

        network: # [object] 
          mode: # [string]  - Select the level of details for network metrics

          # -------------- if mode is custom ---------------

          devices: # [array of strings] Interface filter - Network interfaces to include/exclude. All interfaces are included if this list is empty.
          perInterface: # [boolean] Per interface metrics - Generate separate metrics for each interface
          detail: # [boolean] Detailed metrics - Generate full network metrics

          # --------------------------------------------------------

        disk: # [object] 
          mode: # [string]  - Select the level of details for disk metrics

          # -------------- if mode is custom ---------------

          volumes: # [array of strings] Volume filter - Windows volumes to include/exclude. E.g.: C:, !E:, etc. Wildcards and ! (not) operators are supported. All volumes are included if this list is empty.
          perVolume: # [boolean] Per volume metrics - Generate separate metrics for each volume

          # --------------------------------------------------------


      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] persistence
      enable: # [boolean] Enable disk persistence - Persist metrics on disk

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Max data size - Maximum disk space allowed to be consumed (e.g., 420MB or 4GB). Once reached, older data will be deleted.
      maxDataTime: # [string] Max data age - Maximum amount of time to retain data (e.g., 2h or 4d). Once reached, older data will be deleted.
      compress: # [string] Compression - Select data compression format. Optional.
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>`

      # --------------------------------------------------------

    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  crowdstrike_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. E.g., 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS Account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select (or create) a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    maxMessages: # [number] Max Messages - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number] Visibility timeout seconds - The duration (in seconds) that received messages are hidden from subsequent retrieve requests, after being retrieved by a ReceiveMessage request. This value also automatically extends this duration, to prevent it from expiring before processing completes.
    numReceivers: # [number] Num receivers - The Number of receiver processes to run, the higher the number the better throughput at the expense of CPU overhead
    socketTimeout: # [number] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Toggle to Yes to skip files that trigger a processing error. Defaults to No, which enables retries after processing errors.
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    enableSQSAssumeRole: # [boolean] Enable for SQS - Use Assume Role credentials when accessing SQS.
    preprocess: # [object] 
      disabled: # [boolean] Disabled - Enable Custom Command
      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pollTimeout: # [number] Poll timeout (secs) - The amount of time to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  datadog_agent_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    extractMetrics: # [boolean] Extract metrics - Toggle to Yes to extract each incoming metric to multiple events, one per data point. This works well when sending metrics to a statsd-type output. If sending metrics to DatadogHQ or any destination that accepts arbitrary JSON, leave toggled to No (the default).
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    proxyMode: # [object] 
      enabled: # [boolean] Forward API key validation requests - Toggle to Yes to send key validation requests from Datadog Agent to the Datadog API. If toggled to No (the default), Stream handles key validation requests by always responding that the key is valid.
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  datagen_input: # [object] 
    samples: # [array] Datagen - List of datagens
      - sample: # [string] Data Generator File - Name of the datagen file
        eventsPerSec: # [number] Events Per Second Per Worker Node - Maximum no. of events to generate per second per worker node. Defaults to 10.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  http_raw_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthed access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    allowedPaths: # [array of strings] Allowed URI paths - List of URI paths accepted by this input, wildcards are supported, e.g /api/v*/hook. Defaults to allow all.
    allowedMethods: # [array of strings] Allowed HTTP methods - List of HTTP methods accepted by this input, wildcards are supported, e.g. P*, GET. Defaults to allow all.
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  kinesis_input: # [object] 
    type: # [string] Input Type
    streamName: # [string] Stream name - Kinesis stream name to read data from.
    serviceInterval: # [number] Service Period - Time interval in minutes between consecutive service calls
    shardExpr: # [string] Shard selection expression - A JS expression to be called with each shardId for the stream, if the expression evalutates to a truthy value the shard will be processed.
    shardIteratorType: # [string] Shard iterator start - Location at which to start reading a shard for the first time.
    payloadFormat: # [string] Record data format - Format of data inside the Kinesis Stream records. Gzip compression is automatically detected.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select (or create) a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] [required] Region - Region where the Kinesis stream is located
    endpoint: # [string] Endpoint - Kinesis stream service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to Kinesis stream-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing Kinesis stream requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    enableAssumeRole: # [boolean] Enable for Kinesis stream - Use Assume Role credentials to access Kinesis stream
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    verifyKPLCheckSums: # [boolean] Verify KPL checksums - Verify Kinesis Producer Library (KPL) event checksums
    avoidDuplicates: # [boolean] Avoid duplicate records - Yes means: when resuming streaming from a stored state, Stream will read the next available record, rather than rereading the last-read record. Enabling this can cause data loss after a Worker Node's unexpected shutdown or restart.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  logstream_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to establish a connection.
    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthed access is permitted.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  criblmetrics_input: # [object] 
    type: # [string] Input Type
    prefix: # [string] Metric Name Prefix - A prefix that is applied to the metrics provided by Cribl Stream
    fullFidelity: # [boolean] Full Fidelity - Include granular metrics.  Disabling this will drop the following metrics events: `cribl.logstream.host.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.index.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.source.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.sourcetype.(in_bytes,in_events,out_bytes,out_events)`.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  metrics_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    udpPort: # [number] UDP Port - Enter UDP port number to listen on. Not required if listening on TCP.
    tcpPort: # [number] TCP Port - Enter TCP port number to listen on. Not required if listening on UDP.
    maxBufferSize: # [number] Max Buffer Size (events) - Maximum number of events to buffer when downstream is blocking. Only applies to UDP.
    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to send data
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  s3_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. E.g., 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS Account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select (or create) a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    maxMessages: # [number] Max Messages - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number] Visibility timeout seconds - The duration (in seconds) that received messages are hidden from subsequent retrieve requests, after being retrieved by a ReceiveMessage request. This value also automatically extends this duration, to prevent it from expiring before processing completes.
    numReceivers: # [number] Num receivers - The Number of receiver processes to run, the higher the number the better throughput at the expense of CPU overhead
    socketTimeout: # [number] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Toggle to Yes to skip files that trigger a processing error. Defaults to No, which enables retries after processing errors.
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    enableSQSAssumeRole: # [boolean] Enable for SQS - Use Assume Role credentials when accessing SQS.
    preprocess: # [object] 
      disabled: # [boolean] Disabled - Enable Custom Command
      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    parquetChunkSizeMB: # [number] Max Parquet chunk size (MB) - Maximum file size for each Parquet chunk.
    parquetChunkDownloadTimeout: # [number] Parquet chunk download timeout (seconds) - The maximum time to wait for a Parquet file's chunk to be downloaded. Processing will end if a required chunk could not be downloaded within the time imposed by this setting.
    pollTimeout: # [number] Poll timeout (secs) - The amount of time to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  snmp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    port: # [number] [required] UDP Port - UDP port to receive SNMP traps on. Defaults to 162.
    maxBufferSize: # [number] Max Buffer Size (events) - Maximum number of events to buffer when downstream is blocking.
    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to send data
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  open_telemetry_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.

      # --------------------------------------------------------

    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    activityLogSampleRate: # [number] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    extractSpans: # [boolean] Extract spans - Toggle to Yes to extract each incoming span to a separate event.
    extractMetrics: # [boolean] Extract metrics - Toggle to Yes to extract each incoming Gauge or IntGauge metric to multiple events, one per data point.
    maxActiveCxn: # [number] Max active connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    authType: # [string] Authentication type - OpenTelemetry authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

    # --------------------------------------------------------


    # -------------- if authType is token ---------------

    token: # [string] Token - Bearer token to include in the authorization header

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select (or create) a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Token (text secret) - Select (or create) a stored text secret

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  sqs_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read events from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. E.g., 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can only be evaluated at init time. E.g. referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    queueType: # [string] [required] Queue Type - The queue type used (or created). Defaults to Standard

    # -------------- if queueType is standard ---------------

    numReceivers: # [number] Num receivers - The Number of receiver processes to run, the higher the number the better throughput at the expense of CPU overhead

    # --------------------------------------------------------

    awsAccountId: # [string] AWS Account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    createQueue: # [boolean] Create Queue - Create queue if it does not exist.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select (or create) a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] Region - AWS Region where the SQS queue is located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - SQS service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to SQS-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing SQS requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    enableAssumeRole: # [boolean] Enable for SQS - Use Assume Role credentials to access SQS
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    maxMessages: # [number] Max Messages - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number] Visibility Timeout Seconds - The duration (in seconds) that the received messages are hidden from subsequent retrieve requests after being retrieved by a ReceiveMessage request.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pollTimeout: # [number] Poll timeout (secs) - The amount of time to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  syslog_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    udpPort: # [number] UDP port - Enter UDP port number to listen on. Not required if listening on TCP.
    tcpPort: # [number] TCP port - Enter TCP port number to listen on. Not required if listening on UDP.
    maxBufferSize: # [number] Max buffer size (events) - Maximum number of events to buffer when downstream is blocking. Only applies to UDP.
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to send data
    timestampTimezone: # [string] Default timezone - Timezone to assign to timestamps without timezone info.
    singleMsgUdpPackets: # [boolean] Single msg per UDP - Whether to treat UDP packet data received as full syslog message
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2
    keepFieldsList: # [array of strings] Fields to keep - Wildcard list of fields to keep from source data, * = ALL (default)
    octetCounting: # [boolean] Octet count framing - Enable if incoming messages use octet counting per RFC 6587.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  file_input: # [object] 
    mode: # [string]  - Choose how to discover files to monitor.

    # -------------- if mode is manual ---------------

    path: # [string] Search path - Directory path to search for files. Environment variables will be resolved, e.g. $CRIBL_HOME/log/.
    depth: # [number] Max depth - Set how many subdirectories deep to search. Use 0 to search only files in the given path, 1 to also look in its immediate subdirectories, etc. Leave it empty for unlimited depth.

    # --------------------------------------------------------

    interval: # [number] Polling interval - Time, in seconds, between scanning for files. 
    filenames: # [array of strings] Filename allowlist - The full path of discovered files are matched against this wildcard list.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  tcp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to establish a connection.
    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    enableHeader: # [boolean] Enable Header - If enabled, client will pass the header record with every new connection. The header can contain an authToken, and an object with a list of fields and values to add to every event. These fields can be used to simplify Event Breaker selection, routing, etc. Header has this format, and must be followed by a newline: { "authToken" : "myToken", "fields": { "field1": "value1", "field2": "value2" } }

    # -------------- if enableHeader is true ---------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # --------------------------------------------------------

    preprocess: # [object] 
      disabled: # [boolean] Disabled - Enable Custom Command
      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  appscope_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    ipWhitelistRegex: # [string] IP Allowlist Regex - Regex matching IP addresses that are allowed to establish a connection.
    maxActiveCxn: # [number] Max Active Connections - Maximum number of active connections allowed per Worker Process, use 0 for unlimited
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    enableUnixPath: # [boolean] UNIX domain socket - Toggle to Yes to specify a file-backed UNIX domain socket connection, instead of a network host and port.

    # -------------- if enableUnixPath is false ---------------

    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] Port - Port to listen to.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate name - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Whether to require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections.

      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if enableUnixPath is true ---------------

    unixSocketPath: # [string] UNIX socket path - Path to the UNIX domain socket to listen on.
    unixSocketPerms: # [string,number] UNIX socket permissions - Permissions to set for socket e.g., 777. If empty, falls back to the runtime user's default permissions.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthed access is permitted.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select (or create) a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  wef_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled - Enable/disable this input
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number] [required] Port - Port to listen to.
    tls: # [object] [required] mTLS settings
      disabled: # [boolean] Disabled - Enable TLS
      rejectUnauthorized: # [boolean] Validate client certs - Required for WEF certificate authentication.
      requestCert: # [boolean] Authenticate client - Required for WEF certificate authentication.
      certificateName: # [string] Certificate name - Name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      certPath: # [string] [required] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] [required] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      commonNameRegex: # [string] Common name - Regex matching allowable common names in peer certificates' subject attribute.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to accept from connections.
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to accept from connections
    maxActiveReq: # [number] Max active requests - Maximum number of active requests per Worker Process. Use 0 for unlimited.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2.
    captureHeaders: # [boolean] Capture request headers - Toggle this to Yes to add request headers to events, in the __headers field.
    caFingerprint: # [string] CA fingerprint override - SHA1 fingerprint expected by the client, if it does not match the first certificate in the configured CA chain
    allowMachineIdMismatch: # [boolean] Allow MachineID mismatch - Allow events to be ingested even if their MachineID does not match the client certificate CN.
    subscriptions: # [array] [required] Subscriptions - Subscriptions to events on forwarding endpoints.
      - subscriptionName: # [string] Name - Friendly name for this subscription.
        version: # [string] Version - Version UUID for this subscription. If any subscription parameters are modified, this value will change.
        contentFormat: # [string] Format - Content format in which the endpoint should deliver events.
        heartbeatInterval: # [number] Heartbeat - Max time (in seconds) between endpoint checkins before considering it unavailable.
        batchTimeout: # [number] Batch timeout - Interval (in seconds) over which the endpoint should collect events before sending them to Stream.
        readExistingEvents: # [boolean] Read existing events - Set to Yes if a newly-subscribed endpoint should send previously existing events. Set to No to only receive new events
        sendBookmarks: # [boolean] Use bookmarks - If toggled to Yes, Cribl Edge will keep track of which events have been received, resuming from that point after a re-subscription. This setting takes precedence over 'Read existing events' -- see the documentation for details.
        compress: # [boolean] Compression - If toggled to Yes, Stream will receive compressed events from the source.
        targets: # [array of strings] Targets - Enter the DNS names of the endpoints that should forward these events. You may use wildcards, for example: *.mydomain.com
        querySelector: # [string] Query builder mode - Select the query builder mode.
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  win_event_logs_input: # [object] 
    type: # [string] Input Type
    logNames: # [array of strings] Event Logs - Enter the event logs to collect. Run "Get-WinEvent -ListLog *" in PowerShell to see the available logs.
    readMode: # [string] Read Mode - Read all stored and future event logs, or only future events.
    interval: # [number] Polling Interval - Time, in seconds, between checking for new entries.
    batchSize: # [number] Batch Size - Maximum number of event records to read in one interval.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled - Enable/disable this input
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes.
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Quick Connections - Direct connections to Destinations, optionally via a Pipeline or a Pack.
      - pipeline: # [string] Pipeline/Pack - Select Pipeline or Pack. Optional.
        output: # [string] Destination - Select a Destination.

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable Persistent Queue

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. WithÂ AlwaysÂ On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number] Max buffer size - The maximum amount of events to hold in-memory before dumping the events to disk.
      commitFrequency: # [number] Commit frequency - The number of events to send downstream before committing that Stream has read them.
      maxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
      maxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>.
      compress: # [string] Compression - Codec to use to compress the persisted data.

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
```


