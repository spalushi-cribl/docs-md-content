# inputs.yml


`inputs.yml` contains settings for configuring inputs into Cribl.

```yaml {title="inputs.yml"}
inputs: # [object] 
  collection_input: # [object] 
    type: # [string] Input Type
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Accepts values with multiple-byte units, such as KB, MB, and GB. (Example: 42 MB) Default value of 0 specifies no throttling.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  kafka_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Bootstrap servers - Enter each Kafka bootstrap server you want to use. Specify the hostname and port (such as mykafkabroker:9092) or just the hostname (in which case Cribl Stream will assign port 9092).
    topics: # [array of strings] [required] Topic - Topic to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Kafka Source to a single topic only.
    groupId: # [string] Group ID - The consumer group to which this instance belongs. Defaults to 'Cribl'.
    fromBeginning: # [boolean] From beginning - Leave enabled if you want the Source, upon first subscribing to a topic, to read starting with the earliest available message
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


      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Stream can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
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

    sessionTimeout: # [number; minimum: 1000, maximum: 3600000] Session timeout (ms) - 
      Timeout used to detect client failures when using Kafka's group-management facilities.
      If the client sends no heartbeats to the broker before the timeout expires, 
      the broker will remove the client from the group and initiate a rebalance.
      Value must be between the broker's configured group.min.session.timeout.ms and group.max.session.timeout.ms.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms) for details.
    rebalanceTimeout: # [number; minimum: 1000, maximum: 3600000] Rebalance timeout (ms) - 
      Maximum allowed time for each worker to join the group after a rebalance begins.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms) for details.
    heartbeatInterval: # [number; minimum: 1000, maximum: 3600000] Heartbeat interval (ms) - 
      Expected time between heartbeats to the consumer coordinator when using Kafka's group-management facilities.
      Value must be lower than sessionTimeout and typically should not exceed 1/3 of the sessionTimeout value.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms) for details.
    autoCommitInterval: # [number; minimum: 1000, maximum: 3600000] Offset commit interval (ms) - How often to commit offsets. If both this and Offset commit threshold are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    autoCommitThreshold: # [number; minimum: 1, maximum: 10000] Offset commit threshold - How many events are needed to trigger an offset commit. If both this and Offset commit interval are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    maxBytesPerPartition: # [number; minimum: 1, maximum: 10000000] Byte limit, per partition - Maximum amount of data that Kafka will return per partition, per fetch request. Must equal or exceed the maximum message size (maxBytesPerPartition) that Kafka is configured to allow. Otherwise, Cribl Stream can get stuck trying to retrieve messages. Defaults to 1048576 (1 MB).
    maxBytes: # [number; minimum: 1, maximum: 1000000000] Byte limit - Maximum number of bytes that Kafka will return per fetch request. Defaults to 10485760 (10 MB).
    maxSocketErrors: # [number; maximum: 100] Error limit, per socket - Maximum number of network errors before the consumer re-creates a socket
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  msk_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Bootstrap servers - Enter each Kafka bootstrap server you want to use. Specify the hostname and port (such as mykafkabroker:9092) or just the hostname (in which case Cribl Stream will assign port 9092).
    topics: # [array of strings] [required] Topic - Topic to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Kafka Source to a single topic only.
    groupId: # [string] Group ID - The consumer group to which this instance belongs. Defaults to 'Cribl'.
    fromBeginning: # [boolean] From beginning - Leave enabled if you want the Source, upon first subscribing to a topic, to read starting with the earliest available message
    sessionTimeout: # [number; minimum: 1000, maximum: 3600000] Session timeout (ms) - 
      Timeout used to detect client failures when using Kafka's group-management facilities.
      If the client sends no heartbeats to the broker before the timeout expires, 
      the broker will remove the client from the group and initiate a rebalance.
      Value must be between the broker's configured group.min.session.timeout.ms and group.max.session.timeout.ms.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms) for details.
    rebalanceTimeout: # [number; minimum: 1000, maximum: 3600000] Rebalance timeout (ms) - 
      Maximum allowed time for each worker to join the group after a rebalance begins.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms) for details.
    heartbeatInterval: # [number; minimum: 1000, maximum: 3600000] Heartbeat interval (ms) - 
      Expected time between heartbeats to the consumer coordinator when using Kafka's group-management facilities.
      Value must be lower than sessionTimeout and typically should not exceed 1/3 of the sessionTimeout value.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms) for details.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
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


      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Stream can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
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

    autoCommitInterval: # [number; minimum: 1000, maximum: 3600000] Offset commit interval (ms) - How often to commit offsets. If both this and Offset commit threshold are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    autoCommitThreshold: # [number; minimum: 1, maximum: 10000] Offset commit threshold - How many events are needed to trigger an offset commit. If both this and Offset commit interval are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    maxBytesPerPartition: # [number; minimum: 1, maximum: 10000000] Byte limit, per partition - Maximum amount of data that Kafka will return per partition, per fetch request. Must equal or exceed the maximum message size (maxBytesPerPartition) that Kafka is configured to allow. Otherwise, Cribl Stream can get stuck trying to retrieve messages. Defaults to 1048576 (1 MB).
    maxBytes: # [number; minimum: 1, maximum: 1000000000] Byte limit - Maximum number of bytes that Kafka will return per fetch request. Defaults to 10485760 (10 MB).
    maxSocketErrors: # [number; maximum: 100] Error limit, per socket - Maximum number of network errors before the consumer re-creates a socket
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  http_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    criblAPI: # [string] Cribl HTTP event API - Absolute path on which to listen for the Cribl HTTP API requests. Only _bulk (default /cribl/_bulk) is available. Use empty string to disable.
    elasticAPI: # [string] Elasticsearch API endpoint (Bulk API) - Absolute path on which to listen for the Elasticsearch API requests. Only _bulk (default /elastic/_bulk) is available. Use empty string to disable.
    splunkHecAPI: # [string] Splunk HEC endpoint - Absolute path on which listen for the Splunk HTTP Event Collector API requests. Use empty string to disable.
    splunkHecAcks: # [boolean] Enable Splunk HEC acknowledgements
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authTokensExt: # [array] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
      - token: # [string] Token - Shared secret to be provided by any client (Authorization: <token>)
        description: # [string] Description
        metadata: # [array] Fields - Fields to add to events referencing this token
        - name: # [string] Field Name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  splunk_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to establish a connection
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    socketIdleTimeout: # [number] Socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] Forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports proxy protocol v1 or v2
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    authTokens: # [array] Auth tokens - Shared secrets to be provided by any Splunk forwarder. If empty, unauthorized access is permitted.
      - token: # [string] Token - Shared secrets to be provided by any Splunk forwarder. If empty, unauthorized access is permitted.
        description: # [string] Description
    maxS2Sversion: # [string] Max S2S version - The highest S2S protocol version to advertise during handshake

    # -------------- if maxS2Sversion is v4 ---------------

    useFwdTimezone: # [boolean] Use Universal Forwarder time zone - Event Breakers will determine events' time zone from UF-provided metadata, when TZ can't be inferred from the raw event
    dropControlFields: # [boolean] Drop control fields - Drop Splunk control fields such as `crcSalt` and `_savedPort`. If disabled, control fields are stored in the internal field `__ctrlFields`.
    extractMetrics: # [boolean] Extract metrics - Extract and process Splunk-generated metrics as Cribl metrics
    compress: # [string] Compression - Controls whether to support reading compressed data from a forwarder. Select 'Automatic' to match the forwarder's configuration, or 'Disabled' to reject compressed connections.

    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  splunk_search_input: # [object] 
    type: # [string] Input Type
    searchHead: # [string] Search head - Search head base URL. Can be an expression. Default is https://localhost:8089.
    search: # [string] [required] Search - Enter Splunk search here. Examples: 'index=myAppLogs level=error channel=myApp' OR '| mstats avg(myStat) as myStat WHERE index=myStatsIndex.'
    earliest: # [string] Earliest - The earliest time boundary for the search. Can be an exact or relative time. Examples: '2022-01-14T12:00:00Z' or '-16m@m'
    latest: # [string] Latest - The latest time boundary for the search. Can be an exact or relative time. Examples: '2022-01-14T12:00:00Z' or '-1m@m'
    cronSchedule: # [string] [required] Cron schedule - A cron schedule on which to run this job
    endpoint: # [string] [required] Search endpoint - REST API used to create a search
    outputMode: # [string] [required] Output mode - Format of the returned output
    endpointParams: # [array] Endpoint parameters - Optional request parameters to send to the endpoint
      - name: # [string] Parameter Name
        value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.
    endpointHeaders: # [array] Endpoint headers - Optional request headers to send to the endpoint
      - name: # [string] Header Name
        value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.
    logLevel: # [string] Log level - Collector runtime log level (verbosity)
    requestTimeout: # [number; maximum: 2400] Request timeout (seconds) - HTTP request inactivity timeout. Use 0 for no timeout.
    useRoundRobinDns: # [boolean] Round-robin DNS - When a DNS server returns multiple addresses, Cribl Stream will cycle through them in the order returned
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as self-signed certificates)
    encoding: # [string] Encoding - Character encoding to use when parsing ingested data. When not set, Cribl Stream will default to UTF-8 but may incorrectly interpret multi-byte characters.
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    retryRules: # [object] 
      type: # [string] Retry type - The algorithm to use when performing HTTP retries

      # -------------- if type is static ---------------

      codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
      interval: # [number; maximum: 20000] Wait (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------


      # -------------- if type is backoff ---------------

      interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
      codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------

    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    authType: # [string] Authentication type - Splunk Search authentication type

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
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  splunk_hec_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
      - authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
        enabled: # [boolean] Enable token
        description: # [string] Description - Optional token description
        allowedIndexesAtToken: # [array of strings] Allowed indexes - Enter the values you want to allow in the HEC event index field at the token level. Supports wildcards. To skip validation, leave blank.
        metadata: # [array] Fields - Fields to add to events referencing this token
        - name: # [string] Field Name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    splunkHecAPI: # [string] [required] Splunk HEC endpoint - Absolute path on which to listen for the Splunk HTTP Event Collector API requests. This input supports the /event, /raw and /s2s endpoints.
    metadata: # [array] Fields - Fields to add to every event. Overrides fields added at the token or request level. See [the Source documentation](https://docs.cribl.io/stream/sources-splunk-hec/#fields) for more info.
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    allowedIndexes: # [array of strings] Allowed indexes - List values allowed in HEC event index field. Leave blank to skip validation. Supports wildcards. The values here can expand index validation at the token level.
    splunkHecAcks: # [boolean] Splunk HEC acks - Enable Splunk HEC acknowledgements
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    useFwdTimezone: # [boolean] Use Universal Forwarder time zone (S2S only) - Event Breakers will determine events' time zone from UF-provided metadata, when TZ can't be inferred from the raw event
    dropControlFields: # [boolean] Drop control fields (S2S only) - Drop Splunk control fields such as `crcSalt` and `_savedPort`. If disabled, control fields are stored in the internal field `__ctrlFields`.
    extractMetrics: # [boolean] Extract metrics (S2S only) - Extract and process Splunk-generated metrics as Cribl metrics
    accessControlAllowOrigin: # [array of strings] CORS allowed origins - Optionally, list HTTP origins to which Cribl Stream should send CORS (cross-origin resource sharing) Access-Control-Allow-* headers. Supports wildcards.
    accessControlAllowHeaders: # [array of strings] CORS allowed headers - Optionally, list HTTP headers that Cribl Stream will send to allowed origins as "Access-Control-Allow-Headers" in a CORS preflight response. Use "*" to allow all headers.
    emitTokenMetrics: # [boolean] Emit per-token request metrics - Emit per-token (<prefix>.http.perToken) and summary (<prefix>.http.summary) request metrics
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  azure_blob_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The storage account queue name blob notifications will be read from. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at initialization time. Example referencing a Global Variable: `myQueue-${C.vars.myVar}`
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    visibilityTimeout: # [number; maximum: 604800] Visibility timeout (secs) - The duration (in seconds) that the received messages are hidden from subsequent retrieve requests after being retrieved by a ReceiveMessage request.
    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.
    maxMessages: # [number; minimum: 1, maximum: 32] Message limit - The maximum number of messages to return in a poll request. Azure storage queues never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 32.
    servicePeriodSecs: # [number; minimum: 1, maximum: 10] Service period (secs) - The duration (in seconds) which pollers should be validated and restarted if exited
    skipOnError: # [boolean] Skip file on error - Skip files that trigger a processing error. Disabled by default, which allows retries after processing errors.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
    parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will stop if a chunk cannot be downloaded within the time specified.
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

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  elastic_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    elasticAPI: # [string] [required] Elasticsearch API endpoint - Absolute path on which to listen for Elasticsearch API requests. Defaults to /. _bulk will be appended automatically. For example, /myPath becomes /myPath/_bulk. Requests can then be made to either /myPath/_bulk or /myPath/<myIndexName>/_bulk. Other entries are faked as success.
    authType: # [string] Authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username
    password: # [string] Password

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------


    # -------------- if authType is authTokens ---------------

    authTokens: # [array of strings] Token - Bearer tokens to include in the authorization header

    # --------------------------------------------------------

    apiVersion: # [string] API version - The API version to use for communicating with the server

    # -------------- if apiVersion is custom ---------------

    customAPIVersion: # [string] Custom API Version - Custom version information to respond to requests

    # --------------------------------------------------------

    extraHttpHeaders: # [array] Extra HTTP headers - Headers to add to all events
      - name: # [string] Field Name
        value: # [string] Field Value
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    proxyMode: # [object] 
      enabled: # [boolean] Enable proxy mode - Enable proxying of non-bulk API requests to an external Elastic server. Enable this only if you understand the implications. See [Cribl Docs](https://docs.cribl.io/stream/sources-elastic/#proxy-mode) for more details.

      # -------------- if enabled is true ---------------

      url: # [string] Proxy URL - URL of the Elastic server to proxy non-bulk requests to, such as http://elastic:9200
      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as self-signed certificates)
      removeHeaders: # [array of strings] Remove headers - List of headers to remove from the request to proxy
      timeoutSec: # [number; minimum: 1, maximum: 9007199254740991] Proxy request timeout - Amount of time, in seconds, to wait for a proxy request to complete before canceling it
      authType: # [string] Authentication method - Enter credentials directly, or select a stored secret

      # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  confluent_cloud_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Bootstrap servers - List of Confluent Cloud bootstrap servers to use, such as yourAccount.confluent.cloud:9092
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

    topics: # [array of strings] [required] Topic - Topic to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Kafka Source to a single topic only.
    groupId: # [string] Group ID - The consumer group to which this instance belongs. Defaults to 'Cribl'.
    fromBeginning: # [boolean] From beginning - Leave enabled if you want the Source, upon first subscribing to a topic, to read starting with the earliest available message
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


      # --------------------------------------------------------

    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Stream can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
    sasl: # [object] Authentication - Authentication parameters to use when connecting to brokers. Using TLS is highly recommended.
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      mechanism: # [string] SASL mechanism

      # --------------------------------------------------------

    sessionTimeout: # [number; minimum: 1000, maximum: 3600000] Session timeout (ms) - 
      Timeout used to detect client failures when using Kafka's group-management facilities.
      If the client sends no heartbeats to the broker before the timeout expires, 
      the broker will remove the client from the group and initiate a rebalance.
      Value must be between the broker's configured group.min.session.timeout.ms and group.max.session.timeout.ms.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_session.timeout.ms) for details.
    rebalanceTimeout: # [number; minimum: 1000, maximum: 3600000] Rebalance timeout (ms) - 
      Maximum allowed time for each worker to join the group after a rebalance begins.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#connectconfigs_rebalance.timeout.ms) for details.
    heartbeatInterval: # [number; minimum: 1000, maximum: 3600000] Heartbeat interval (ms) - 
      Expected time between heartbeats to the consumer coordinator when using Kafka's group-management facilities.
      Value must be lower than sessionTimeout and typically should not exceed 1/3 of the sessionTimeout value.
      See [Kafka's documentation](https://kafka.apache.org/documentation/#consumerconfigs_heartbeat.interval.ms) for details.
    autoCommitInterval: # [number; minimum: 1000, maximum: 3600000] Offset commit interval (ms) - How often to commit offsets. If both this and Offset commit threshold are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    autoCommitThreshold: # [number; minimum: 1, maximum: 10000] Offset commit threshold - How many events are needed to trigger an offset commit. If both this and Offset commit interval are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    maxBytesPerPartition: # [number; minimum: 1, maximum: 10000000] Byte limit, per partition - Maximum amount of data that Kafka will return per partition, per fetch request. Must equal or exceed the maximum message size (maxBytesPerPartition) that Kafka is configured to allow. Otherwise, Cribl Stream can get stuck trying to retrieve messages. Defaults to 1048576 (1 MB).
    maxBytes: # [number; minimum: 1, maximum: 1000000000] Byte limit - Maximum number of bytes that Kafka will return per fetch request. Defaults to 10485760 (10 MB).
    maxSocketErrors: # [number; maximum: 100] Error limit, per socket - Maximum number of network errors before the consumer re-creates a socket
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  grafana_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep alive timeout (seconds) - Maximum time to wait for additional data, after the last response was sent, before closing a socket connection. This can be very useful when Grafana Agent remote write's request frequency is high so, reusing connections, would help mitigating the cost of creating a new connection per request. Note that Grafana Agent's embedded Prometheus would attempt to keep connections open for up to 5 minutes.
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    prometheusAPI: # [string] Remote Write API endpoint - Absolute path on which to listen for Grafana Agent's Remote Write requests. Defaults to /api/prom/push, which will expand as: 'http://<your‑upstream‑URL>:<your‑port>/api/prom/push'. Either this field or 'Logs API endpoint' must be configured.
    lokiAPI: # [string] Logs API endpoint - Absolute path on which to listen for Loki logs requests. Defaults to /loki/api/v1/push, which will (in this example) expand as: 'http://<your‑upstream‑URL>:<your‑port>/loki/api/v1/push'. Either this field or 'Remote Write API endpoint' must be configured.
    prometheusAuth: # [object] 
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
      oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
        - name: # [string] Name - OAuth parameter name
          value: # [string] Value - OAuth parameter value
      oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
        - name: # [string] Name - OAuth header name
          value: # [string] Value - OAuth header value

      # --------------------------------------------------------

    lokiAuth: # [object] 
      authType: # [string] Authentication type - Loki logs authentication type

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
      oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
        - name: # [string] Name - OAuth parameter name
          value: # [string] Value - OAuth parameter value
      oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
        - name: # [string] Name - OAuth header name
          value: # [string] Value - OAuth header value

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  loki_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    lokiAPI: # [string] [required] Logs API endpoint - Absolute path on which to listen for Loki logs requests. Defaults to /loki/api/v1/push, which will (in this example) expand as: 'http://<your‑upstream‑URL>:<your‑port>/loki/api/v1/push'.
    authType: # [string] Authentication type - Loki logs authentication type

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
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  prometheus_rw_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    prometheusAPI: # [string] [required] Remote Write API endpoint - Absolute path on which to listen for Prometheus requests. Defaults to /write, which will expand as: http://<your‑upstream‑URL>:<your‑port>/write.
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
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  prometheus_input: # [object] 
    type: # [string] Input Type
    dimensionList: # [array of strings] Extra Dimensions - Other dimensions to include in events
    discoveryType: # [string] Discovery Type - Target discovery mechanism. Use static to manually enter a list of targets.

    # -------------- if discoveryType is static ---------------

    targetList: # [array of strings] Targets - List of Prometheus targets to pull metrics from. Values can be in URL or host[:port] format. For example: http://localhost:9090/metrics, localhost:9090, or localhost. In cases where just host[:port] is specified, the endpoint will resolve to 'http://host[:port]/metrics'.

    # --------------------------------------------------------


    # -------------- if discoveryType is dns ---------------

    nameList: # [array of strings] DNS Names - List of DNS names to resolve
    recordType: # [string] Record Type - DNS Record type to resolve
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets

    # --------------------------------------------------------


    # -------------- if discoveryType is ec2 ---------------

    usePublicIp: # [boolean] Use Public IP - Use public IP address for discovered targets. Set to false if the private IP address should be used.
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePort: # [number; minimum: 1, maximum: 65535] Metrics Port - The port number in the metrics URL for discovered targets.
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets
    searchFilter: # [array] Search Filter - EC2 Instance Search Filter
      - Name: # [string] Filter Name - Search filter attribute name, see: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html for more information. Attributes can be manually entered if not present in the drop down list
        Values: # [array of strings] Filter Values - Search Filter Values, if empty only "running" EC2 instances will be returned
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.
    awsSecretKey: # [string] Secret key
    region: # [string] Region - Region where the EC2 is located
    endpoint: # [string] Endpoint - EC2 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to EC2-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing EC2 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for EC2 - Use Assume Role credentials to access EC2
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).

    # --------------------------------------------------------

    interval: # [number; minimum: 1, maximum: 60] Poll Interval - How often in minutes to scrape targets for metrics, 60 must be evenly divisible by the value or save will fail.
    logLevel: # [string] [required] Log Level - Collector runtime Log Level
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as self-signed certificates)
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authType: # [string] Authentication method - Enter credentials directly, or select a stored secret
    username: # [string] Username - Username for Prometheus Basic authentication
    password: # [string] Password - Password for Prometheus Basic authentication

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  edge_prometheus_input: # [object] 
    type: # [string] Input Type
    dimensionList: # [array of strings] Extra Dimensions - Other dimensions to include in events
    discoveryType: # [string] [required] Discovery Type - Target discovery mechanism. Use static to manually enter a list of targets.

    # -------------- if discoveryType is static ---------------

    targets: # [array] Targets
      - protocol: # [string] Protocol - Protocol to use when collecting metrics
        host: # [string] Host - Name of host from which to pull metrics.
        port: # [number; minimum: 1, maximum: 65535] Port - The port number in the metrics URL for discovered targets.
        path: # [string] Path - Path to use when collecting metrics from discovered targets

    # --------------------------------------------------------


    # -------------- if discoveryType is dns ---------------

    nameList: # [array of strings] DNS Names - List of DNS names to resolve
    recordType: # [string] Record Type - DNS Record type to resolve
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets

    # --------------------------------------------------------


    # -------------- if discoveryType is ec2 ---------------

    usePublicIp: # [boolean] Use Public IP - Use public IP address for discovered targets. Set to false if the private IP address should be used.
    scrapeProtocol: # [string] Metrics Protocol - Protocol to use when collecting metrics
    scrapePort: # [number; minimum: 1, maximum: 65535] Metrics Port - The port number in the metrics URL for discovered targets.
    scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets
    searchFilter: # [array] Search Filter - EC2 Instance Search Filter
      - Name: # [string] Filter Name - Search filter attribute name, see: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html for more information. Attributes can be manually entered if not present in the drop down list
        Values: # [array of strings] Filter Values - Search Filter Values, if empty only "running" EC2 instances will be returned
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.
    awsSecretKey: # [string] Secret key
    region: # [string] Region - Region where the EC2 is located
    endpoint: # [string] Endpoint - EC2 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to EC2-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing EC2 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    enableAssumeRole: # [boolean] Enable for EC2 - Use Assume Role credentials to access EC2
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).

    # --------------------------------------------------------


    # -------------- if discoveryType is k8s-node ---------------

    scrapeProtocol: # [string] Protocol - Protocol to use when collecting metrics
    scrapePort: # [number; minimum: 1, maximum: 65535] Port - The port number in the metrics URL for discovered targets.
    scrapePath: # [string] Path - Path to use when collecting metrics from discovered targets

    # --------------------------------------------------------


    # -------------- if discoveryType is k8s-pods ---------------

    scrapeProtocolExpr: # [string] Protocol - Protocol to use when collecting metrics
    scrapePortExpr: # [string] Port - The port number in the metrics URL for discovered targets.
    scrapePathExpr: # [string] Path - Path to use when collecting metrics from discovered targets
    podFilter: # [array] Filter Rules - 
  Add rules to decide which pods to discover for metrics.
  Pods are searched if no rules are given or of all the rules'
  expressions evaluate to true.

      - filter: # [string] Filter Expression - JavaScript expression applied to pods objects. Return 'true' to include it.
        description: # [string] Description - Optional description of this rule's purpose

    # --------------------------------------------------------

    interval: # [number; minimum: 2] Poll Interval - How often in seconds to scrape targets for metrics.
    timeout: # [number; maximum: 60000] HTTP Connection Timeout - Timeout, in milliseconds, before aborting HTTP connection attempts; 1-60000 or 0 to disable
    persistence: # [object] Disk Spooling
      enable: # [boolean] Enable disk spooling - Spool events on disk for Cribl Edge and Search. Default is disabled.

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time period for grouping spooled events. Default is 10m.
      maxDataSize: # [string] Data size limit - Maximum disk space that can be consumed before older buckets are deleted. Examples: 420MB, 4GB. Default is 1GB.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data before older buckets are deleted. Examples: 2h, 4d. Default is 24h.
      compress: # [string] Compression - Data compression format. Default is gzip.

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    authType: # [string] Authentication method - Enter credentials directly, or select a stored secret
    username: # [string] Username - Username for Prometheus Basic authentication
    password: # [string] Password - Password for Prometheus Basic authentication

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  office365_mgmt_input: # [object] 
    type: # [string] Input Type
    planType: # [string] [required] Subscription plan - Office 365 subscription plan for your organization, typically Office 365 Enterprise
    tenantId: # [string] Tenant ID - Office 365 Azure Tenant ID
    appId: # [string] [required] App ID - Office 365 Azure Application ID
    timeout: # [number; maximum: 2400] Request timeout (seconds) - HTTP request inactivity timeout, use 0 to disable
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    publisherIdentifier: # [string] Publisher Identifier - Optional Publisher Identifier to use in API requests, defaults to tenant id if not defined. For more information see [here](https://docs.microsoft.com/en-us/office/office-365-management-api/office-365-management-activity-api-reference#start-a-subscription)
    contentConfig: # [array] Content Types - Enable Office 365 Management Activity API content types and polling intervals. Polling intervals are used to set up search date range and cron schedule, e.g.: */${interval} * * * *. Because of this, intervals entered must be evenly divisible by 60 to give a predictable schedule.
      - contentType: # [string] Content Type - Office 365 Management Activity API Content Type
        description: # [string] Interval Description - If interval type is minutes the value entered must evenly divisible by 60 or save will fail
        interval: # [number; minimum: 1, maximum: 60] Interval
        logLevel: # [string] Log Level - Collector runtime Log Level
        enabled: # [boolean] Enabled
    ingestionLag: # [number; maximum: 7200] Ingestion lag (minutes) - Use this setting to account for ingestion lag. This is necessary because there can be a lag of 60 - 90 minutes (or longer) before Office 365 events are available for retrieval.
    retryRules: # [object] 
      type: # [string] Retry type - The algorithm to use when performing HTTP retries

      # -------------- if type is static ---------------

      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      interval: # [number; maximum: 20000] Wait (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------


      # -------------- if type is backoff ---------------

      interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------

    authType: # [string] Authentication method - Enter client secret directly, or select a stored secret
    clientSecret: # [string] Client secret - Office 365 Azure client secret

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Client secret (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  office365_service_input: # [object] 
    type: # [string] Input Type
    planType: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Office 365 Enterprise
    tenantId: # [string] Tenant ID - Office 365 Azure Tenant ID
    appId: # [string] [required] App ID - Office 365 Azure Application ID
    timeout: # [number; maximum: 2400] Request timeout (seconds) - HTTP request inactivity timeout, use 0 to disable
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    contentConfig: # [array] Content Types - Enable Office 365 Service Communication API content types and polling intervals. Polling intervals are used to set up search date range and cron schedule, e.g.: */${interval} * * * *. Because of this, intervals entered for current and historical status must be evenly divisible by 60 to give a predictable schedule.
      - contentType: # [string] Content Type - Office 365 Services API Content Type
        description: # [string] Interval Description - If interval type is minutes the value entered must evenly divisible by 60 or save will fail
        interval: # [number; maximum: 60] Interval
        logLevel: # [string] Log Level - Collector runtime Log Level
        enabled: # [boolean] Enabled
    retryRules: # [object] 
      type: # [string] Retry type - The algorithm to use when performing HTTP retries

      # -------------- if type is static ---------------

      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      interval: # [number; maximum: 20000] Wait (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------


      # -------------- if type is backoff ---------------

      interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------

    authType: # [string] Authentication method - Enter client secret directly, or select a stored secret
    clientSecret: # [string] Client secret - Office 365 Azure client secret

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Client secret (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  office365_msg_trace_input: # [object] 
    type: # [string] Input Type
    url: # [string] Report URL - URL to use when retrieving report data.
    interval: # [number; minimum: 1, maximum: 60] [required] Poll interval - How often (in minutes) to run the report. Must divide evenly into 60 minutes to create a predictable schedule, or Save will fail.
    startDate: # [string] Date range start - Backward offset for the search range's head. (E.g.: -3h@h) Message Trace data is delayed; this parameter (with Date range end) compensates for delay and gaps.
    endDate: # [string] Date range end - Backward offset for the search range's tail. (E.g.: -2h@h) Message Trace data is delayed; this parameter (with Date range start) compensates for delay and gaps.
    timeout: # [number; maximum: 2400] Request timeout (seconds) - HTTP request inactivity timeout. Maximum is 2400 (40 minutes); enter 0 to wait indefinitely.
    disableTimeFilter: # [boolean] Disable time filter - Disables time filtering of events when a date range is specified.
    authType: # [string] Authentication method - Select authentication method.

    # -------------- if authType is manual ---------------

    username: # [string] Username - Username to run Message Trace API call.
    password: # [string] Password - Password to run Message Trace API call.

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials.

    # --------------------------------------------------------


    # -------------- if authType is oauth ---------------

    clientSecret: # [string] Client secret - client_secret to pass in the OAuth request parameter.
    tenantId: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory.
    clientId: # [string] Client ID - client_id to pass in the OAuth request parameter.
    resource: # [string] Resource - Resource to pass in the OAuth request parameter.
    planType: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Office 365 Enterprise

    # --------------------------------------------------------


    # -------------- if authType is oauthSecret ---------------

    textSecret: # [string] Client secret - Select or create a secret that references your client_secret to pass in the OAuth request parameter.
    tenantId: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory.
    clientId: # [string] Client ID - client_id to pass in the OAuth request parameter.
    resource: # [string] Resource - Resource to pass in the OAuth request parameter.
    planType: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Office 365 Enterprise

    # --------------------------------------------------------


    # -------------- if authType is oauthCert ---------------

    certOptions: # [object] 
      certificateName: # [string] Certificate - The name of the predefined certificate.
      privKeyPath: # [string] Private key path - Path to the private key to use. Key should be in PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt the private key.
      certPath: # [string] [required] Certificate path - Path to the certificate to use. Certificate should be in PEM format. Can reference $ENV_VARS.
    tenantId: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory.
    clientId: # [string] Client ID - client_id to pass in the OAuth request parameter.
    resource: # [string] Resource - Resource to pass in the OAuth request parameter.
    planType: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Office 365 Enterprise

    # --------------------------------------------------------

    rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors
    maxTaskReschedule: # [number; minimum: 1] Task reschedule limit - Maximum number of times a task can be rescheduled
    logLevel: # [string] Log level - Log Level (verbosity) for collection runtime behavior.
    jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    retryRules: # [object] 
      type: # [string] Retry type - The algorithm to use when performing HTTP retries

      # -------------- if type is static ---------------

      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      interval: # [number; maximum: 20000] Wait (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------


      # -------------- if type is backoff ---------------

      interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
      codes: # [array of numbers] Retry HTTP codes - List of http codes that trigger a retry. Leave empty to use the default list of 429, 500, and 503.
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  eventhub_input: # [object] 
    type: # [string] Input Type
    brokers: # [array of strings] Brokers - List of Event Hubs Kafka brokers to connect to (example: yourdomain.servicebus.windows.net:9093). The hostname can be found in the host portion of the primary or secondary connection string in Shared Access Policies.
    topics: # [array of strings] [required] Event Hub name - The name of the Event Hub (Kafka topic) to subscribe to. Warning: To optimize performance, Cribl suggests subscribing each Event Hubs Source to only a single topic.
    groupId: # [string] Group ID - The consumer group this instance belongs to. Default is 'Cribl'.
    fromBeginning: # [boolean] From beginning - Start reading from earliest available data; relevant only during initial subscription
    connectionTimeout: # [number; minimum: 1000, maximum: 3600000] Connection timeout (ms) - Maximum time to wait for a connection to complete successfully
    requestTimeout: # [number; minimum: 1000, maximum: 3600000] Request timeout (ms) - Maximum time to wait for Kafka to respond to a request
    maxRetries: # [number; maximum: 100] Retry limit - If messages are failing, you can set the maximum number of retries as high as 100 to prevent loss of data
    maxBackOff: # [number; minimum: 30000, maximum: 180000] Backoff limit (ms) - The maximum wait time for a retry, in milliseconds. Default (and minimum) is 30,000 ms (30 seconds); maximum is 180,000 ms (180 seconds).
    initialBackoff: # [number; minimum: 300, maximum: 600000] Initial retry interval (ms) - Initial value used to calculate the retry, in milliseconds. Maximum is 600,000 ms (10 minutes).
    backoffRate: # [number; minimum: 2, maximum: 20] Backoff multiplier - Set the backoff multiplier (2-20) to control the retry frequency for failed messages. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. The multiplier is used in an exponential backoff formula; see the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.
    authenticationTimeout: # [number; minimum: 1000, maximum: 3600000] Authentication timeout (ms) - Maximum time to wait for Kafka to respond to an authentication request
    reauthenticationThreshold: # [number; minimum: 1000, maximum: 1800000] Reauthentication threshold (ms) - Specifies a time window during which Cribl Stream can reauthenticate if needed. Creates the window measuring backward from the moment when credentials are set to expire.
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

    sessionTimeout: # [number; minimum: 6000, maximum: 300000] Session timeout (ms) - 
      Timeout (session.timeout.ms in Kafka domain) used to detect client failures when using Kafka's group-management facilities.
      If the client sends no heartbeats to the broker before the timeout expires, the broker will remove the client from the group and initiate a rebalance.
      Value must be lower than rebalanceTimeout.
      See details [here](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    rebalanceTimeout: # [number; minimum: 1000, maximum: 3600000] Rebalance timeout (ms) - 
      Maximum allowed time (rebalance.timeout.ms in Kafka domain) for each worker to join the group after a rebalance begins.
      If the timeout is exceeded, the coordinator broker will remove the worker from the group.
      See [Recommended configurations](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    heartbeatInterval: # [number; minimum: 1000, maximum: 3600000] Heartbeat interval (ms) - 
      Expected time (heartbeat.interval.ms in Kafka domain) between heartbeats to the consumer coordinator when using Kafka's group-management facilities.
      Value must be lower than sessionTimeout and typically should not exceed 1/3 of the sessionTimeout value.
      See [Recommended configurations](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md).
    autoCommitInterval: # [number; minimum: 1000, maximum: 3600000] Offset commit interval (ms) - How often to commit offsets. If both this and Offset commit threshold are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    autoCommitThreshold: # [number; minimum: 1, maximum: 10000] Offset commit threshold - How many events are needed to trigger an offset commit. If both this and Offset commit interval are set, Cribl Stream commits offsets when either condition is met. If both are empty, Cribl Stream commits offsets after each batch.
    maxBytesPerPartition: # [number; minimum: 1, maximum: 10000000] Byte limit, per partition - Maximum amount of data that Kafka will return per partition, per fetch request. Must equal or exceed the maximum message size (maxBytesPerPartition) that Kafka is configured to allow. Otherwise, Cribl Stream can get stuck trying to retrieve messages. Defaults to 1048576 (1 MB).
    maxBytes: # [number; minimum: 1, maximum: 1000000000] Byte limit - Maximum number of bytes that Kafka will return per fetch request. Defaults to 10485760 (10 MB).
    maxSocketErrors: # [number; maximum: 100] Error limit, per socket - Maximum number of network errors before the consumer re-creates a socket
    minimizeDuplicates: # [boolean] Minimize duplicates - Minimize duplicate events by starting only one consumer for each topic partition
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  exec_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    command: # [string] Command - Command to execute; supports Bourne shell (or CMD on Windows) syntax
    retries: # [number] Retry limit - Maximum number of retry attempts in the event that the command fails
    scheduleType: # [string] Schedule type - Select a schedule type; either an interval (in seconds) or a cron-style schedule.

    # -------------- if scheduleType is interval ---------------

    interval: # [number; minimum: 1] Interval - Interval between command executions in seconds.

    # --------------------------------------------------------


    # -------------- if scheduleType is cronSchedule ---------------

    cronSchedule: # [string] Schedule - Cron schedule to execute the command on.

    # --------------------------------------------------------

    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  firehose_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  google_pubsub_input: # [object] 
    type: # [string] Input Type
    topicName: # [string] Topic ID - ID of the topic to receive events from
    subscriptionName: # [string] [required] Subscription ID - ID of the subscription to use when receiving events
    createTopic: # [boolean] Create topic - Create topic if it does not exist
    createSubscription: # [boolean] Create subscription - Create subscription if it does not exist

    # -------------- if createSubscription is true ---------------

    orderedDelivery: # [boolean] Ordered delivery - Receive events in the order they were added to the queue. The process sending events must have ordering enabled.

    # --------------------------------------------------------

    region: # [string] Region - Region to retrieve messages from. Select 'default' to allow Google to auto-select the nearest region. When using ordered delivery, the selected region must be allowed by message storage policy.
    googleAuthMethod: # [string] Google authentication method - Choose Auto to use Google Application Default Credentials (ADC), Manual to enter Google service account credentials directly, or Secret to select or create a stored secret that references Google service account credentials.

    # -------------- if googleAuthMethod is manual ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right.

    # --------------------------------------------------------


    # -------------- if googleAuthMethod is secret ---------------

    secret: # [string] Service account credentials (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    maxBacklog: # [number; minimum: 1] Backlog limit - If Destination exerts backpressure, this setting limits how many inbound events Stream will queue for processing before it stops retrieving events
    concurrency: # [number; minimum: 1, maximum: 100] Number of concurrent streams - How many streams to pull messages from at one time. Doubling the value doubles the number of messages this Source pulls from the topic (if available), while consuming more CPU and memory. Defaults to 5.
    requestTimeout: # [number; minimum: 10000] Request timeout (ms) - Pull request timeout, in milliseconds
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  cribl_input: # [object] 
    type: # [string] Input Type
    filter: # [string] 
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  cribl_tcp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    socketIdleTimeout: # [number] Socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] Forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports proxy protocol v1 or v2
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    enableLoadBalancing: # [boolean] Enable load balancing - Load balance traffic across all Worker Processes
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  cribl_http_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  cribl_lake_http_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  tcpjson_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to establish a connection
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    socketIdleTimeout: # [number] Socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] Forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports proxy protocol v1 or v2
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    enableLoadBalancing: # [boolean] Enable load balancing - Load balance traffic across all Worker Processes
    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
    authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthorized access is permitted.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  system_metrics_input: # [object] 
    type: # [string] Input Type
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between consecutive metric collections. Default is 10 seconds.
    host: # [object] 
      mode: # [string]  - Select level of detail for host metrics

      # -------------- if mode is custom ---------------

      custom: # [object] 
        system: # [object] 
          mode: # [string]  - Select the level of detail for system metrics

          # -------------- if mode is custom ---------------

          processes: # [boolean] Process metrics - Generate metrics for the numbers of processes in various states

          # --------------------------------------------------------

        cpu: # [object] 
          mode: # [string]  - Select the level of detail for CPU metrics

          # -------------- if mode is custom ---------------

          perCpu: # [boolean] Per-CPU metrics - Generate metrics for each CPU
          detail: # [boolean] Detailed metrics - Generate metrics for all CPU states
          time: # [boolean] CPU time metrics - Generate raw, monotonic CPU time counters

          # --------------------------------------------------------

        memory: # [object] 
          mode: # [string]  - Select the level of detail for memory metrics

          # -------------- if mode is custom ---------------

          detail: # [boolean] Detailed metrics - Generate metrics for all memory states

          # --------------------------------------------------------

        network: # [object] 
          mode: # [string]  - Select the level of detail for network metrics

          # -------------- if mode is custom ---------------

          devices: # [array of strings] Interface filter - Network interfaces to include/exclude. Examples: eth0, !lo. All interfaces are included if this list is empty.
          perInterface: # [boolean] Per-interface metrics - Generate separate metrics for each interface
          detail: # [boolean] Detailed metrics - Generate full network metrics

          # --------------------------------------------------------

        disk: # [object] 
          mode: # [string]  - Select the level of detail for disk metrics

          # -------------- if mode is custom ---------------

          devices: # [array of strings] Device filter - Block devices to include/exclude. Examples: sda*, !loop*. Wildcards and ! (not) operators are supported. All devices are included if this list is empty.
          mountpoints: # [array of strings] Mountpoint filter - Filesystem mountpoints to include/exclude. Examples: /, /home, !/proc*, !/tmp. Wildcards and ! (not) operators are supported. All mountpoints are included if this list is empty.
          fstypes: # [array of strings] Filesystem type filter - Filesystem types to include/exclude. Examples: ext4, !*tmpfs, !squashfs. Wildcards and ! (not) operators are supported. All types are included if this list is empty.
          perDevice: # [boolean] Per-device metrics - Generate separate metrics for each device
          detail: # [boolean] Detailed metrics - Generate full disk metrics

          # --------------------------------------------------------


      # --------------------------------------------------------

    process: # [object] 
      sets: # [array] Process sets - Configure sets to collect process metrics
        - name: # [string] Set Name
          filter: # [string] Filter Expression
          includeChildren: # [boolean] Include Child Processes
    container: # [object] 
      mode: # [string]  - Select the level of detail for container metrics

      # -------------- if mode is basic ---------------

      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number; minimum: 1] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------


      # -------------- if mode is all ---------------

      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number; minimum: 1] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------


      # -------------- if mode is custom ---------------

      filters: # [array] Container filters - Containers matching any of these will be included. All are included if no filters are added.
        - expr: # [string] Expression
      allContainers: # [boolean] All containers - Include stopped and paused containers
      perDevice: # [boolean] Per-device metrics - Generate separate metrics for each device
      detail: # [boolean] Detailed metrics - Generate full container metrics
      dockerSocket: # [array of strings] Docker socket - Full paths for Docker's UNIX-domain socket
      dockerTimeout: # [number; minimum: 1] Docker timeout - Timeout, in seconds, for the Docker API

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] persistence
      enable: # [boolean] Enable disk spooling - Spool metrics to disk for Cribl Edge and Search

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
      compress: # [string] Data compression format
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/system_metrics

      # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  system_state_input: # [object] 
    type: # [string] Input Type
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between consecutive state collections. Default is 300 seconds (5 minutes).
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    collectors: # [object] 
      hostsfile: # [object] Hosts File - Creates events based on entries collected from the hosts file
        enable: # [boolean] Enabled
      interfaces: # [object] Interfaces - Creates events for each of the host’s network interfaces
        enable: # [boolean] Enabled
      disk: # [object] Disks & File Systems - Creates events for physical disks, partitions, and file systems
        enable: # [boolean] Enabled
      metadata: # [object] Host Info - Creates events based on the host system’s current state
        enable: # [boolean] Enabled
      routes: # [object] Routes - Creates events based on entries collected from the host’s network routes
        enable: # [boolean] Enabled
      dns: # [object] DNS - Creates events for DNS resolvers and search entries
        enable: # [boolean] Enabled
      user: # [object] Users & Groups - Creates events for local users and groups
        enable: # [boolean] Enabled
      firewall: # [object] Firewall - Creates events for Firewall rules entries
        enable: # [boolean] Enabled
      services: # [object] Services - Creates events from the list of services
        enable: # [boolean] Enabled
      ports: # [object] Listening Ports - Creates events from list of listening ports
        enable: # [boolean] Enabled
      loginUsers: # [object] Logged-In Users - Creates events from list of logged-in users
        enable: # [boolean] Enabled
    persistence: # [object] 
      enable: # [boolean] Enable disk spooling - Spool metrics to disk for Cribl Edge and Search

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
      compress: # [string] Data compression format
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/system_state

      # --------------------------------------------------------

    disableNativeModule: # [boolean] Use Windows Tools - Enable to use built-in tools (PowerShell) to collect events instead of native API (default) [Learn more](https://docs.cribl.io/edge/sources-system-state/#advanced-tab)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  kube_metrics_input: # [object] 
    type: # [string] Input Type
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between consecutive metrics collections. Default is 15 secs.
    rules: # [array] Filter Rules - Add rules to decide which Kubernetes objects to generate metrics for. Events are generated if no rules are given or of all the rules' expressions evaluate to true.
      - filter: # [string] Filter Expression - JavaScript expression applied to Kubernetes objects. Return 'true' to include it.
        description: # [string] Description - Optional description of this rule's purpose
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] persistence
      enable: # [boolean] Enable disk spooling - Spool metrics on disk for Cribl Search

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
      compress: # [string] Data compression format
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>

      # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  kube_logs_input: # [object] 
    type: # [string] Input Type
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between checks for new containers. Default is 15 secs.
    rules: # [array] Filter Rules - Add rules to decide which Pods to collect logs from. Logs are collected if no rules are given or if all the rules' expressions evaluate to true.
      - filter: # [string] Filter Expression - JavaScript expression applied to Pod objects. Return 'true' to include it.
        description: # [string] Description - Optional description of this rule's purpose
    timestamps: # [boolean] Enable timestamps - For use when containers do not emit a timestamp, prefix each line of output with a timestamp. If you enable this setting, you can use the Kubernetes Logs Event Breaker and the kubernetes_logs Pre-processing Pipeline to remove them from the events after the timestamps are extracted.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] Disk Spooling
      enable: # [boolean] Enable disk spooling - Spool events on disk for Cribl Edge and Search. Default is disabled.

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time period for grouping spooled events. Default is 10m.
      maxDataSize: # [string] Data size limit - Maximum disk space that can be consumed before older buckets are deleted. Examples: 420MB, 4GB. Default is 1GB.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data before older buckets are deleted. Examples: 2h, 4d. Default is 24h.
      compress: # [string] Compression - Data compression format. Default is gzip.

      # --------------------------------------------------------

    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    enableLoadBalancing: # [boolean] Enable load balancing - Load balance traffic across all Worker Processes
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  kube_events_input: # [object] 
    type: # [string] Input Type
    rules: # [array] Filter Rules - Filtering on event fields
      - filter: # [string] Filter Expression - JavaScript expression applied to Kubernetes objects. Return 'true' to include it.
        description: # [string] Description - Optional description of this rule's purpose
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  windows_metrics_input: # [object] 
    type: # [string] Input Type
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between consecutive metric collections. Default is 10 seconds.
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

          perCpu: # [boolean] Per-CPU metrics - Generate metrics for each CPU
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

    process: # [object] 
      sets: # [array] Process sets - Configure sets to collect process metrics
        - name: # [string] Set Name
          filter: # [string] Filter Expression
          includeChildren: # [boolean] Include Child Processes
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    persistence: # [object] persistence
      enable: # [boolean] Enable disk spooling - Spool metrics to disk for Cribl Edge and Search

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
      compress: # [string] Data compression format
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/windows_metrics

      # --------------------------------------------------------

    disableNativeModule: # [boolean] Use Windows Tools - Enable to use built-in tools (PowerShell) to collect metrics instead of native API (default) [Learn more](https://docs.cribl.io/edge/sources-windows-metrics/#advanced-tab)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  crowdstrike_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    maxMessages: # [number; minimum: 1, maximum: 10] Message limit - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number; maximum: 43200] Visibility timeout seconds - After messages are retrieved by a ReceiveMessage request, Cribl Stream will hide them from subsequent retrieve requests for at least this duration. You can set this as high as 43200 sec. (12 hours).
    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.
    socketTimeout: # [number; minimum: 1, maximum: 43200] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Skip files that trigger a processing error. Disabled by default, which allows retries after processing errors.
    enableAssumeRole: # [boolean] Enable for Amazon S3 - Use Assume Role credentials to access Amazon S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    enableSQSAssumeRole: # [boolean] Enable for Amazon SQS - Use Assume Role credentials when accessing Amazon SQS
    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    checkpointing: # [object] 
      enabled: # [boolean] Enable checkpointing - Resume processing files after an interruption

      # -------------- if enabled is true ---------------

      retries: # [number; maximum: 100] Retries - The number of times to retry processing when a processing error occurs. If Skip file on error is enabled, this setting is ignored.

      # --------------------------------------------------------

    pollTimeout: # [number; minimum: 1, maximum: 20] Poll timeout (secs) - How long to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    encoding: # [string] Encoding - Character encoding to use when parsing ingested data. When not set, Cribl Stream will default to UTF-8 but may incorrectly interpret multi-byte characters.
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  datadog_agent_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    extractMetrics: # [boolean] Extract metrics - Toggle to Yes to extract each incoming metric to multiple events, one per data point. This works well when sending metrics to a statsd-type output. If sending metrics to DatadogHQ or any destination that accepts arbitrary JSON, leave toggled to No (the default).
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    proxyMode: # [object] 
      enabled: # [boolean] Forward API key validation requests - Toggle to Yes to send key validation requests from Datadog Agent to the Datadog API. If toggled to No (the default), Stream handles key validation requests by always responding that the key is valid.

      # -------------- if enabled is true ---------------

      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).

      # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  datagen_input: # [object] 
    type: # [string] Input Type
    samples: # [array] Datagens
      - sample: # [string] Data Generator File Name
        eventsPerSec: # [number; minimum: 1] Events Per Second Per Worker Node - Maximum number of events to generate per second per Worker Node. Defaults to 10.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  http_raw_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array of strings] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    allowedPaths: # [array of strings] Allowed URI paths - List of URI paths accepted by this input, wildcards are supported, e.g /api/v*/hook. Defaults to allow all.
    allowedMethods: # [array of strings] Allowed HTTP methods - List of HTTP methods accepted by this input. Wildcards are supported (such as P*, GET). Defaults to allow all.
    authTokensExt: # [array] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
      - token: # [string] Token - Shared secret to be provided by any client (Authorization: <token>)
        description: # [string] Description
        metadata: # [array] Fields - Fields to add to events referencing this token
        - name: # [string] Field Name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  kinesis_input: # [object] 
    type: # [string] Input Type
    streamName: # [string] Stream name - Kinesis Data Stream to read data from
    serviceInterval: # [number; minimum: 1, maximum: 5] Service period - Time interval in minutes between consecutive service calls
    shardExpr: # [string] Shard selection expression - A JavaScript expression to be called with each shardId for the stream. If the expression evaluates to a truthy value, the shard will be processed.
    shardIteratorType: # [string] Shard iterator start - Location at which to start reading a shard for the first time
    payloadFormat: # [string] Record data format - Format of data inside the Kinesis Stream records. Gzip compression is automatically detected.
    getRecordsLimit: # [number; minimum: 5000, maximum: 10000] Records limit per call - Maximum number of records per getRecords call
    getRecordsLimitTotal: # [number; minimum: 20000] Total records limit - Maximum number of records, across all shards, to pull down at once per Worker Process
    loadBalancingAlgorithm: # [string] Shard load balancing - The load-balancing algorithm to use for spreading out shards across Workers and Worker Processes
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
    verifyKPLCheckSums: # [boolean] Verify KPL checksums - Verify Kinesis Producer Library (KPL) event checksums
    avoidDuplicates: # [boolean] Avoid duplicate records - When resuming streaming from a stored state, Stream will read the next available record, rather than rereading the last-read record. Enabling this setting can cause data loss after a Worker Node's unexpected shutdown or restart.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  criblmetrics_input: # [object] 
    type: # [string] Input Type
    prefix: # [string] Metric name prefix - A prefix that is applied to the metrics provided by Cribl Stream
    fullFidelity: # [boolean] Full fidelity - Include granular metrics. Disabling this will drop the following metrics events: `cribl.logstream.host.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.index.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.source.(in_bytes,in_events,out_bytes,out_events)`, `cribl.logstream.sourcetype.(in_bytes,in_events,out_bytes,out_events)`.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  metrics_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    udpPort: # [number; maximum: 65535] UDP Port - Enter UDP port number to listen on. Not required if listening on TCP.
    tcpPort: # [number; maximum: 65535] TCP Port - Enter TCP port number to listen on. Not required if listening on UDP.
    maxBufferSize: # [number] Buffer size limit (events) - Maximum number of events to buffer when downstream is blocking. Only applies to UDP.
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to send data
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    udpSocketRxBufSize: # [number; minimum: 256, maximum: 4294967295] UDP socket buffer size (bytes) - Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Caution: Increasing this value will affect OS memory utilization.
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  s3_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    maxMessages: # [number; minimum: 1, maximum: 10] Message limit - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number; maximum: 43200] Visibility timeout seconds - After messages are retrieved by a ReceiveMessage request, Cribl Stream will hide them from subsequent retrieve requests for at least this duration. You can set this as high as 43200 sec. (12 hours).
    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.
    socketTimeout: # [number; minimum: 1, maximum: 43200] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Skip files that trigger a processing error. Disabled by default, which allows retries after processing errors.
    enableAssumeRole: # [boolean] Enable for Amazon S3 - Use Assume Role credentials to access Amazon S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    enableSQSAssumeRole: # [boolean] Enable for Amazon SQS - Use Assume Role credentials when accessing Amazon SQS
    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
    parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will stop if a chunk cannot be downloaded within the time specified.
    checkpointing: # [object] 
      enabled: # [boolean] Enable checkpointing - Resume processing files after an interruption

      # -------------- if enabled is true ---------------

      retries: # [number; maximum: 100] Retries - The number of times to retry processing when a processing error occurs. If Skip file on error is enabled, this setting is ignored.

      # --------------------------------------------------------

    pollTimeout: # [number; minimum: 1, maximum: 20] Poll timeout (secs) - How long to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    encoding: # [string] Encoding - Character encoding to use when parsing ingested data. When not set, Cribl Stream will default to UTF-8 but may incorrectly interpret multi-byte characters.
    tagAfterProcessing: # [boolean] Tag after processing - Add a tag to processed S3 objects. Requires s3:GetObjectTagging and s3:PutObjectTagging AWS permissions.

    # -------------- if tagAfterProcessing is true ---------------

    processedTagKey: # [string] Tag key - The key for the S3 object tag applied after processing. This field accepts an expression for dynamic generation.
    processedTagValue: # [string] Tag value - The value for the S3 object tag applied after processing. This field accepts an expression for dynamic generation.

    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  s3_inventory_input: # [object] 
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    maxMessages: # [number; minimum: 1, maximum: 10] Message limit - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number; maximum: 43200] Visibility timeout seconds - After messages are retrieved by a ReceiveMessage request, Cribl Stream will hide them from subsequent retrieve requests for at least this duration. You can set this as high as 43200 sec. (12 hours).
    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.
    socketTimeout: # [number; minimum: 1, maximum: 43200] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Skip files that trigger a processing error. Disabled by default, which allows retries after processing errors.
    enableAssumeRole: # [boolean] Enable for Amazon S3 - Use Assume Role credentials to access Amazon S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    enableSQSAssumeRole: # [boolean] Enable for Amazon SQS - Use Assume Role credentials when accessing Amazon SQS
    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
    parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will stop if a chunk cannot be downloaded within the time specified.
    checkpointing: # [object] 
      enabled: # [boolean] Enable checkpointing - Resume processing files after an interruption

      # -------------- if enabled is true ---------------

      retries: # [number; maximum: 100] Retries - The number of times to retry processing when a processing error occurs. If Skip file on error is enabled, this setting is ignored.

      # --------------------------------------------------------

    pollTimeout: # [number; minimum: 1, maximum: 20] Poll timeout (secs) - How long to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    type: # [string] Input Type
    checksumSuffix: # [string] Checksum Suffix - Filename suffix of the manifest checksum file. If a filename matching this suffix is received        in the queue, the matching manifest file will be downloaded and validated against its value. Defaults to "checksum"
    maxManifestSizeKB: # [integer; minimum: 1] Manifest size limit (KB) - Maximum download size (KB) of each manifest or checksum file. Manifest files larger than this size will not be read.        Defaults to 4096.
    validateInventoryFiles: # [boolean] Validate inventory files - If set to Yes, each inventory file in the manifest will be validated against its checksum. Defaults to false
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  snmp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    port: # [number; maximum: 65535] [required] UDP port - UDP port to receive SNMP traps on. Defaults to 162.
    snmpV3Auth: # [object] SNMPv3 authentication - Authentication parameters for SNMPv3 trap. Set the log level to debug if you are experiencing authentication or decryption issues.
      v3AuthEnabled: # [boolean] Enabled

      # -------------- if v3AuthEnabled is true ---------------

      allowUnmatchedTrap: # [boolean] Allow unmatched traps - Pass through traps that don't match any of the configured users. Cribl Stream will not attempt to decrypt these traps.
      v3Users: # [array] SNMP v3 users - User credentials for receiving v3 traps
        - name: # [string] V3 name
          authProtocol: # [string] Authentication protocol

      # --------------------------------------------------------

    maxBufferSize: # [number] Buffer size limit (events) - Maximum number of events to buffer when downstream is blocking.
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to send data
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    udpSocketRxBufSize: # [number; minimum: 256, maximum: 4294967295] UDP socket buffer size (bytes) - Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Caution: Increasing this value will affect OS memory utilization.
    varbindsWithTypes: # [boolean] Include varbind types - If enabled, parses varbinds as an array of objects that include OID, value, and type
    bestEffortParsing: # [boolean] Best effort parsing - If enabled, the parser will attempt to parse varbind octet strings as UTF-8, first, otherwise will fallback to other methods
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  open_telemetry_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    protocol: # [string] Protocol - Select whether to leverage gRPC or HTTP for OpenTelemetry

    # -------------- if protocol is http ---------------

    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 sec.; maximum 600 sec. (10 min.).
    enableHealthCheck: # [boolean] Health check endpoint - Enable to expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist.
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.

    # --------------------------------------------------------


    # -------------- if protocol is grpc ---------------

    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------


    # --------------------------------------------------------

    extractSpans: # [boolean] Extract spans - Enable to extract each incoming span to a separate event
    extractMetrics: # [boolean] Extract metrics - Enable to extract each incoming Gauge or IntGauge metric to multiple events, one per data point
    otlpVersion: # [string] OTLP version - The version of OTLP Protobuf definitions to use when interpreting received data

    # -------------- if otlpVersion is 1.3.1 ---------------

    extractLogs: # [boolean] Extract logs - Enable to extract each incoming log record to a separate event

    # --------------------------------------------------------

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
    oauthParams: # [array] OAuth parameters - Additional parameters to send in the OAuth login request. Cribl Stream will combine the secret with these parameters, and will send the URL-encoded result in a POST request to the endpoint specified in the 'Login URL'. We'll automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth parameter name
        value: # [string] Value - OAuth parameter value
    oauthHeaders: # [array] OAuth headers - Additional headers to send in the OAuth login request. Cribl Stream will automatically add the content-type header 'application/x-www-form-urlencoded' when sending this request.
      - name: # [string] Name - OAuth header name
        value: # [string] Value - OAuth header value

    # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  model_driven_telemetry_input: # [object] 
    type: # [string] Input Type
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    shutdownTimeoutMs: # [number; minimum: 1] Shutdown timeout - Time in milliseconds to allow the server to shutdown gracefully before forcing shutdown. Defaults to 5000.
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  sqs_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read events from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can only be evaluated at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    queueType: # [string] [required] Queue type - The queue type used (or created)

    # -------------- if queueType is standard ---------------

    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.

    # --------------------------------------------------------

    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    createQueue: # [boolean] Create queue - Create queue if it does not exist
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
    maxMessages: # [number; minimum: 1, maximum: 10] Message limit - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number; maximum: 43200] Visibility Timeout Seconds - After messages are retrieved by a ReceiveMessage request, Cribl Stream will hide them from subsequent retrieve requests for at least this duration. You can set this as high as 43200 sec. (12 hours).
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    pollTimeout: # [number; minimum: 1, maximum: 20] Poll timeout (secs) - How long to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  syslog_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    udpPort: # [number; maximum: 65535] UDP port - Enter UDP port number to listen on. Not required if listening on TCP.
    tcpPort: # [number; maximum: 65535] TCP port - Enter TCP port number to listen on. Not required if listening on UDP.
    maxBufferSize: # [number] Buffer size limit (events) - Maximum number of events to buffer when downstream is blocking. Only applies to UDP.
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to send data
    timestampTimezone: # [string] Default timezone - Timezone to assign to timestamps without timezone info
    singleMsgUdpPackets: # [boolean] Single msg per UDP - Treat UDP packet data received as full syslog message
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports Proxy Protocol V1 or V2

    # -------------- if enableProxyHeader is true ---------------

    enableEnhancedProxyHeaderParsing: # [boolean] Enable enhanced TLS handshake for proxy protocol - When enabled, parses PROXY protocol headers during the TLS handshake. Disable if compatibility issues arise.

    # --------------------------------------------------------

    keepFieldsList: # [array of strings] Fields to keep - Wildcard list of fields to keep from source data; * = ALL (default)
    octetCounting: # [boolean] Octet count framing - Enable if incoming messages use octet counting per RFC 6587.
    inferFraming: # [boolean] Infer Syslog framing - Enable if we should infer the syslog framing of the incoming messages.
    strictlyInferOctetCounting: # [boolean] Strictly infer octet count framing - Enable if we should infer octet counting only if the messages comply with RFC 5424.
    allowNonStandardAppName: # [boolean] Allow non-standard app name - Enable if RFC 3164-formatted messages have hyphens in the app name portion of the TAG section. If disabled, only alphanumeric characters and underscores are allowed. Ignored for RFC 5424-formatted messages.
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process for TCP connections. Use 0 for unlimited.
    socketIdleTimeout: # [number] TCP socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] TCP forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] TCP Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    udpSocketRxBufSize: # [number; minimum: 256, maximum: 4294967295] UDP socket buffer size (bytes) - Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Caution: Increasing this value will affect OS memory utilization.
    enableLoadBalancing: # [boolean] Enable TCP load balancing - Load balance traffic across all Worker Processes
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  file_input: # [object] 
    type: # [string] Input Type
    mode: # [string]  - Choose how to discover files to monitor

    # -------------- if mode is manual ---------------

    path: # [string] Search path - Directory path to search for files. Environment variables will be resolved, e.g. $CRIBL_HOME/log/.
    depth: # [number] Max depth - Set how many subdirectories deep to search. Use 0 to search only files in the given path, 1 to also look in its immediate subdirectories, etc. Leave it empty for unlimited depth.
    suppressMissingPathErrors: # [boolean] Suppress errors when search path does not exist
    deleteFiles: # [boolean] Delete files - Delete files after they have been collected

    # --------------------------------------------------------

    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between scanning for files
    filenames: # [array of strings] Filename allowlist - The full path of discovered files are matched against this wildcard list
    tailOnly: # [boolean] Collect from end - Read only new entries at the end of all files discovered at next startup. Cribl Stream will then read newly discovered files from the head. Disable this to resume reading all files from head.
    idleTimeout: # [number; minimum: 1] Idle timeout - Time, in seconds, before an idle file is closed
    maxAgeDur: # [string] Age duration limit - The maximum age of files to monitor. Format examples: 60s, 4h, 3d, 1w. Age is relative to file modification time. Leave empty to apply no age filters.
    checkFileModTime: # [boolean] Check file modification times - Skip files with modification times earlier than the maximum age duration
    forceText: # [boolean] Force text format - Forces files containing binary data to be streamed as text

    # -------------- if forceText is false ---------------

    includeUnidentifiableBinary: # [boolean] Enable binary files - Stream binary files as Base64-encoded chunks.

    # --------------------------------------------------------

    hashLen: # [number; minimum: 1] Hash length - Length of file header bytes to use in hash for unique file identification
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  tcp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to establish a connection
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    socketIdleTimeout: # [number] Socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] Forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports proxy protocol v1 or v2
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    enableHeader: # [boolean] Enable header - Client will pass the header record with every new connection. The header can contain an authToken, and an object with a list of fields and values to add to every event. These fields can be used to simplify Event Breaker selection, routing, etc. Header has this format, and must be followed by a newline: { "authToken" : "myToken", "fields": { "field1": "value1", "field2": "value2" } }

    # -------------- if enableHeader is true ---------------

    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate

    # --------------------------------------------------------

    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  appscope_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to establish a connection
    maxActiveCxn: # [number] Active connection limit - Maximum number of active connections allowed per Worker Process. Use 0 for unlimited.
    socketIdleTimeout: # [number] Socket idle timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. After this time, the connection will be closed. Leave at 0 for no inactive socket monitoring.
    socketEndingMaxWait: # [number] Forced socket termination timeout (seconds) - How long the server will wait after initiating a closure for a client to close its end of the connection. If the client doesn't close the connection within this time, the server will forcefully terminate the socket to prevent resource leaks and ensure efficient connection cleanup and system stability. Leave at 0 for no inactive socket monitoring.
    socketMaxLifespan: # [number] Socket max lifespan (seconds) - The maximum duration a socket can remain open, even if active. This helps manage resources and mitigate issues caused by TCP pinning. Set to 0 to disable.
    enableProxyHeader: # [boolean] Enable proxy protocol - Enable if the connection is proxied by a device that supports proxy protocol v1 or v2
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    enableUnixPath: # [boolean] UNIX domain socket - Toggle to Yes to specify a file-backed UNIX domain socket connection, instead of a network host and port.

    # -------------- if enableUnixPath is false ---------------

    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] Port - Port to listen on
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if enableUnixPath is true ---------------

    unixSocketPath: # [string] UNIX socket path - Path to the UNIX domain socket to listen on.
    unixSocketPerms: # [string,number] UNIX socket permissions - Permissions to set for socket e.g., 777. If empty, falls back to the runtime user's default permissions.

    # --------------------------------------------------------

    filter: # [object] 
      allow: # [array] Rules - Specify processes that AppScope should be loaded into, and the config to use.
        - procname: # [string] Process name - Specify the name of a process or family of processes.
          arg: # [string] Process argument - Specify a string to substring-match against process command-line.
          config: # [string] AppScope config - Choose a config to apply to processes that match the process name and/or argument.
      transportURL: # [string] Transport override - To override the UNIX domain socket or address/port specified in General Settings (while leaving Authentication settings as is), enter a URL.
    persistence: # [object] Persistence
      enable: # [boolean] Enable disk spooling - Spool events and metrics on disk for Cribl Edge and Search

      # -------------- if enable is true ---------------

      timeWindow: # [string] Bucket time span - Time span for each file bucket
      maxDataSize: # [string] Data size limit - Maximum disk space allowed to be consumed (examples: 420MB, 4GB). When limit is reached, older data will be deleted.
      maxDataTime: # [string] Data age limit - Maximum amount of time to retain data (examples: 2h, 4d). When limit is reached, older data will be deleted.
      compress: # [string] Data compression format
      destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/appscope

      # --------------------------------------------------------

    authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
    authToken: # [string] Auth token - Shared secret to be provided by any client (in authToken header field). If empty, unauthorized access is permitted.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  wef_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authMethod: # [string] Authentication method - How to authenticate incoming client connections

    # -------------- if authMethod is clientCert ---------------

    tls: # [object] mTLS settings
      disabled: # [boolean] Disabled - Enable TLS
      rejectUnauthorized: # [boolean] Validate client certs - Required for WEF certificate authentication
      requestCert: # [boolean] Authenticate client - Required for WEF certificate authentication
      certificateName: # [string] Certificate - Name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] [required] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] [required] CA certificate path - Server path containing CA certificates (in PEM format) to use. Can reference $ENV_VARS. If multiple certificates are present in a .pem, each must directly certify the one preceding it.
      commonNameRegex: # [string] Common name - Regex matching allowable common names in peer certificates' subject attribute
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version
      ocspCheck: # [boolean] Verify certificate via OCSP - Enable OCSP check of certificate

      # -------------- if ocspCheck is true ---------------

      ocspCheckFailClose: # [boolean] Strict validation - If enabled, checks will fail on any OCSP error. Otherwise, checks will fail only when a certificate is revoked, ignoring other errors.

      # --------------------------------------------------------

    caFingerprint: # [string] CA fingerprint override - SHA1 fingerprint expected by the client, if it does not match the first certificate in the configured CA chain
    allowMachineIdMismatch: # [boolean] Allow MachineID mismatch - Allow events to be ingested even if their MachineID does not match the client certificate CN
    logFingerprintMismatch: # [boolean] Log CA fingerprint mismatch warning - Log a warning if the client certificate authority (CA) fingerprint does not match the expected value. A mismatch prevents Cribl from receiving events from the Windows Event Forwarder.

    # --------------------------------------------------------


    # -------------- if authMethod is kerberos ---------------

    keytab: # [string] Keytab location - Path to the keytab file containing the service principal credentials. Cribl Stream will use `/etc/krb5.keytab` if not provided.
    principal: # [string] Service principal name - Kerberos principal used for authentication, typically in the form HTTP/<hostname>@<REALM>

    # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Preserve the client’s original IP address in the __srcIpPort field when connecting through an HTTP proxy that supports the X-Forwarded-For header. This does not apply to TCP-layer Proxy Protocol v1/v2.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events in the __headers field
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    enableHealthCheck: # [boolean] Health check endpoint - Expose the /cribl_health endpoint, which returns 200 OK when this Source is healthy
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    subscriptions: # [array] [required] Subscriptions - Subscriptions to events on forwarding endpoints
      - subscriptionName: # [string] Subscription name
        version: # [string] Version - Version UUID for this subscription. If any subscription parameters are modified, this value will change.
        contentFormat: # [string] Format - Content format in which the endpoint should deliver events
        heartbeatInterval: # [number; minimum: 1] Heartbeat - Maximum time (in seconds) between endpoint checkins before considering it unavailable
        batchTimeout: # [number] Batch timeout - Interval (in seconds) over which the endpoint should collect events before sending them to Stream
        readExistingEvents: # [boolean] Read existing events - Newly subscribed endpoints will send previously existing events. Disable to receive new events only.
        sendBookmarks: # [boolean] Use bookmarks - Keep track of which events have been received, resuming from that point after a re-subscription. This setting takes precedence over 'Read existing events'. See [Cribl Docs](https://docs.cribl.io/stream/sources-wef/#subscriptions) for more details.
        compress: # [boolean] Compression - Receive compressed events from the source
        targets: # [array of strings] Targets - The DNS names of the endpoints that should forward these events. You may use wildcards, such as *.mydomain.com
        locale: # [string] Locale - The RFC-3066 locale the Windows clients should use when sending events. Defaults to "en-US".
        querySelector: # [string] Query builder mode
        metadata: # [array] Fields - Fields to add to events ingested under this subscription
        - name: # [string] Field Name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  win_event_logs_input: # [object] 
    type: # [string] Input Type
    logNames: # [array of strings] Event logs - Enter the event logs to collect. Run "Get-WinEvent -ListLog *" in PowerShell to see the available logs.
    readMode: # [string] Read mode - Read all stored and future event logs, or only future events
    eventFormat: # [string] Event format - Format of individual events
    disableNativeModule: # [boolean] Use Windows Tools - Enable to use built-in tools (PowerShell for JSON, wevtutil for XML) to collect event logs instead of native API (default) [Learn more](https://docs.cribl.io/edge/sources-windows-event-logs/#advanced-settings)
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between checking for new entries (Applicable for pre-4.8.0 nodes that use Windows Tools)
    batchSize: # [number; minimum: 1] Batch size - The maximum number of events to read in one polling interval. A batch size higher than 500 can cause delays when pulling from multiple event logs. (Applicable for pre-4.8.0 nodes that use Windows Tools)
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    maxEventBytes: # [number; minimum: 1, maximum: 134217728] Max event bytes - The maximum number of bytes in an event before it is flushed to the pipelines
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  raw_udp_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    maxBufferSize: # [number] Buffer size limit (events) - Maximum number of events to buffer when downstream is blocking.
    ipWhitelistRegex: # [string] IP allowlist regex - Regex matching IP addresses that are allowed to send data
    singleMsgUdpPackets: # [boolean] Single msg per UDP - If true, each UDP packet is assumed to contain a single message. If false, each UDP packet is assumed to contain multiple messages, separated by newlines.
    ingestRawBytes: # [boolean] Ingest raw bytes - If true, a __rawBytes field will be added to each event containing the raw bytes of the datagram.
    udpSocketRxBufSize: # [number; minimum: 256, maximum: 4294967295] UDP socket buffer size (bytes) - Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Caution: Increasing this value will affect OS memory utilization.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  journal_files_input: # [object] 
    type: # [string] Input Type
    path: # [string] Search path - Directory path to search for journals. Environment variables will be resolved, e.g. $CRIBL_EDGE_FS_ROOT/var/log/journal/$MACHINE_ID.
    interval: # [number; minimum: 1] Polling interval - Time, in seconds, between scanning for journals. 
    journals: # [array of strings] [required] Journal allowlist - The full path of discovered journals are matched against this wildcard list.
    rules: # [array] Filter Rules - Add rules to decide which journal objects to allow. Events are generated if no rules are given or if all the rules' expressions evaluate to true.
      - filter: # [string] Filter Expression - JavaScript expression applied to Journal objects. Return 'true' to include it.
        description: # [string] Description - Optional description of this rule's purpose
    currentBoot: # [boolean] Current boot only - Skip log messages that are not part of the current boot session.
    maxAgeDur: # [string] Age duration limit - The maximum log message age, in duration form (e.g,: 60s, 4h, 3d, 1w).  Default of no value will apply no max age filters.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  wiz_input: # [object] 
    type: # [string] Input Type
    endpoint: # [string] GraphQL endpoint - The Wiz GraphQL API endpoint. Example: https://api.us1.app.wiz.io/graphql
    authUrl: # [string] [required] Authentication URL - The authentication URL to generate an OAuth token
    authAudienceOverride: # [string] Authentication audience - The audience to use when requesting an OAuth token for a custom auth URL. When not specified, `wiz-api` will be used.
    clientId: # [string] [required] Client ID - The client ID of the Wiz application
    contentConfig: # [array] [required] Content types
      - contentType: # [string] Content name - The name of the Wiz query
        contentDescription: # [string] Description
        enabled: # [boolean] Enable content
    requestTimeout: # [number; maximum: 2400] Request timeout (seconds) - HTTP request inactivity timeout. Use 0 to disable.
    keepAliveTime: # [number; minimum: 10] Keep alive time (seconds) - How often workers should check in with the scheduler to keep job subscription alive
    maxMissedKeepAlives: # [number; minimum: 2] Worker timeout (periods) - The number of Keep Alive Time periods before an inactive worker will have its job subscription revoked.
    ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
    ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    retryRules: # [object] 
      type: # [string] Retry type - The algorithm to use when performing HTTP retries

      # -------------- if type is static ---------------

      codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
      interval: # [number; maximum: 20000] Wait (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------


      # -------------- if type is backoff ---------------

      interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
      limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
      multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
      codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
      enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
      retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
      retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

      # --------------------------------------------------------

    authType: # [string] Authentication method - Enter client secret directly, or select a stored secret
    clientSecret: # [string] Client secret - The client secret of the Wiz application

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Client Secret (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  netflow_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. For IPv4 (all addresses), use the default '0.0.0.0'. For IPv6, enter '::' (all addresses) or specify an IP address.
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    enablePassThrough: # [boolean] Enable pass-through - Allow forwarding of events to a NetFlow destination. Enabling this feature will generate an extra event containing __netflowRaw which can be routed to a NetFlow destination. Note that these events will not count against ingest quota.
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist.
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    udpSocketRxBufSize: # [number; minimum: 256, maximum: 4294967295] UDP socket buffer size (bytes) - Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Caution: Increasing this value will affect OS memory utilization.
    templateCacheMinutes: # [number; minimum: 1, maximum: 3600] Template cache minutes - Specifies how many minutes NetFlow v9 templates are cached before being discarded if not refreshed. Adjust based on your network's template update frequency to optimize performance and memory usage.
    v5Enabled: # [boolean] V5 - Accept messages in Netflow V5 format.
    v9Enabled: # [boolean] V9 - Accept messages in Netflow V9 format.
    ipfixEnabled: # [boolean] IPFIX - Accept messages in IPFIX format.
    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  security_lake_input: # [object] 
    type: # [string] Input Type
    queueName: # [string] Queue - The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. Example: 'https://host:port/myQueueName'. Value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. Example referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    fileFilter: # [string] Filename filter - Regex matching file names to download and process. Defaults to: .*
    awsAccountId: # [string] AWS account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key
    region: # [string] Region - AWS Region where the S3 bucket and SQS queue are located. Required, unless the Queue entry is a URL or ARN that includes a Region.
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to the AWS Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
    reuseConnections: # [boolean] Reuse connections - Reuse connections between requests, which can improve performance
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA, such as self-signed certificates
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    maxMessages: # [number; minimum: 1, maximum: 10] Message limit - The maximum number of messages SQS should return in a poll request. Amazon SQS never returns more messages than this value (however, fewer messages might be returned). Valid values: 1 to 10.
    visibilityTimeout: # [number; maximum: 43200] Visibility timeout seconds - After messages are retrieved by a ReceiveMessage request, Cribl Stream will hide them from subsequent retrieve requests for at least this duration. You can set this as high as 43200 sec. (12 hours).
    numReceivers: # [number; minimum: 1, maximum: 100] Number of receivers - How many receiver processes to run. The higher the number, the better the throughput - at the expense of CPU overhead.
    socketTimeout: # [number; minimum: 1, maximum: 43200] Socket timeout - Socket inactivity timeout (in seconds). Increase this value if timeouts occur due to backpressure.
    skipOnError: # [boolean] Skip file on error - Skip files that trigger a processing error. Disabled by default, which allows retries after processing errors.
    enableAssumeRole: # [boolean] Enable for Amazon S3 - Use Assume Role credentials to access Amazon S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the assumed role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
    enableSQSAssumeRole: # [boolean] Enable for Amazon SQS - Use Assume Role credentials when accessing Amazon SQS
    preprocess: # [object] 
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments to be added to the custom command

      # --------------------------------------------------------

    metadata: # [array] Fields - Fields to add to events from this input
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
    parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will stop if a chunk cannot be downloaded within the time specified.
    checkpointing: # [object] 
      enabled: # [boolean] Enable checkpointing - Resume processing files after an interruption

      # -------------- if enabled is true ---------------

      retries: # [number; maximum: 100] Retries - The number of times to retry processing when a processing error occurs. If Skip file on error is enabled, this setting is ignored.

      # --------------------------------------------------------

    pollTimeout: # [number; minimum: 1, maximum: 20] Poll timeout (secs) - How long to wait for events before trying polling again. The lower the number the higher the AWS bill. The higher the number the longer it will take for the source to react to configuration changes and system restarts.
    encoding: # [string] Encoding - Character encoding to use when parsing ingested data. When not set, Cribl Stream will default to UTF-8 but may incorrectly interpret multi-byte characters.
    description: # [string] Description
    disabled: # [boolean] Disabled
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
  zscaler_hec_input: # [object] 
    type: # [string] Input Type
    disabled: # [boolean] Disabled
    host: # [string] Address - Address to bind on. Defaults to 0.0.0.0 (all addresses).
    port: # [number; maximum: 65535] [required] Port - Port to listen on
    authTokens: # [array] Auth tokens - Shared secrets to be provided by any client (Authorization: <token>). If empty, unauthorized access is permitted.
      - authType: # [string] Authentication method - Select Manual to enter an auth token directly, or select Secret to use a text secret to authenticate
        enabled: # [boolean] Enable token
        description: # [string] Description
        allowedIndexesAtToken: # [array of strings] Allowed indexes - Enter the values you want to allow in the HEC event index field at the token level. Supports wildcards. To skip validation, leave blank.
        metadata: # [array] Fields - Fields to add to events referencing this token
        - name: # [string] Field Name
          value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    tls: # [object] TLS settings (server side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      certificateName: # [string] Certificate - The name of the predefined certificate
      privKeyPath: # [string] Private key path - Path on server containing the private key to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key
      certPath: # [string] Certificate path - Path on server containing certificates to use. PEM format. Can reference $ENV_VARS.
      caPath: # [string] CA certificate path - Path on server containing CA certificates to use. PEM format. Can reference $ENV_VARS.
      requestCert: # [boolean] Authenticate client (mutual auth) - Require clients to present their certificates. Used to perform client authentication using SSL certs.
      minVersion: # [string] Minimum TLS version
      maxVersion: # [string] Maximum TLS version

      # --------------------------------------------------------

    maxActiveReq: # [number] Active request limit - Maximum number of active requests allowed per Worker Process. Set to 0 for unlimited. Caution: Increasing the limit above the default value, or setting it to unlimited, may degrade performance and reduce throughput.
    maxRequestsPerSocket: # [integer] Requests-per-socket limit - Maximum number of requests per socket before Cribl Stream instructs the client to close the connection. Default is 0 (unlimited).
    enableProxyHeader: # [boolean] Show originating IP - Extract the client IP and port from PROXY protocol v1/v2. When enabled, the X-Forwarded-For header is ignored. Disable to use the X-Forwarded-For header for client IP extraction.
    captureHeaders: # [boolean] Capture request headers - Add request headers to events, in the __headers field
    activityLogSampleRate: # [number; minimum: 1] Activity log sample rate - How often request activity is logged at the `info` level. A value of 1 would log every request, 10 every 10th request, etc.
    requestTimeout: # [number] Request timeout (seconds) - How long to wait for an incoming request to complete before aborting it. Use 0 to disable.
    socketTimeout: # [number] Socket timeout (seconds) - How long Cribl Stream should wait before assuming that an inactive socket has timed out. To wait forever, set to 0.
    keepAliveTimeout: # [number; minimum: 1, maximum: 600] Keep-alive timeout (seconds) - After the last response is sent, Cribl Stream will wait this long for additional data before closing the socket connection. Minimum 1 second, maximum 600 seconds (10 minutes).
    ipAllowlistRegex: # [string] IP allowlist regex - Messages from matched IP addresses will be processed, unless also matched by the denylist
    ipDenylistRegex: # [string] IP denylist regex - Messages from matched IP addresses will be ignored. This takes precedence over the allowlist.
    hecAPI: # [string] [required] HEC endpoint - Absolute path on which to listen for the Zscaler HTTP Event Collector API requests. This input supports the /event endpoint.
    metadata: # [array] Fields - Fields to add to every event. May be overridden by fields added at the token or request level.
      - name: # [string] Field Name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    allowedIndexes: # [array of strings] Allowed indexes - List values allowed in HEC event index field. Leave blank to skip validation. Supports wildcards. The values here can expand index validation at the token level.
    hecAcks: # [boolean] Zscaler HEC Acks - Whether to enable Zscaler HEC acknowledgements
    accessControlAllowOrigin: # [array of strings] CORS allowed origins - Optionally, list HTTP origins to which Cribl Stream should send CORS (cross-origin resource sharing) Access-Control-Allow-* headers. Supports wildcards.
    accessControlAllowHeaders: # [array of strings] CORS allowed headers - Optionally, list HTTP headers that Cribl Stream will send to allowed origins as "Access-Control-Allow-Headers" in a CORS preflight response. Use "*" to allow all headers.
    emitTokenMetrics: # [boolean] Emit per-token request metrics - Enable to emit per-token (<prefix>.http.perToken) and summary (<prefix>.http.summary) request metrics
    description: # [string] Description
    pipeline: # [string] Pipeline - Pipeline to process data from this Source before sending it through the Routes
    sendToRoutes: # [boolean]  - Select whether to send data to Routes, or directly to Destinations.

    # -------------- if sendToRoutes is false ---------------

    connections: # [array] Use QuickConnect - Direct connections to Destinations, and optionally via a Pipeline or a Pack
      - pipeline: # [string] Pipeline or Pack
        output: # [string] Destination

    # --------------------------------------------------------

    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    pqEnabled: # [boolean] Enable persistent queue - Use a disk queue to minimize data loss when connected services block. See [Cribl Docs](https://docs.cribl.io/stream/persistent-queues) for PQ defaults (Cribl-managed Cloud Workers) and configuration options (on-prem and hybrid Workers).

    # -------------- if pqEnabled is true ---------------

    pq: # [object] 
      mode: # [string] Mode - With Smart mode, PQ will write events to the filesystem only when it detects backpressure from the processing engine. With Always On mode, PQ will always write events directly to the queue before forwarding them to the processing engine.
      maxBufferSize: # [number; minimum: 42] Buffer size limit - The maximum number of events to hold in memory before writing the events to disk
      commitFrequency: # [number; minimum: 1] Commit frequency - The number of events to send downstream before committing that Stream has read them
      maxFileSize: # [string] File size limit - The maximum size to store in each queue file before closing and optionally compressing. Enter a numeral with units of KB, MB, etc.
      maxSize: # [string] Queue size limit - The maximum disk space that the queue can consume (as an average per Worker Process) before queueing stops. Enter a numeral with units of KB, MB, etc.
      path: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/inputs/<input-id>
      compress: # [string] Compression - Codec to use to compress the persisted data

    # --------------------------------------------------------

    streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
```
