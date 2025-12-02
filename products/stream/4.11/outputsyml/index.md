# outputs.yml


`outputs.yml` contains configuration settings for Cribl Stream [Destinations](destinations).

```yaml {title="$CRIBL_HOME/default/cribl/outputs.yml"}
outputs: # [object] 
  default_output: # [object] 
    type: # [string] Output Type
    defaultId: # [string,null] Default Output ID - ID of the default output. This will be used whenever a nonexistent/deleted output is referenced.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  webhook_output: # [object] 
    type: # [string] Output Type
    url: # [string] URL - URL to send events to.
    method: # [string] Method - The method to use when sending events. Defaults to POST.
    format: # [string] Format - Specifies how to format events before sending out. Defaults to NDJSON.

    # -------------- if format is custom ---------------

    customSourceExpression: # [string] Source expression - Expression to evaluate as event. E.g., `${fieldA}, ${fieldB}`. Defaults to __httpOut (i.e. value of field __httpOut).
    customDropWhenNull: # [boolean] Drop when null - Whether or not to drop events when the source expression evaluates to null.
    customEventDelimiter: # [string] Event delimiter - Delimiter string to insert between individual events. Defaults to newline character.
    customContentType: # [string] Content type - Content type to use for request. Defaults to application/x-ndjson. Any content types set in Advanced Settings > Extra HTTP headers will override this entry.

    # --------------------------------------------------------

    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication type - The authentication method to use for the HTTP request. Defaults to None.

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

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

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  devnull_output: # [object] 
    type: # [string] Output Type
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  syslog_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Protocol - The network protocol to use for sending out syslog messages

    # -------------- if protocol is tcp ---------------

    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
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

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # --------------------------------------------------------


    # -------------- if protocol is udp ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number] Port - The port to connect to on the provided host
    maxRecordSize: # [number] Max Record Size - Maximum size of syslog messages. If max record size is > than MTU then UDP packets can be fragmented across, set this value  <= MTU to avoid fragmentation.

    # --------------------------------------------------------

    facility: # [number] Facility - Default value for message facility, will be overwritten by value of __facility if set. Defaults to user.
    severity: # [number] Severity - Default value for message severity, will be overwritten by value of __severity if set. Defaults to notice.
    appName: # [string] App Name - Default value for application name, will be overwritten by value of __appname if set. Defaults to Cribl.
    messageFormat: # [string] Message Format - The syslog message format depending on the receiver's support
    timestampFormat: # [string] Timestamp Format - Timestamp format to use when serializing event's time field
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    octetCountFraming: # [boolean] Octet count framing - When enabled, messages will be prefixed with the byte count of the message. Otherwise, no prefix will be set, and the message will be appended with a \n.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_output: # [object] 
    type: # [string] Output Type
    host: # [string] Address - The hostname of the receiver
    port: # [number] [required] Port - The port to connect to on the provided host
    nestedFields: # [string] Nested field serialization - Specifies how to serialize nested fields into index-time fields.
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
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

    enableMultiMetrics: # [boolean] Output multiple metrics - Output metrics in multiple-metric format in a single event. Supported in Splunk 8.0 and above.
    enableACK: # [boolean] Minimize in-flight data loss - Check if indexer is shutting down and stop sending data. This helps minimize data loss during shutdown.
    maxS2Sversion: # [string] Max S2S version - The highest S2S protocol version to advertise during handshake.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Shared secret token to use when establishing a connection to a Splunk indexer.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_lb_output: # [object] 
    type: # [string] Output Type
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.
    maxConcurrentSenders: # [number] Max connections - Maximum number of concurrent connections (per worker process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.
    nestedFields: # [string] Nested field serialization - Specifies how to serialize nested fields into index-time fields.
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
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

    enableMultiMetrics: # [boolean] Output multiple metrics - Output metrics in multiple-metric format in a single event. Supported in Splunk 8.0 and above.
    enableACK: # [boolean] Minimize in-flight data loss - Check if indexer is shutting down and stop sending data. This helps minimize data loss during shutdown.
    maxS2Sversion: # [string] Max S2S version - The highest S2S protocol version to advertise during handshake.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    indexerDiscovery: # [boolean] Indexer Discovery - Automatically discover indexers in indexer clustering environment.

    # -------------- if indexerDiscovery is true ---------------

    indexerDiscoveryConfigs: # [object]  - List of configurations to set up indexer discovery in Splunk Indexer clustering environment.
      site: # [string] [required] Site - Clustering site of the indexers from where indexers need to be discovered. In case of single site cluster, it defaults to 'default' site.
      masterUri: # [string] Cluster Manager URI - Full URI of Splunk cluster Manager (scheme://host:port). E.g.: https://managerAddress:8089
      refreshIntervalSec: # [number] [required] Refresh Period - Time interval in seconds between two consecutive indexer list fetches from cluster Manager.
      authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
      authToken: # [string] Auth token - Authentication token required to authenticate to cluster Manager for indexer discovery.

      # -------------- if authType is manual ---------------


      # --------------------------------------------------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # -------------- if authType is secret ---------------


      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if indexerDiscovery is false ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    hosts: # [array] Destinations - Set of Splunk indexers to load-balance data to.
      - host: # [string] Address - The hostname of the receiver.
        port: # [number] Port - The port to connect to on the provided host.
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS.
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (iff not an IP); otherwise, to the global TLS settings.
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.
    maxConcurrentSenders: # [number] Max connections - Maximum number of concurrent connections (per worker process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Shared secret token to use when establishing a connection to a Splunk indexer.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  splunk_hec_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    url: # [string] Splunk HEC Endpoint - URL to a Splunk HEC endpoint to send events to, e.g., http://localhost:8088/services/collector/event
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    urls: # [array] Splunk HEC Endpoints
      - url: # [string] HEC Endpoint - URL to a Splunk HEC endpoint to send events to, e.g., http://localhost:8088/services/collector/event
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.

    # --------------------------------------------------------

    nextQueue: # [string] Next Processing Queue - Which Splunk processing queue to send the events after HEC processing
    tcpRouting: # [string] Default _TCP_ROUTING - Set the value of _TCP_ROUTING for events that does not have _ctrl._TCP_ROUTING set
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    enableMultiMetrics: # [boolean] Output multi-metrics - Output metrics in multiple-metric format, supported in Splunk 8.0 and above to allow multiple metrics in a single event.
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # -------------- if authType is manual ---------------

    token: # [string] HEC Auth token - Splunk HEC authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] HEC Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  tcpjson_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number] Port - The port to connect to on the provided host

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    hosts: # [array] Destinations - Set of hosts to load-balance data to.
      - host: # [string] Address - The hostname of the receiver.
        port: # [number] Port - The port to connect to on the provided host.
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS.
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (iff not an IP); otherwise, to the global TLS settings.
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.
    maxConcurrentSenders: # [number] Max connections - Maximum number of concurrent connections (per worker process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
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

    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tokenTTLMinutes: # [number] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Optional authentication token to include as part of the connection header

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  wavefront_output: # [object] 
    type: # [string] Output Type
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # -------------- if authType is manual ---------------

    token: # [string] Auth token - WaveFront API authentication token (see [here](https://docs.wavefront.com/wavefront_api.html#generating-an-api-token))

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    domain: # [string] Domain name - WaveFront domain name, e.g. "longboard"
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  signalfx_output: # [object] 
    type: # [string] Output Type
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # -------------- if authType is manual ---------------

    token: # [string] Auth token - SignalFx API access token (see [here](https://docs.signalfx.com/en/latest/admin-guide/tokens.html#working-with-access-tokens))

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    realm: # [string] Realm - SignalFx realm name, e.g. "us0"
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  filesystem_output: # [object] 
    type: # [string] Output Type
    destPath: # [string] Output location - Final destination for the output files
    stagePath: # [string] Staging location - Filesystem location in which to buffer files before compressing and moving to final destination. Use performant stable storage.
    addIdToStagePath: # [boolean] Add output ID - Append output's ID to staging location.
    removeEmptyDirs: # [boolean] Remove staging dirs - Remove empty staging directories after moving files.

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number] Staging cleanup period - How often (secs) to clean-up empty directories when 'Remove Staging Dirs' is enabled.

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JS expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value â if present â otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data.

    # -------------- if format is json ---------------

    compress: # [string] Compress - Choose data compression format to apply before moving files to final destination.

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    parquetSchema: # [string] Parquet schema - Select a Parquet schema. New schemas can be uploaded under Processing > Knowledge > Parquet Schemas
    parquetRowGroupSize: # [string] Row group size - Ideal memory size for row group segments. E.g., 128MB or 1GB. Affects memory use when writing. Imposes a target, not a strict limit; row groups final size may be larger or smaller.
    parquetPageSize: # [string] Page size - Ideal memory size for page segments. E.g., 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; pages final size may be larger or smaller.
    spacer: # [null] 
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented.
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that not all reader implemtations support Data page V2.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Output up to 20 unique rows that were skipped due to data format mismatch. Must have Logging set to Debug to see output.

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant).
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`)
    maxFileSizeMB: # [number] Max file size (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number] Max file open time (Sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number] Max file idle time (Sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number] Max open files - Maximum number of files to keep open concurrently. When over, the oldest open files will be closed and moved to final output location.
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  s3_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] S3 bucket name - Name of the destination S3 bucket. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `myBucket-${C.vars.myVar}`.
    region: # [string] Region - Region where the S3 bucket is located.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    endpoint: # [string] Endpoint - S3 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to S3-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    enableAssumeRole: # [boolean] Enable for S3 - Use Assume Role credentials to access S3
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant stable storage.
    addIdToStagePath: # [boolean] Add output ID - Append output's ID to staging location.
    removeEmptyDirs: # [boolean] Remove staging dirs - Remove empty staging directories after moving files.

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number] Staging cleanup period - How often (secs) to clean-up empty directories when 'Remove Staging Dirs' is enabled.

    # --------------------------------------------------------

    destPath: # [string] [required] Key prefix - Prefix to append to files before uploading. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `myKeyPrefix-${C.vars.myVar}`.
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects.
    storageClass: # [string] Storage class - Storage class to select for uploaded objects.
    serverSideEncryption: # [string] Server side encryption - Server-side encryption for uploaded objects.

    # -------------- if serverSideEncryption is aws:kms ---------------

    kmsKeyId: # [string] KMS key ID - ID or ARN of the KMS customer managed key to use for encryption

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JS expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value â if present â otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data.

    # -------------- if format is json ---------------

    compress: # [string] Compress - Choose data compression format to apply before moving files to final destination.

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    parquetSchema: # [string] Parquet schema - Select a Parquet schema. New schemas can be uploaded under Processing > Knowledge > Parquet Schemas
    parquetRowGroupSize: # [string] Row group size - Ideal memory size for row group segments. E.g., 128MB or 1GB. Affects memory use when writing. Imposes a target, not a strict limit; row groups final size may be larger or smaller.
    parquetPageSize: # [string] Page size - Ideal memory size for page segments. E.g., 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; pages final size may be larger or smaller.
    spacer: # [null] 
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented.
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that not all reader implemtations support Data page V2.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Output up to 20 unique rows that were skipped due to data format mismatch. Must have Logging set to Debug to see output.

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant).
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`)
    maxFileSizeMB: # [number] Max file size (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number] Max file open time (Sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number] Max file idle time (Sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number] Max open files - Maximum number of files to keep open concurrently. When over, the oldest open files will be closed and moved to final output location.
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  azure_blob_output: # [object] 
    type: # [string] Output Type
    containerName: # [string] Container name - A container organizes a set of blobs, similar to a directory in a file system.
    createContainer: # [boolean] Create container - Creates the configured container in Azure Blob Storage if it does not already exist.
    destPath: # [string] [required] Blob prefix - Root directory prepended to path before uploading.
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant stable storage.
    addIdToStagePath: # [boolean] Add output ID - Append output's ID to staging location.
    removeEmptyDirs: # [boolean] Remove staging dirs - Remove empty staging directories after moving files.

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number] Staging cleanup period - How often (secs) to clean-up empty directories when 'Remove Staging Dirs' is enabled.

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JS expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value â if present â otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data.

    # -------------- if format is json ---------------

    compress: # [string] Compress - Choose data compression format to apply before moving files to final destination.

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    parquetSchema: # [string] Parquet schema - Select a Parquet schema. New schemas can be uploaded under Processing > Knowledge > Parquet Schemas
    parquetRowGroupSize: # [string] Row group size - Ideal memory size for row group segments. E.g., 128MB or 1GB. Affects memory use when writing. Imposes a target, not a strict limit; row groups final size may be larger or smaller.
    parquetPageSize: # [string] Page size - Ideal memory size for page segments. E.g., 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; pages final size may be larger or smaller.
    spacer: # [null] 
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented.
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that not all reader implemtations support Data page V2.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Output up to 20 unique rows that were skipped due to data format mismatch. Must have Logging set to Debug to see output.

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant).
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`)
    maxFileSizeMB: # [number] Max file size (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number] Max file open time (Sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number] Max file idle time (Sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number] Max open files - Maximum number of files to keep open concurrently. When over, the oldest open files will be closed and moved to final output location.
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    authType: # [string] Authentication method - Enter connection string directly, or select a stored secret
    connectionString: # [string] Connection string - Enter your Azure Storage account connection string. If left blank, Stream will fall back to env.AZURE_STORAGE_CONNECTION_STRING.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Connection string (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  azure_logs_output: # [object] 
    type: # [string] Output Type
    logType: # [string] Log Type - The Log Type of events sent to this LogAnalytics workspace. Can be overwritten by event field __logType.
    resourceId: # [string] Resource ID - Optional Resource ID of the Azure resource the data should be associated with, can be overridden by event field __resourceId. This populates the _ResourceId property and allows the data to be included in resource-centric queries. If this field isn't specified, the data will not be included in resource-centric queries.
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter workspace ID and workspace key directly, or select a stored secret
    workspaceId: # [string] Workspace ID - Azure Log Analytics Workspace ID. See Azure Dashboard WorkspaceÂ > Advanced settings.
    workspaceKey: # [string] Workspace key - Azure Log Analytics Workspace Primary or Secondary Shared Key. See Azure Dashboard WorkspaceÂ > Advanced settings.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    keypairSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  kinesis_output: # [object] 
    type: # [string] Output Type
    streamName: # [string] Stream Name - Kinesis stream name to send events to.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

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
    concurrency: # [number] Put request concurrency - Maximum number of ongoing put requests before blocking.
    maxRecordSizeKB: # [number] Max record size (KB, uncompressed) - Maximum size (KB) of each individual record before compression. For non-compressible data 1MB is the max recommended size
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  honeycomb_output: # [object] 
    type: # [string] Output Type
    dataset: # [string] Dataset name - Name of the dataset to send events to â e.g., observability
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    team: # [string] API key - Team API key where the dataset belongs

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  azure_eventhub_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Brokers - List of Event Hubs Kafka brokers to connect to, eg. yourdomain.servicebus.windows.net:9093. The hostname can be found in the host portion of the primary or secondary connection string in Shared Access Policies.
    topic: # [string] [required] Event Hub Name - The name of the Event Hub (a.k.a. Kafka Topic) to publish events. Can be overwritten using field __topicOut.
    ack: # [number] Acknowledgments - Control the number of required acknowledgments
    format: # [string] Record data format - Format to use to serialize events before writing to the Event Hubs Kafka brokers.
    maxRecordSizeKB: # [number] Max record size (KB, uncompressed) - Maximum size (KB) of each record batch before compression. Setting should be < message.max.bytes settings in Event Hubs brokers.
    flushEventCount: # [number] Max events per batch - Maximum number of events in a batch before forcing a flush.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
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

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  google_chronicle_output: # [object] 
    type: # [string] Output Type
    authenticationMethod: # [string] Authentication Method

    # -------------- if authenticationMethod is manual ---------------

    apiKey: # [string] API key - Organization's API key in Google Chronicle

    # --------------------------------------------------------


    # -------------- if authenticationMethod is secret ---------------

    apiKeySecret: # [string] API key (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    logType: # [string] Log type - Log type value to send to Chronicle. Can be overwritten by event field __logType.
    logTextField: # [string] Log text field - Name of the event field that contains the log text to send. If not specified, Stream sends a JSON representation of the whole event.
    logFormatType: # [string] Send events as
    region: # [string] Region - Regional endpoint to send events to
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  google_cloud_storage_output: # [object] 
    type: # [string] Output Type
    bucket: # [string] Bucket name - Name of the destination Bucket. This value can be a constant or a JavaScript expression that can only be evaluated at init time. E.g. referencing a Global Variable: `myBucket-${C.vars.myVar}`.
    region: # [string] [required] Region - Region where the bucket is located.
    endpoint: # [string] [required] Endpoint - Google Cloud Storage service endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing Google Cloud Storage requests.
    awsAuthenticationMethod: # [string] Authentication method

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - HMAC access Key
    awsSecretKey: # [string] Secret - HMAC secret

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant stable storage.
    destPath: # [string] [required] Key prefix - Prefix to append to files before uploading. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `myKeyPrefix-${C.vars.myVar}`.
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects.
    storageClass: # [string] Storage class - Storage class to select for uploaded objects.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    addIdToStagePath: # [boolean] Add output ID - Append output's ID to staging location.
    removeEmptyDirs: # [boolean] Remove staging dirs - Remove empty staging directories after moving files.

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number] Staging cleanup period - How often (secs) to clean-up empty directories when 'Remove Staging Dirs' is enabled.

    # --------------------------------------------------------

    partitionExpr: # [string] Partitioning expression - JS expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value â if present â otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data.

    # -------------- if format is json ---------------

    compress: # [string] Compress - Choose data compression format to apply before moving files to final destination.

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    parquetSchema: # [string] Parquet schema - Select a Parquet schema. New schemas can be uploaded under Processing > Knowledge > Parquet Schemas
    parquetRowGroupSize: # [string] Row group size - Ideal memory size for row group segments. E.g., 128MB or 1GB. Affects memory use when writing. Imposes a target, not a strict limit; row groups final size may be larger or smaller.
    parquetPageSize: # [string] Page size - Ideal memory size for page segments. E.g., 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; pages final size may be larger or smaller.
    spacer: # [null] 
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented.
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that not all reader implemtations support Data page V2.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Output up to 20 unique rows that were skipped due to data format mismatch. Must have Logging set to Debug to see output.

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant).
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`)
    maxFileSizeMB: # [number] Max file size (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number] Max file open time (Sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number] Max file idle time (Sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number] Max open files - Maximum number of files to keep open concurrently. When over, the oldest open files will be closed and moved to final output location.
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  google_pubsub_output: # [object] 
    type: # [string] Output Type
    topicName: # [string] Topic ID - ID of the topic to send events to.
    createTopic: # [boolean] Create topic - If enabled, create topic if it does not exist.
    orderedDelivery: # [boolean] Ordered delivery - If enabled, send events in the order they were added to the queue. For this to work correctly, the process receiving events must have ordering enabled.
    region: # [string] Region - Region to publish messages to. Select 'default' to allow Google to auto-select the nearest region. When using ordered delivery, the selected region must be allowed by message storage policy.
    googleAuthMethod: # [string] Authentication Method - Google authentication method. Choose Auto to use environment variables PUBSUB_PROJECT and PUBSUB_CREDENTIALS.

    # -------------- if googleAuthMethod is manual ---------------

    serviceAccountCredentials: # [string] Service account credentials - Contents of service account credentials (JSON keys) file downloaded from Google Cloud. To upload a file, click the upload button at this field's upper right. As an alternative, you can use environment variables (see [here](https://googleapis.dev/ruby/google-cloud-pubsub/latest/file.AUTHENTICATION.html)).

    # --------------------------------------------------------


    # -------------- if googleAuthMethod is secret ---------------

    secret: # [string] Service account credentials (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    batchSize: # [number] Batch size - The maximum number of items the Google API should batch before it sends them to the topic.
    batchTimeout: # [number] Batch timeout (ms) - The maximum amount of time, in milliseconds, that the Google API should wait to send a batch (if the Batch size is not reached).
    maxQueueSize: # [number] Max queue size - Maximum number of queued batches before blocking.
    maxRecordSizeKB: # [number] Max batch size (KB) - Maximum size (KB) of batches to send.
    maxInProgress: # [number] Max concurrent requests - The maximum number of in-progress API requests before backpressure is applied.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  kafka_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Brokers - List of Kafka brokers to connect to, e.g., kafkaBrokerHost:9092.
    topic: # [string] [required] Topic - The topic to publish events to. Can be overridden using the __topicOut field.
    ack: # [number] Acknowledgments - Control the number of required acknowledgments.
    format: # [string] Record data format - Format to use to serialize events before writing to Kafka.
    compression: # [string] Compression - Codec to use to compress the data before sending to Kafka.
    maxRecordSizeKB: # [number] Max record size (KB, uncompressed) - Maximum size (KB) of each record batch before compression. Setting should be < message.max.bytes settings in Kafka brokers.
    flushEventCount: # [number] Max events per batch - Maximum number of events in a batch before forcing a flush.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled - Enable Schema Registry

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for access to the Confluent Schema Registry, i.e.: http://localhost:8081
      defaultKeySchemaId: # [number] Default key schema ID - Used when __keySchemaIdOut is not present, to transform key values, leave blank if key transformation is not required by default.
      defaultValueSchemaId: # [number] Default value schema ID - Used when __valueSchemaIdOut is not present, to transform _raw, leave blank if value transformation is not required by default.
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

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  confluent_cloud_output: # [object] 
    type: # [string] Output Type
    brokers: # [array of strings] Brokers - List of Confluent Cloud brokers to connect to, e.g., yourAccount.confluent.cloud:9092.
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

    topic: # [string] [required] Topic - The topic to publish events to. Can be overridden using the __topicOut field.
    ack: # [number] Acknowledgments - Control the number of required acknowledgments.
    format: # [string] Record data format - Format to use to serialize events before writing to Kafka.
    compression: # [string] Compression - Codec to use to compress the data before sending to Kafka.
    maxRecordSizeKB: # [number] Max record size (KB, uncompressed) - Maximum size (KB) of each record batch before compression. Setting should be < message.max.bytes settings in Kafka brokers.
    flushEventCount: # [number] Max events per batch - Maximum number of events in a batch before forcing a flush.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    kafkaSchemaRegistry: # [object] Kafka Schema Registry Authentication
      disabled: # [boolean] Disabled - Enable Schema Registry

      # -------------- if disabled is false ---------------

      schemaRegistryURL: # [string] Schema Registry URL - URL for access to the Confluent Schema Registry, i.e.: http://localhost:8081
      defaultKeySchemaId: # [number] Default key schema ID - Used when __keySchemaIdOut is not present, to transform key values, leave blank if key transformation is not required by default.
      defaultValueSchemaId: # [number] Default value schema ID - Used when __valueSchemaIdOut is not present, to transform _raw, leave blank if value transformation is not required by default.
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

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  elastic_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    url: # [string] Bulk API URL or Cloud ID - Enter Cloud ID or URL to an Elastic cluster to send events to â e.g., http://elastic:9200/_bulk
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    urls: # [array] Bulk API URLs
      - url: # [string] URL - URL to an Elastic node to send events to â e.g., http://elastic:9200/_bulk
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.

    # --------------------------------------------------------

    index: # [string] Index or Data Stream - Index or Data Stream to send events to. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be overwritten by an event's __index field.
    docType: # [string] Type - Document type to use for events. Can be overwritten by an event's __type field
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    extraParams: # [array] Extra Parameters - Extra Parameters.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    auth: # [object] 
      disabled: # [boolean] Authentication Disabled

      # -------------- if disabled is false ---------------

      authType: # [string] Authentication method - Enter credentials directly, or select a stored secret

      # --------------------------------------------------------

    elasticVersion: # [string] Elastic Version - Optional Elasticsearch version, used to format events. If not specified, will auto-discover version.
    elasticPipeline: # [string] Elastic pipeline - Optional Elasticsearch destination pipeline
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  newrelic_output: # [object] 
    type: # [string] Output Type
    region: # [string] Region - Which New Relic region endpoint to use.
    logType: # [string] Log type - Name of the logtype to send with events, e.g.: observability, access_log. The event's 'sourcetype' field (if set) will override this value.
    messageField: # [string] Log message field - Name of field to send as log message value. If not present, event will be serialized and sent as JSON.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - New Relic API key. Can be overridden using __newRelic_apiKey field.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  newrelic_events_output: # [object] 
    type: # [string] Output Type
    region: # [string] [required] Region - Which New Relic region endpoint to use.
    accountId: # [string] Account ID - New Relic account ID
    eventType: # [string] [required] Event type - Default eventType to use when not present in an event. For more information, see [here](https://docs.newrelic.com/docs/telemetry-data-platform/custom-data/custom-events/data-requirements-limits-custom-event-data/#reserved-words).
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - New Relic API key. Can be overridden using __newRelic_apiKey field.

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
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
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication type - InfluxDB authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

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

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cloudwatch_output: # [object] 
    type: # [string] Output Type
    logGroupName: # [string] Log group name - CloudWatch log group to associate events with
    logStreamName: # [string] [required] Log stream prefix - Prefix for CloudWatch log stream name. This prefix will be used to generate a unique log stream name per cribl instance, for example: myStream_myHost_myOutputId
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] [required] Region - Region where the CloudWatchLogs is located
    endpoint: # [string] Endpoint - CloudWatchLogs service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to CloudWatchLogs-compatible endpoint.
    signatureVersion: # [string] Signature version - Signature version to use for signing CloudWatchLogs requests.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    enableAssumeRole: # [boolean] Enable for CloudWatchLogs - Use Assume Role credentials to access CloudWatchLogs
    assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
    assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
    maxQueueSize: # [number] Max queue size - Maximum number of queued batches before blocking
    maxRecordSizeKB: # [number] Max record size (KB, uncompressed) - Maximum size (KB) of each individual record before compression. For non compressible data 1MB is the max recommended size
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  minio_output: # [object] 
    type: # [string] Output Type
    endpoint: # [string] [required] MinIO endpoint - MinIO service url (e.g. http://minioHost:9000)
    bucket: # [string] MinIO bucket name - Name of the destination MinIO bucket. This value can be a constant or a JavaScript expression that can only be evaluated at init time. E.g. referencing a Global Variable: `myBucket-${C.vars.myVar}`.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

    # --------------------------------------------------------

    awsSecretKey: # [string] Secret key - Secret key
    region: # [string] Region - Region where the MinIO service/cluster is located
    stagePath: # [string] [required] Staging location - Filesystem location in which to buffer files, before compressing and moving to final destination. Use performant stable storage.
    addIdToStagePath: # [boolean] Add output ID - Append output's ID to staging location.
    removeEmptyDirs: # [boolean] Remove staging dirs - Remove empty staging directories after moving files.

    # -------------- if removeEmptyDirs is true ---------------

    emptyDirCleanupSec: # [number] Staging cleanup period - How often (secs) to clean-up empty directories when 'Remove Staging Dirs' is enabled.

    # --------------------------------------------------------

    destPath: # [string] [required] Key prefix - Root directory to prepend to path before uploading. Enter a constant, or a JS expression enclosed in quotes or backticks.
    signatureVersion: # [string] Signature version - Signature version to use for signing MinIO requests.
    objectACL: # [string] Object ACL - Object ACL to assign to uploaded objects.
    storageClass: # [string] Storage class - Storage class to select for uploaded objects.
    serverSideEncryption: # [string] Server-side encryption - Server-side encryption for uploaded objects.
    reuseConnections: # [boolean] Reuse connections - Whether to reuse connections between requests, which can improve performance.
    rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
    partitionExpr: # [string] Partitioning expression - JS expression defining how files are partitioned and organized. Default is date-based. If blank, Stream will fall back to the event's __partition field value â if present â otherwise to each location's root directory.
    format: # [string] Data format - Format of the output data.

    # -------------- if format is json ---------------

    compress: # [string] Compress - Choose data compression format to apply before moving files to final destination.

    # --------------------------------------------------------


    # -------------- if format is parquet ---------------

    parquetSchema: # [string] Parquet schema - Select a Parquet schema. New schemas can be uploaded under Processing > Knowledge > Parquet Schemas
    parquetRowGroupSize: # [string] Row group size - Ideal memory size for row group segments. E.g., 128MB or 1GB. Affects memory use when writing. Imposes a target, not a strict limit; row groups final size may be larger or smaller.
    parquetPageSize: # [string] Page size - Ideal memory size for page segments. E.g., 1MB or 128MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; pages final size may be larger or smaller.
    spacer: # [null] 
    parquetVersion: # [string] Parquet version - Determines which data types are supported and how they are represented.
    parquetDataPageVersion: # [string] Data page version - Serialization format of data pages. Note that not all reader implemtations support Data page V2.
    shouldLogInvalidRows: # [boolean] Log invalid rows - Output up to 20 unique rows that were skipped due to data format mismatch. Must have Logging set to Debug to see output.

    # --------------------------------------------------------

    baseFileName: # [string] File name prefix expression - JavaScript expression to define the output filename prefix (can be constant).
    fileNameSuffix: # [string] File name suffix expression - JavaScript expression to define the output filename suffix (can be constant).  The `__format` variable refers to the value of the `Data format` field (`json` or `raw`).  The `__compression` field refers to the kind of compression being used (`none` or `gzip`)
    maxFileSizeMB: # [number] Max file size (MB) - Maximum uncompressed output file size. Files of this size will be closed and moved to final output location.
    maxFileOpenTimeSec: # [number] Max file open time (Sec) - Maximum amount of time to write to a file. Files open for longer than this will be closed and moved to final output location.
    maxFileIdleTimeSec: # [number] Max file idle time (Sec) - Maximum amount of time to keep inactive files open. Files open for longer than this will be closed and moved to final output location.
    maxOpenFiles: # [number] Max open files - Maximum number of files to keep open concurrently. When over, the oldest open files will be closed and moved to final output location.
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  statsd_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination Protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number] [required] Port - Destination port.
    mtu: # [number] Max record Size (Bytes) - Used when Protocol is UDP, to specify the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - Used when Protocol is TCP, to specify how often buffers should be flushed resulting in records sent to the destination.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  statsd_ext_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination Protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number] [required] Port - Destination port.
    mtu: # [number] Max record Size (Bytes) - Used when Protocol is UDP, to specify the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - Used when Protocol is TCP, to specify how often buffers should be flushed resulting in records sent to the destination.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  graphite_output: # [object] 
    type: # [string] Output Type
    protocol: # [string] Destination Protocol - Protocol to use when communicating with the destination.

    # -------------- if protocol is tcp ---------------

    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # --------------------------------------------------------

    host: # [string] [required] Host - The hostname of the destination.
    port: # [number] [required] Port - Destination port.
    mtu: # [number] Max record Size (Bytes) - Used when Protocol is UDP, to specify the maximum size of packets sent to the destination. Also known as the MTU for the network path to the destination system.
    flushPeriodSec: # [number] Flush period (sec) - Used when Protocol is TCP, to specify how often buffers should be flushed resulting in records sent to the destination.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  router_output: # [object] 
    type: # [string] Output Type
    rules: # [array] Rules - Event routing rules
      - filter: # [string] Filter Expression - JavaScript expression to select events to send to output
        output: # [string] Output - Output to send matching events to
        description: # [string] Description - Description of this rule's purpose
        final: # [boolean] Final - Flag to control whether to stop the event from being checked against other rules
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  sqs_output: # [object] 
    type: # [string] Output Type
    queueName: # [string] Queue Name - The name, URL, or ARN of the SQS queue to send events to. When a non-AWS URL is specified, format must be: '{url}/myQueueName'. E.g., 'https://host:port/myQueueName'. Must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. Can be evaluated only at init time. E.g., referencing a Global Variable: `https://host:port/myQueue-${C.vars.myVar}`.
    queueType: # [string] [required] Queue Type - The queue type used (or created). Defaults to Standard.
    awsAccountId: # [string] AWS Account ID - SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.
    messageGroupId: # [string] Message Group ID - This parameter applies only to FIFO queues. The tag that specifies that a message belongs to a specific message group. Messages that belong to the same message group are processed in a FIFO manner. Use event field __messageGroupId to override this value.
    createQueue: # [boolean] Create Queue - Create queue if it does not exist.
    awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

    # -------------- if awsAuthenticationMethod is manual ---------------

    awsApiKey: # [string] Access key - Access key

    # --------------------------------------------------------


    # -------------- if awsAuthenticationMethod is secret ---------------

    awsSecret: # [string] Secret key pair - Select or create a stored secret that references your access key and secret key.

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
    maxQueueSize: # [number] Max queue size - Maximum number of queued batches before blocking.
    maxRecordSizeKB: # [number] Max record size (KB) - Maximum size (KB) of batches to send. Per the SQS spec, the max allowed value is 256 KB.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max record size.
    maxInProgress: # [number] Max concurrent requests - The maximum number of in-progress API requests before backpressure is applied.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  snmp_output: # [object] 
    type: # [string] Output Type
    hosts: # [array] SNMP Trap Destinations - One or more SNMP destinations to forward traps to
      - host: # [string] Address - Destination host
        port: # [number] Port - Destination port, default is 162
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  sumo_logic_output: # [object] 
    type: # [string] Output Type
    url: # [string] API URL - Sumo Logic HTTP collector URL to which events should be sent.
    customSource: # [string] Custom source name - Optionally, override the source name configured on the SumoÂ Logic HTTP collector. This can also be overridden at the event level with the __sourceName field.
    customCategory: # [string] Custom source category - Optionally, override the source category configured on the SumoÂ Logic HTTP collector. This can also be overridden at the event level with the __sourceCategory field.
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  datadog_output: # [object] 
    type: # [string] Output Type
    contentType: # [string] Send logs as - The content type to use when sending logs.
    message: # [string] Message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    source: # [string] Source - Name of the source to send with logs. When you send logs as JSON objects, the event's 'source' field (if set) will override this value.
    host: # [string] Host - Name of the host to send with logs. When you send logs as JSON objects, the event's 'host' field (if set) will override this value.
    service: # [string] Service - Name of the service to send with logs. When you send logs as JSON objects, the event's '__service' field (if set) will override this value.
    tags: # [array of strings] Datadog tags - List of tags to send with logs (e.g., 'env:prod', 'env_staging:east').
    allowApiKeyFromEvents: # [boolean] Allow API key from events - If enabled, the API key can be set from the event's '__agent_api_key' field.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
    severity: # [string] Severity - Default value for message severity. When you send logs as JSON objects, the event's '__severity' field (if set) will override this value.
    site: # [string] Datadog site
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - Organization's API key in Datadog

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  grafana_cloud_output: # [object] 
    type: # [string] Output Type
    lokiUrl: # [string] Loki URL - The endpoint to send logs to, e.g.: https://logs-prod-us-central1.grafana.net
    prometheusUrl: # [string] [required] Prometheus URL - The remote_write endpoint to send Prometheus metrics to, e.g.: https://prometheus-blocks-prod-us-central1.grafana.net/api/prom/push
    message: # [string] Logs message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    messageFormat: # [string] Message Format - Which format to use when sending logs to Loki (Protobuf or JSON).  Defaults to Protobuf.

    # -------------- if messageFormat is json ---------------

    compress: # [boolean] Compress - Whether to compress the payload body before sending. Applies only to Loki's JSON payloads, as both Prometheus' and Loki's Protobuf variant are snappy-compressed by default.

    # --------------------------------------------------------

    labels: # [array] Logs labels - List of labels to send with logs. Labels define Loki streams, so use static labels to avoid proliferating label value combinations and streams. Can be merged and/or overridden by the event's __labels field (e.g.: '__labels: {host: "cribl.io", level: "error"}').
      - name: # [string] Name - Name of the label.
        value: # [string] Value - Value of the label.
    metricRenameExpr: # [string] Metrics renaming expression - A JS expression that can be used to rename metrics. E.g.: name.replace(/\./g, '_') will replace all '.' characters in a metric's name with the supported '_' character. Use the 'name' global variable to access the metric's name.  You can access event fields' values via __e.<fieldName>.
    prometheusAuth: # [object] 
      authType: # [string] Authentication Type - The authentication method to use for the HTTP requests

      # -------------- if authType is token ---------------

      token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. E.g.: <your-username>:<your-api-key>.

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # --------------------------------------------------------


      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for authentication
      password: # [string] Password - Password (a.k.a API key in Grafana Cloud domain) for authentication

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

      # --------------------------------------------------------

    lokiAuth: # [object] 
      authType: # [string] Authentication Type - The authentication method to use for the HTTP requests

      # -------------- if authType is token ---------------

      token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. E.g.: <your-username>:<your-api-key>.

      # --------------------------------------------------------


      # -------------- if authType is textSecret ---------------

      textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

      # --------------------------------------------------------


      # -------------- if authType is basic ---------------

      username: # [string] Username - Username for authentication
      password: # [string] Password - Password (a.k.a API key in Grafana Cloud domain) for authentication

      # --------------------------------------------------------


      # -------------- if authType is credentialsSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

      # --------------------------------------------------------

    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking. Warning: Setting this value > 1 can cause Loki and Prometheus to complain about entries being delivered out of order.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    maxPayloadEvents: # [number] Max events per request - Maximum number of events to include in the request body. Default is 0 (unlimited). Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Maximum time between requests. Small values can reduce the payload size below the configured 'Max record size' and 'Max events per request'. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki and Prometheus to complain about entries being delivered out of order.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  loki_output: # [object] 
    type: # [string] Output Type
    url: # [string] Loki URL - The endpoint to send logs to.
    message: # [string] Logs message field - Name of the event field that contains the message to send. If not specified, Stream sends a JSON representation of the whole event.
    messageFormat: # [string] Message Format - Which format to use when sending logs to Loki (Protobuf or JSON).  Defaults to Protobuf.

    # -------------- if messageFormat is json ---------------

    compress: # [boolean] Compress - Whether to compress the payload body before sending.

    # --------------------------------------------------------

    labels: # [array] Logs labels - List of labels to send with logs. Labels define Loki streams, so use static labels to avoid proliferating label value combinations and streams. Can be merged and/or overridden by the event's __labels field (e.g.: '__labels: {host: "cribl.io", level: "error"}').
      - name: # [string] Name - Name of the label.
        value: # [string] Value - Value of the label.
    authType: # [string] Authentication Type - The authentication method to use for the HTTP requests

    # -------------- if authType is token ---------------

    token: # [string] Auth token - Bearer token to include in the authorization header. In Grafana Cloud, this is generally built by concatenating the username and the API key, separated by a colon. E.g.: <your-username>:<your-api-key>.

    # --------------------------------------------------------


    # -------------- if authType is textSecret ---------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------


    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for authentication
    password: # [string] Password - Password (a.k.a API key in Grafana Cloud domain) for authentication

    # --------------------------------------------------------


    # -------------- if authType is credentialsSecret ---------------

    credentialsSecret: # [string] Credentials secret - Select or create a secret that references your credentials

    # --------------------------------------------------------

    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking. Warning: Setting this value > 1 can cause Loki to complain about entries being delivered out of order.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    maxPayloadEvents: # [number] Max events per request - Maximum number of events to include in the request body. Defaults to 0 (unlimited). Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Maximum time between requests. Small values can reduce the payload size below the configured 'Max record size' and 'Max events per request'. Warning: Setting this too low can increase the number of ongoing requests (depending on the value of 'Request concurrency'); this can cause Loki to complain about entries being delivered out of order.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  prometheus_output: # [object] 
    type: # [string] Output Type
    url: # [string] Remote Write URL - The endpoint to send metrics to.
    metricRenameExpr: # [string] Metric renaming expression - A JS expression that can be used to rename metrics. E.g.: name.replace(/\./g, '_') will replace all '.' characters in a metric's name with the supported '_' character. Use the 'name' global variable to access the metric's name.  You can access event fields' values via __e.<fieldName>.
    sendMetadata: # [boolean] Send metadata - Whether to generate and send metadata (`type` and `metricFamilyName`) requests.

    # -------------- if sendMetadata is true ---------------

    metricsFlushPeriodSec: # [number] Metadata flush period (sec) - How frequently metrics metadata is sent out. Value cannot be smaller than the base Flush period (sec) set above.

    # --------------------------------------------------------

    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication type - Remote Write authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

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

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  ring_output: # [object] 
    type: # [string] Output Type
    format: # [string] Data format - Format of the output data.
    partitionExpr: # [string] Partitioning expression - JS expression to define how files are partitioned and organized. If left blank, Cribl Stream will fallback on event.__partition.
    maxDataSize: # [string] Max data size - Maximum disk space allowed to be consumed (e.g., 420MB or 4GB). Once reached, older data will be deleted.
    maxDataTime: # [string] Max data age - Maximum amount of time to retain data (e.g., 2h or 4d). Once reached, older data will be deleted.
    compress: # [string] Compression - Select data compression format. Optional.
    destPath: # [string] Path location - Path to use to write metrics. Defaults to $CRIBL_HOME/state/<id>`
    onBackpressure: # [string] Backpressure behavior - Whether to block or drop events when all receivers are exerting backpressure.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  open_telemetry_output: # [object] 
    type: # [string] Output Type
    endpoint: # [string] Endpoint - The endpoint to send OTEL events to. It can be any valid URL or an IP address (both IPv4 and IPv6 are supported). If the port is not specified, it will default to 4317, unless the endpoint is an HTTPS-based URL or TLS is enabled. In such cases, it will default to 443.
    authType: # [string] Authentication type - OpenTelemetry authentication type

    # -------------- if authType is basic ---------------

    username: # [string] Username - Username for Basic authentication
    password: # [string] Password - Password for Basic authentication

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

    tls: # [object] TLS settings (client side)
      disabled: # [boolean] Disabled

      # -------------- if disabled is false ---------------

      rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to No.
      certificateName: # [string] Certificate name - The name of the predefined certificate.
      caPath: # [string] CA certificate path - Path on client in which to find CA certificates to verify the server's cert. PEM format. Can reference $ENV_VARS.
      privKeyPath: # [string] Private key path (mutual auth) - Path on client in which to find the private key to use. PEM format. Can reference $ENV_VARS.
      certPath: # [string] Certificate path (mutual auth) - Path on client in which to find certificates to use. PEM format. Can reference $ENV_VARS.
      passphrase: # [string] Passphrase - Passphrase to use to decrypt private key.
      minVersion: # [string] Minimum TLS version - Minimum TLS version to use when connecting
      maxVersion: # [string] Maximum TLS version - Maximum TLS version to use when connecting

      # --------------------------------------------------------

    metadata: # [array] Metadata - Extra information to send with each gRPC request in the form of a list of key-value pairs. Value supports JavaScript expressions that are evaluated just once, when the destination gets started. In case you need to pass credentials as metadata, it's encouraged to use 'C.Secret'.
      - key: # [string] Key - The key of the metadata.
        value: # [string] Value - The value of the metadata.
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    keepAliveTime: # [number] Keep Alive Time (seconds) - How often the sender should ping the peer to keep the connection alive.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  dataset_output: # [object] 
    type: # [string] Output Type
    messageField: # [string] Message field - Name of the event field that contains the message or attributes to send. If not specified, all of the event's non-internal fields will be sent as attributes.
    excludeFields: # [array of strings] Exclude fields - Fields to exclude from the event if the Message field is either unspecified or refers to an object. Ignored if the Message field is a string. If empty, we send all non-internal fields.
    serverHostField: # [string] Server/host field - Name of the event field that contains the `serverHost` identifier. If not specified, defaults to `cribl_<outputId>`.
    timestampField: # [string] Timestamp field - Name of the event field that contains the timestamp. If not specified, defaults to `ts`, `_time`, or `Date.now()`, in that order.
    defaultSeverity: # [string] Severity - Default value for event severity. If the `sev` or `__severity` fields are set on an event, the first one matching will override this value.
    site: # [string] DataSet site - DataSet site to which events should be sent
    customUrl: # [string] 
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter API key directly, or select a stored secret
    apiKey: # [string] API key - A 'Log Write Access' API key for the DataSet account

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] API key (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  logstream_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number] Port - The port to connect to on the provided host

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    hosts: # [array] Destinations - Set of hosts to load-balance data to.
      - host: # [string] Address - The hostname of the receiver.
        port: # [number] Port - The port to connect to on the provided host.
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS.
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (iff not an IP); otherwise, to the global TLS settings.
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.
    maxConcurrentSenders: # [number] Max connections - Maximum number of concurrent connections (per worker process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
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

    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tokenTTLMinutes: # [number] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token
    authToken: # [string] Auth token - Optional authentication token to include as part of the connection header

    # -------------- if authType is manual ---------------


    # --------------------------------------------------------

    textSecret: # [string] Auth token (text secret) - Select or create a stored text secret

    # -------------- if authType is secret ---------------


    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cribl_tcp_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    host: # [string] Address - The hostname of the receiver
    port: # [number] Port - The port to connect to on the provided host

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    hosts: # [array] Destinations - Set of hosts to load-balance data to.
      - host: # [string] Address - The hostname of the receiver.
        port: # [number] Port - The port to connect to on the provided host.
        tls: # [string] TLS - Whether to inherit TLS configs from group setting or disable TLS.
        servername: # [string] TLS Servername - Servername to use if establishing a TLS connection. If not specified, defaults to connection host (iff not an IP); otherwise, to the global TLS settings.
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.
    maxConcurrentSenders: # [number] Max connections - Maximum number of concurrent connections (per worker process). A random set of IPs will be picked on every DNS resolution period. Use 0 for unlimited.

    # --------------------------------------------------------

    compression: # [string] Compression - Codec to use to compress the data before sending
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
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

    connectionTimeout: # [number] Connection Timeout - Amount of time (milliseconds) to wait for the connection to establish before retrying
    writeTimeout: # [number] Write Timeout - Amount of time (milliseconds) to wait for a write to complete before assuming connection is dead
    tokenTTLMinutes: # [number] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60
    excludeFields: # [array of strings] Exclude Fields - Fields to exclude from the event. By default, all internal fields except `__output` are sent.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
  cribl_http_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    url: # [string] Cribl endpoint - URL of a Cribl Worker to send events to, e.g., http://localhost:10200
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    urls: # [array] Cribl Worker Endpoints
      - url: # [string] Cribl Endpoint - URL of a Cribl Worker to send events to, e.g., http://localhost:10200
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.

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

    tokenTTLMinutes: # [number] Auth Token TTL minutes - The number of minutes before the internally generated authentication token expires, valid values between 1 and 60.
    excludeFields: # [array of strings] Exclude fields - Fields to exclude from the event. By default, all internal fields except `__output` are sent.
    compression: # [string] Compression - Codec to use to compress the data before sending.
    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  humio_hec_output: # [object] 
    type: # [string] Output Type
    loadBalanced: # [boolean] Load balancing - Use load-balanced destinations

    # -------------- if loadBalanced is false ---------------

    url: # [string] Humio HEC Endpoint - URL to a Humio HEC endpoint to send events to, e.g., https://cloud.us.humio.com/api/v1/ingest/hec for JSON and https://cloud.us.humio.com/api/v1/ingest/hec/raw for raw 
    useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. When a DNS server returns multiple addresses, this will cause Stream to cycle through them in the order returned.

    # --------------------------------------------------------


    # -------------- if loadBalanced is true ---------------

    excludeSelf: # [boolean] Exclude current host IPs - Exclude all IPs of the current host from the list of any resolved hostnames.
    urls: # [array] Humio HEC Endpoints
      - url: # [string] HEC Endpoint - URL to a Humio HEC endpoint to send events to, e.g., https://cloud.us.humio.com/api/v1/ingest/hec for JSON and https://cloud.us.humio.com/api/v1/ingest/hec/raw for raw 
        weight: # [number] Load Weight - The weight to use for load-balancing purposes.
    dnsResolvePeriodSec: # [number] DNS resolution period (seconds) - Re-resolve any hostnames every this many seconds and pick up destinations from A records.
    loadBalanceStatsPeriodSec: # [number] Load balance stats period (seconds) - How far back in time to keep traffic stats for load balancing purposes.

    # --------------------------------------------------------

    concurrency: # [number] Request concurrency - Maximum number of ongoing requests before blocking.
    maxPayloadSizeKB: # [number] Max body size (KB) - Maximum size, in KB, of the request body.
    maxPayloadEvents: # [number] Max events per request - Max number of events to include in the request body. Default is 0 (unlimited).
    compress: # [boolean] Compress - Whether to compress the payload body before sending.
    rejectUnauthorized: # [boolean] Validate server certs - Reject certs that are not authorized by a CA in the CA certificate path, or by another trusted CA (e.g., the system's CA). Defaults to Yes.
    timeoutSec: # [number] Request timeout - Amount of time, in seconds, to wait for a request to complete before aborting it.
    flushPeriodSec: # [number] Flush period (sec) - Maximum time between requests. Small values could cause the payload size to be smaller than the configured Max body size.
    extraHttpHeaders: # [array] Extra HTTP headers - Extra HTTP headers.
      - name: # [string] Name - Field name
        value: # [string] Value - Field value
    failedRequestLoggingMode: # [string] Failed request logging mode - Determines which data should be logged when a request fails. Defaults to None.  All headers are redacted by default, except those listed under `Safe Headers`.
    safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
    format: # [string] Request Format - Send data in JSON format to the api/v1/ingest/hec endpoint , or raw 1-request-per-line to the api/v1/ingest/hec/raw endpoint .
    authType: # [string] Authentication method - Enter a token directly, or provide a secret referencing a token

    # -------------- if authType is manual ---------------

    token: # [string] HEC Auth token - Humio HEC authentication token

    # --------------------------------------------------------


    # -------------- if authType is secret ---------------

    textSecret: # [string] HEC Auth token (text secret) - Select or create a stored text secret

    # --------------------------------------------------------

    onBackpressure: # [string] Backpressure behavior - Whether to block, drop, or queue events when all receivers are exerting backpressure.

    # -------------- if onBackpressure is queue ---------------

    pqMaxFileSize: # [string] Max file size - The maximum size to store in each queue file before closing and optionally compressing (KB, MB, etc.).
    pqMaxSize: # [string] Max queue size - The maximum amount of disk space the queue is allowed to consume. Once reached, the system stops queueing and applies the fallback Queue-full behavior. Enter a numeral with units of KB, MB, etc.
    pqPath: # [string] Queue file path - The location for the persistent queue files. To this field's value, the system will append: /<worker-id>/<output-id>.
    pqCompress: # [string] Compression - Codec to use to compress the persisted data.
    pqOnBackpressure: # [string] Queue-full behavior - Whether to block or drop events when the queue is exerting backpressure (full capacity or low disk). 'Block' is the same behavior as non-PQ blocking. 'Drop new data' throws away incoming data, while leaving the contents of the PQ unchanged.
    pqControls: # [object] 

    # --------------------------------------------------------

    pipeline: # [string] Pipeline - Pipeline to process data before sending out to this output.
    systemFields: # [array of strings] System fields - Set of fields to automatically add to events using this output. E.g.: cribl_pipe, c*. Wildcards supported.
    environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
    streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
```




