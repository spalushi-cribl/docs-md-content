# jobs.yml


`jobs.yml` maintains parameters for configured [Collectors](/stream/collectors), corresponding to those listed on the **Collectors** page. Each collection job is listed according to the pattern shown below.

```yaml {title="jobs.yml"}
collection_job: # [object] 
  workerAffinity: # [boolean] Worker affinity - If enabled, tasks are created and run by the same Worker Node
  collector: # [object] 
    type: # [string] Collector type - The type of collector to run

    # -------------- if type is azure_blob ---------------

    conf: # [object] [required] 
      type: # [string] 
      outputName: # [string] Auto-populate from - An optional predefined Destination that will be used to auto-populate Collector settings
      authType: # [string] Authentication method - Enter authentication data directly, or select a secret referencing your auth data

      # -------------- if authType is manual ---------------

      connectionString: # [string] Connection string - Enter your Azure storage account Connection String. If left blank, Cribl Stream will fall back to env.AZURE_STORAGE_CONNECTION_STRING.

      # --------------------------------------------------------


      # -------------- if authType is secret ---------------

      textSecret: # [string] Connection string (text secret) - Text secret

      # --------------------------------------------------------


      # -------------- if authType is clientSecret ---------------

      storageAccountName: # [string] Storage account name - The name of your Azure storage account
      tenantId: # [string] Tenant ID - The service principal's tenant ID
      clientId: # [string] Client ID - The service principal's client ID
      clientTextSecret: # [string] Client secret (text secret) - Text secret containing the client secret
      endpointSuffix: # [string] Endpoint suffix - The endpoint suffix for the service URL. Takes precedence over the Azure Cloud setting. Defaults to core.windows.net.
      azureCloud: # [string] Azure Cloud - The Azure cloud to use. Defaults to Azure Public Cloud.

      # --------------------------------------------------------


      # -------------- if authType is clientCert ---------------

      storageAccountName: # [string] Storage account name - The name of your Azure storage account
      tenantId: # [string] Tenant ID - The service principal's tenant ID
      clientId: # [string] Client ID - The service principal's client ID
      certificate: # [object] 
        certificateName: # [string] Certificate - The certificate you registered as credentials for your app in the Azure portal
      azureCloud: # [string] Azure Cloud - The Azure cloud to use. Defaults to Azure Public Cloud.
      endpointSuffix: # [string] Endpoint suffix - The endpoint suffix for the service URL. Takes precedence over the Azure Cloud setting. Defaults to core.windows.net.

      # --------------------------------------------------------

      containerName: # [string] Container name - Container to collect from. This value can be a constant, or a JavaScript expression that can only be evaluated at init time. Example referencing a Global Variable: myBucket-${C.vars.myVar}
      path: # [string] Path - The directory from which to collect data. Templating is supported, such as myDir/${datacenter}/${host}/${app}/. Time-based tokens are supported, such as myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/.
      extractors: # [array] Path extractors - Extractors allow use of template tokens as context for expressions that enrich discovery results. For example, given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)} will enrich discovery results with a human-readable "date" field.
        - key: # [string] Token - A token from the template path, such as epoch
          expression: # [string] Extractor Expression - A JavaScript expression that accesses a corresponding <token> through the value variable and evaluates the token to populate event fields. Example: {date: new Date(+value*1000)}
      recurse: # [boolean] Recursive - Recurse through subdirectories
      includeMetadata: # [boolean] Include metadata - Include Azure Blob metadata in collected events. In each event, metadata will be located at: __collectible.metadata.
      includeTags: # [boolean] Include tags - Include Azure Blob tags in collected events. In each event, tags will be located at: __collectible.tags. Disable this feature when using a Shared Access Signature Connection String, to prevent errors.
      maxBatchSize: # [number; minimum: 1] Batch size limit - Maximum number of metadata objects to batch before recording as results
      parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit - Maximum file size for each Parquet chunk
      parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will abort if a chunk cannot be downloaded within the time specified.

    # --------------------------------------------------------


    # -------------- if type is cribl_lake ---------------

    conf: # [object] [required] 
      type: # [string] 
      dataset: # [string] Lake dataset - Lake dataset to collect data from.

    # --------------------------------------------------------


    # -------------- if type is database ---------------

    conf: # [object] [required] 
      type: # [string] 
      connectionId: # [string] Connection - Select an existing Connection, or go to Knowledge > Database Connections to add one
      query: # [string] [required] SQL Query - An expression that resolves to the query string for selecting data from the database. Has access to the special ${earliest} and ${latest} variables, which will resolve to the Collector run's start and end time.
      queryValidationEnabled: # [boolean] Validate Query - Enforces a basic query validation that allows only a single 'select' statement. Disable for more complex queries or when using semicolons. Caution: Disabling query validation allows DDL and DML statements to be executed, which could be destructive to your database.
      defaultBreakers: # [string] Hidden Default Breakers
      __scheduling: # [object] 
        stateTracking: # [object] 
          enabled: # [boolean] Enabled - Enable tracking of collection progress between consecutive scheduled executions.

          # -------------- if enabled is true ---------------

          trackingColumn: # [string] Tracking Column - A numeric database column whose greatest seen value will be tracked. In the collector query, this column must be returned from the database in monotonically increasing fashion.

          # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if type is filesystem ---------------

    conf: # [object] [required] 
      type: # [string] 
      outputName: # [string] Auto-populate from - Select a predefined configuration (a Destination) to auto-populate Collector settings
      path: # [string] Directory - The directory from which to collect data. Templating is supported, such as /myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, such as /myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/.
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. For example, given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field.
        - key: # [string] Token - A token from the template directory, such as epoch
          expression: # [string] Extractor expression - JavaScript expression that receives token under "value" variable, and evaluates to populate event fields, such as {date: new Date(+value*1000)}
      recurse: # [boolean] Recursive - Recurse through subdirectories
      maxBatchSize: # [number; minimum: 1] Batch size limit (files) - Maximum number of metadata files to batch before recording as results

    # --------------------------------------------------------


    # -------------- if type is google_cloud_storage ---------------

    conf: # [object] [required] 
      type: # [string] 
      outputName: # [string] Auto-populate from - Name of the predefined Destination that will be used to auto-populate Collector settings
      bucket: # [string] Bucket name - Name of the bucket to collect from. This value can be a constant or a JavaScript expression that can only be evaluated at init time. Example referencing a Global Variable: `myBucket-${C.vars.myVar}`.
      path: # [string] Path - The directory from which to collect data. Templating is supported, such as myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, such as myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/.
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. For example, given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field.
        - key: # [string] Token - A token from the template path, such as epoch
          expression: # [string] Extractor Expression - JavaScript expression that receives token under "value" variable, and evaluates to populate event fields, such as {date: new Date(+value*1000)}
      endpoint: # [string] Endpoint - Google Cloud Storage service endpoint. If empty, the endpoint will default to https://storage.googleapis.com.
      disableTimeFilter: # [boolean] Disable time filter - Used to disable Collector event time filtering when a date range is specified
      recurse: # [boolean] Recursive - Recurse through subdirectories
      maxBatchSize: # [number; minimum: 1] Batch size limit (objects) - Maximum number of metadata objects to batch before recording as results
      authType: # [string] Authentication method - Enter account credentials manually, select a secret that references your credentials, or use Google Application Default Credentials

      # -------------- if authType is manual ---------------

      serviceAccountCredentials: # [string] Service account credentials - Contents of Google Cloud service account credentials (JSON keys) file. To upload a file, click the upload button at this field's upper right.

      # --------------------------------------------------------


      # -------------- if authType is secret ---------------

      textSecret: # [string] Service account credentials (text secret) - Select or create a stored text secret that references your credentials

      # --------------------------------------------------------

      parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
      parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - The maximum time allowed for downloading a Parquet chunk. Processing will abort if a chunk cannot be downloaded within the time specified.

    # --------------------------------------------------------


    # -------------- if type is health_check ---------------

    conf: # [object] [required] 
      discovery: # [object] 
        discoverType: # [string] Discover Type - Defines how task discovery will be performed. Use None to skip the discovery. Use HTTP Request to make a REST call to discover tasks. Use Item List to enumerate items for collect to retrieve. Use JSON Response to manually define discover tasks as a JSON array of objects. Each entry returned by the discover operation will result in a collect task.

        # -------------- if discoverType is http ---------------

        discoverUrl: # [string] Discover URL - Expression to derive URL to use for the Discover operation (can be a constant).
        discoverMethod: # [string] Discover method - Discover HTTP method.
        discoverRequestHeaders: # [array] Discover Headers - Optional discover request headers.
          - name: # [string] Name - Header name.
            value: # [string] Value - JavaScript expression to compute the header value (can be a constant).
        discoverDataField: # [string] Discover Data Field - Path to field in the response object which contains discover results (e.g.: level1.name), leave blank if the result is an array.

        # --------------------------------------------------------


        # -------------- if discoverType is json ---------------

        manualDiscoverResult: # [string] Discover result - Allows hard-coding the Discover result. Must be a JSON object. Works with the Discover Data field.
        discoverDataField: # [string] Discover data field - Within the response JSON, name of the field or array element to pull results from. Leave blank if the result is an array of values. Sample entry: items, json: { items: [{id: 'first'},{id: 'second'}] }

        # --------------------------------------------------------


        # -------------- if discoverType is list ---------------

        itemList: # [array of strings] Discover items - Comma-separated list of items to return from the Discover task. Each item returned will generate a collect task, and can be referenced using `${id}` in the collect URL, headers, or parameters.

        # --------------------------------------------------------

      collectUrl: # [string] Health check URL - Expression to derive URL to use for the health check operation (can be a constant).
      collectMethod: # [string] [required] Health check method - Health check HTTP method.

      # -------------- if collectMethod is get ---------------

      collectRequestParams: # [array] Health check parameters - Optional health check request parameters.
        - name: # [string] Name - Parameter name
          value: # [string] Value - JavaScript expression to compute the parameter value (can be a constant).

      # --------------------------------------------------------


      # -------------- if collectMethod is post ---------------

      collectRequestParams: # [array] Health check parameters - Optional health check request parameters.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter value (can be a constant).

      # --------------------------------------------------------


      # -------------- if collectMethod is post_with_body ---------------

      collectBody: # [string] Health check POST Body - Template for POST body to send with the health check request. You can reference parameters from the Discover response, using template params of the form: ${variable}.

      # --------------------------------------------------------

      collectRequestHeaders: # [array] Health check headers - Optional health check request headers.
        - name: # [string] Name - Header Name
          value: # [string] Value - JavaScript expression to compute the header value (can be a constant).
      authenticateCollect: # [boolean] Authenticate health check - Enable to make auth health check call.
      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls. You can specify API Key–based authentication by adding the appropriate Collect headers.

      # -------------- if authentication is basic ---------------

      username: # [string] Username - Basic authentication username
      password: # [string] Password - Basic authentication password

      # --------------------------------------------------------


      # -------------- if authentication is basicSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your credentials

      # --------------------------------------------------------


      # -------------- if authentication is login ---------------

      loginUrl: # [string] Login URL - URL to use for login API call. This call is expected to be a POST.
      username: # [string] Username - Login username
      password: # [string] Password - Login password
      loginBody: # [string] POST body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication Headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header value (can be a constant).

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call, this call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. If left blank, the entire response body will be used to derive the authorization header.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication Headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header value (can be a constant).

      # --------------------------------------------------------


      # -------------- if authentication is oauth ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      clientSecretParamValue: # [string] Client secret value - Secret value to add to HTTP requests as the 'client secret' parameter. Stored on disk encrypted, and is automatically added to request parameters
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if authentication is oauthSecret ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      textSecret: # [string] Client secret value (text secret) - Select or create a text secret that contains the client secret's value.
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g., `${earliest}`). If a constant, use single quotes (e.g., 'earliest'). Values without delimiters (e.g., earliest) are evaluated as strings.

      # --------------------------------------------------------

      timeout: # [number; maximum: 1800] Request Timeout (secs) - HTTP request inactivity timeout, use 0 to disable
      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
      defaultBreakers: # [string] Hidden Default Breakers
      safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.
      retryRules: # [object] 
        type: # [string] Retry type - The algorithm to use when performing HTTP retries

        # -------------- if type is static ---------------

        interval: # [number; maximum: 20000] Wait (ms) - Time interval between retries. Maximum allowed value is 20,000 ms (1/3 minute).
        limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.

        # --------------------------------------------------------


        # -------------- if type is backoff ---------------

        interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
        limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
        multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff, e.g., base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.

        # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if type is office365_mgmt ---------------

    conf: # [object] [required] 
      plan_type: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Enterprise
      tenant_id: # [string] [required] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory
      app_id: # [string] [required] Application identifier - Identifier of the registered application in Azure Active Directory.
      client_secret: # [string] [required] Client secret - Application key of the registered application.
      publisher_identifier: # [string] Publisher identifier - Optional PublisherIdentifier to use in API requests, defaults to tenant id if not defined.
      content_type: # [string] [required] Content type - The type of content to retrieve from the Office 365 management communications API.
      ingestionLag: # [number] Ingestion lag (minutes) - This is necessary because there can be a delay of 60 - 90 minutes before office 365 events are available for retrieval. This setting will adjust the date range passed to the management activity API to account for the delay.

    # --------------------------------------------------------


    # -------------- if type is office365_service ---------------

    conf: # [object] [required] 
      tenant_id: # [string] Tenant identifier - Directory ID (tenant identifier) in Azure Active Directory
      app_id: # [string] [required] Application identifier - Identifier of the registered application in Azure Active Directory.
      client_secret: # [string] [required] Client secret - Application key of the registered application.
      content_type: # [string] [required] Content type - The type of content to retrieve from the Office 365 service communications API.

    # --------------------------------------------------------


    # -------------- if type is prometheus ---------------

    conf: # [object] [required] 
      dimensionList: # [array] Extra Dimensions - Other dimensions to include in events
      username: # [string] Username - Optional username for Basic authentication
      password: # [string] Password - Optional password for Basic authentication
      discoveryType: # [string] Discovery Type - Target discovery mechanism, use static to manually enter a list of targets

      # -------------- if discoveryType is static ---------------

      targetList: # [array] Targets - List of Prometheus targets to pull metrics from, values can be in URL or host[:port] format. For example: http://localhost:9090/metrics, localhost:9090, or localhost. In the cases where just host[:port] are specified, the endpoint will resolve to 'http://host[:port]/metrics'.

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
      scrapePort: # [number; minimum: 1, maximum: 65535] Metrics Port - The port number in the metrics URL for discovered targets.
      scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets
      searchFilter: # [array] Search Filter - EC2 Instance Search Filter
        - Name: # [string] Filter Name - Search filter attribute name, see: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html for more information. Attributes can be manually entered if not present in the drop down list
          Values: # [array of strings] Filter Values - Search Filter Values
      region: # [string] Region - Region from which to retrieve data.
      awsAuthenticationMethod: # [string] Authentication method - AWS authentication method
      enableAssumeRole: # [boolean] Enable Assume Role - Use Assume Role credentials
      endpoint: # [string] Endpoint - EC2 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, used to point to a EC2-compatible endpoint.
      signatureVersion: # [string] Signature version - Signature version to use for signing EC2 requests

      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if type is rest ---------------

    conf: # [object] [required] 
      type: # [string] 
      discovery: # [object] 
        discoverType: # [string] Discover type - Defines how task discovery will be performed. Each entry returned by the Discover operation will result in a Collect task.

        # -------------- if discoverType is http ---------------

        discoverUrl: # [string] Discover URL - URL to use for the Discover operation. Can be a constant URL, or a JavaScript expression to derive the URL.
        discoverMethod: # [string] Discover method
        discoverRequestHeaders: # [array] Discover headers
          - name: # [string] Name
            value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.
        pagination: # [object] 
          type: # [string] Pagination

          # -------------- if type is response_body ---------------

          attribute: # [array,string] Response attributes - Names of attributes within the response that contain next-page information
          maxPages: # [number] Page limit - Maximum number of pages to retrieve for the discover task. Defaults to 50 pages. Set to 0 to retrieve all pages.
          lastPageExpr: # [string] Last-page expression - JavaScript expression used to determine when the last page has been reached. The values tested by this expression must be in the Response attributes section.

          # --------------------------------------------------------


          # -------------- if type is response_header ---------------

          attribute: # [array,string] Response attributes - Names of attributes within the response that contain next-page information
          maxPages: # [number] Page limit - Maximum number of pages to retrieve for the discover task. Defaults to 50 pages. Set to 0 to retrieve all pages.

          # --------------------------------------------------------


          # -------------- if type is response_header_link ---------------

          nextRelationAttribute: # [string] Next page relation name - Relation name used in the link header that refers to the next page in the result set. Example: rel="next" refers to the next page of results: <https://myHost/nextPage>; rel="next"
          curRelationAttribute: # [string] Current page relation name - Relation name used in the link header that refers to the current result set. Example: rel="self" refers to the current page of results: <https://myHost/curPage>; rel="self" 
          maxPages: # [number] Page limit - Maximum number of pages to retrieve for the discover task. Defaults to 50 pages. Set to 0 to retrieve all pages.

          # --------------------------------------------------------


          # -------------- if type is request_offset ---------------

          offsetField: # [string] Offset field name - Query string parameter that sets the index from which to begin returning records. Example: /api/v1/query?term=cribl&limit=100&offset=0
          offset: # [number] Starting offset - Offset index from which to start request. Defaults to undefined, which will start discovery from the first record.
          offsetSpacer: # [null] 
          limitField: # [string] Limit field name - Query string parameter that sets the number of records retrieved per request. Example: /api/v1/query?term=cribl&limit=100&offset=0
          limit: # [number; minimum: 1] Record limit - Maximum number of records to retrieve per request
          limitSpacer: # [null] 
          totalRecordField: # [string] Total record count field name - Name of the attribute in the response that contains the total number of records for the query
          maxPages: # [number] Page limit - Maximum number of pages to retrieve for the discover task. Defaults to 50 pages. Set to 0 to retrieve all pages.
          zeroIndexed: # [boolean] Zero-based index - Enable to indicate that the first page in the requested data is at index 0. Disabled by default, which indicates index 1.

          # --------------------------------------------------------


          # -------------- if type is request_page ---------------

          pageField: # [string] Page number field name - Query string parameter that sets the page index to be returned. Example: /api/v1/query?term=cribl&page_size=100&page_number=0
          page: # [number] Starting page number - Page number from which to start request. Defaults to undefined, which will start discovery from the first page.
          offsetSpacer: # [null] 
          sizeField: # [string] Page size field name - Query string parameter that sets the number of records retrieved per request. Example: /api/v1/query?term=cribl&page_size=100&page_number=0
          size: # [number; minimum: 1] Record limit - Maximum number of records to retrieve per page
          limitSpacer: # [null] 
          totalPageField: # [string] Total page count field name - Name of the attribute in the response that contains the total number of pages for the query
          totalRecordField: # [string] Total record count field name - Name of the attribute in the response that contains the total number of records for the query
          maxPages: # [number] Page limit - Maximum number of pages to retrieve for the discover task. Defaults to 50 pages. Set to 0 to retrieve all pages.
          zeroIndexed: # [boolean] Zero-based index - Enable to indicate that the first page in the requested data is at index 0. Disabled by default, which indicates index 1.

          # --------------------------------------------------------

        discoverDataField: # [string] Discover data field - Path to field in the response object that contains discovery results (ex: level1.name). Leave blank if the result is an array.
        enableDiscoverCode: # [boolean] Format discover result with custom code

        # --------------------------------------------------------


        # -------------- if discoverType is json ---------------

        manualDiscoverResult: # [string] Discover result - Allows hard-coding the Discover result. Must be a JSON object or array. Works with Discover data field.
        discoverDataField: # [string] Discover data field - Within the response JSON, the name of the field to pull results from, typically a JSON array. Leave blank if the result itself is an array of values. Sample entry: items, json: { items: [{id: 'first'},{id: 'second'}] }

        # --------------------------------------------------------


        # -------------- if discoverType is list ---------------

        itemList: # [array of strings] Discover items - Comma-separated list of items to return from the Discover task. Each item returned generates a Collect task and can be referenced using `${id}` in the Collect URL, headers, or parameters.

        # --------------------------------------------------------

      collectUrl: # [string] Collect URL - URL (constant or JavaScript expression) to use for the Collect operation
      collectMethod: # [string] [required] Collect method

      # -------------- if collectMethod is get ---------------

      collectRequestParams: # [array] Collect parameters
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if collectMethod is post ---------------

      collectRequestParams: # [array] Collect parameters
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if collectMethod is post_with_body ---------------

      collectBody: # [string] Collect POST body - Template for POST body to send with the Collect request. Reference global variables, functions, or parameters from the Discover response using template params: `${C.vars.myVar}`, or `${Date.now()}`, `${param}`

      # --------------------------------------------------------


      # -------------- if collectMethod is other ---------------

      collectVerb: # [string] Collect verb - Custom HTTP method to use for the Collect operation
      collectBody: # [string] Collect body - Template for body to send with the Collect request. Reference global variables, functions, or parameters from the Discover response using template parameters: `${C.vars.myVar}`, or `${Date.now()}`, `${param}`
      collectRequestParams: # [array] Collect parameters
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------

      collectRequestHeaders: # [array] Collect headers
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.
      pagination: # [object] 
        type: # [string] Pagination

        # -------------- if type is response_body ---------------

        attribute: # [array,string] Response attributes - Names of attributes within the response that contain next-page information
        maxPages: # [number] Page limit - Maximum number of pages to retrieve per collection task. Defaults to 50 pages. Set to 0 to retrieve all pages.
        lastPageExpr: # [string] Last-page expression - JavaScript expression used to determine when the last page has been reached. The values tested by this expression must be in the Response attributes section.

        # --------------------------------------------------------


        # -------------- if type is response_header ---------------

        attribute: # [array,string] Response attributes - Names of attributes within the response that contain next-page information
        maxPages: # [number] Page limit - Maximum number of pages to retrieve per collection task. Defaults to 50 pages. Set to 0 to retrieve all pages.

        # --------------------------------------------------------


        # -------------- if type is response_header_link ---------------

        nextRelationAttribute: # [string] Next page relation name - Relation name used in the link header that refers to the next page in the result set. Example: rel="next" refers to the next page of results: <https://myHost/nextPage>; rel="next"
        curRelationAttribute: # [string] Current page relation name - Relation name used in the link header that refers to the current result set. Example: rel="self" refers to the current page of results: <https://myHost/curPage>; rel="self" 
        maxPages: # [number] Page limit - Maximum number of pages to retrieve per collection task. Defaults to 50 pages. Set to 0 to retrieve all pages.

        # --------------------------------------------------------


        # -------------- if type is request_offset ---------------

        offsetField: # [string] Offset field name - Query string parameter that sets the index from which to begin returning records. Example: /api/v1/query?term=cribl&limit=100&offset=0
        offset: # [number] Starting offset - Offset index from which to start request. Defaults to undefined, which will start collection from the first record.
        offsetSpacer: # [null] 
        limitField: # [string] Limit field name - Query string parameter that sets the number of records retrieved per request. Example: /api/v1/query?term=cribl&limit=100&offset=0
        limit: # [number; minimum: 1] Record limit - Maximum number of records to collect per request
        limitSpacer: # [null] 
        totalRecordField: # [string] Total record count field name - Name of the attribute in the response that contains the total number of records for the query
        maxPages: # [number] Page limit - Maximum number of pages to retrieve per collection task. Defaults to 50 pages. Set to 0 to retrieve all pages.
        zeroIndexed: # [boolean] Zero-based index - Enable to indicate that the first page in the requested data is at index 0. Disabled by default, which indicates index 1.

        # --------------------------------------------------------


        # -------------- if type is request_page ---------------

        pageField: # [string] Page number field name - Query string parameter that sets the page index to be returned. Example: /api/v1/query?term=cribl&page_size=100&page_number=0
        page: # [number] Starting page number - Page number from which to start request. Defaults to undefined, which will start collection from the first page.
        offsetSpacer: # [null] 
        sizeField: # [string] Page size field name - Query string parameter that sets the number of records retrieved per request. Example: /api/v1/query?term=cribl&page_size=100&page_number=0
        size: # [number; minimum: 1] Record limit - Maximum number of records to collect per page
        limitSpacer: # [null] 
        totalPageField: # [string] Total page count field name - Name of the attribute in the response that contains the total number of pages for the query
        totalRecordField: # [string] Total record count field name - Name of the attribute in the response that contains the total number of records for the query
        maxPages: # [number] Page limit - Maximum number of pages to retrieve per collection task. Defaults to 50 pages. Set to 0 to retrieve all pages.
        zeroIndexed: # [boolean] Zero-based index - Enable to indicate that the first page in the requested data is at index 0. Disabled by default, which indicates index 1.

        # --------------------------------------------------------

      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls. You can specify API key–based authentication by adding the appropriate Collect headers.

      # -------------- if authentication is basic ---------------

      username: # [string] Username
      password: # [string] Password

      # --------------------------------------------------------


      # -------------- if authentication is basicSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your credentials

      # --------------------------------------------------------


      # -------------- if authentication is login ---------------

      loginUrl: # [string] Login URL - URL to use for login API call. This call is expected to be a POST.
      username: # [string] Login username
      password: # [string] Login password
      loginBody: # [string] POST body - Template for POST body to send with login request. ${username} and ${password} are used to specify location of these attributes in the message.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderKey: # [string] Authorization header - Authorization header key to pass in Discover and Collect calls. Defaults to the literal name 'Authorization'.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression used to compute the Authorization header to pass in Discover and Collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication headers
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call. This call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST body - Template for POST body to send with login request. ${username} and ${password} are used to specify location of these attributes in the message.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. If left blank, the entire response body will be used to derive the authorization header.
      authHeaderKey: # [string] Authorization header - Authorization header key to pass in Discover and Collect calls. Defaults to the literal name 'Authorization'.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in Discover and Collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication headers
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if authentication is oauth ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderKey: # [string] Authorization header - Authorization header key to pass in Discover and Collect calls. Defaults to the literal name 'Authorization'.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in Discover and Collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Defaults to 'client_secret'. Automatically added to request parameters using the value specified.
      clientSecretParamValue: # [string] Client secret value - Secret value to add to HTTP requests as the 'client secret' parameter. Value is stored encrypted on disk and automatically added to request parameters.
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.
      authRequestHeaders: # [array] Authentication headers
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if authentication is oauthSecret ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK. Leave blank if the response content type is text/plain; the entire response body will be used to derive the authorization header.
      authHeaderKey: # [string] Authorization header - Authorization header key to pass in Discover and Collect calls. Defaults to the literal name 'Authorization'.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in Discover and Collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Defaults to 'client_secret'. Automatically added to request parameters using the value specified.
      textSecret: # [string] Client secret value (text secret) - Select or create a text secret that contains the client secret's value
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.
      authRequestHeaders: # [array] Authentication headers
        - name: # [string] Name
          value: # [string] Value - JavaScript expression to compute parameter value, usually enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values that aren't successfully evaluated as JavaScript expressions will be treated as string constants.

      # --------------------------------------------------------


      # -------------- if authentication is google_oauth ---------------

      scopes: # [array of strings] Scopes - Scopes to use during authentication. See [Google's docs](https://developers.google.com/identity/protocols/oauth2/scopes) for more information.
      serviceAccountCredentials: # [string] Service account credentials - Contents of Google Cloud service account credentials (JSON keys) file. To upload a file, click the upload icon in this field's upper right.
      subject: # [string] Impersonated account's email address - Email address of a user account with Super Admin permissions to the resources the collector will retrieve

      # --------------------------------------------------------


      # -------------- if authentication is google_oauthSecret ---------------

      scopes: # [array of strings] Scopes - Scopes to use during authentication. See [Google's docs](https://developers.google.com/identity/protocols/oauth2/scopes) for more information.
      textSecret: # [string] Service account credentials (text secret) - Select or create a text secret that contains the Google service account credentials value
      subject: # [string] Impersonated account's email address - Email address of a user account with Super Admin permissions to the resources the collector will retrieve

      # --------------------------------------------------------


      # -------------- if authentication is hmac ---------------

      hmacFunctionId: # [string] HMAC Function - Select or create an HMAC Function to use with authentication

      # --------------------------------------------------------

      timeout: # [number; maximum: 1800] Request timeout (secs) - HTTP request inactivity timeout. Use 0 to disable.
      useRoundRobinDns: # [boolean] Round-robin DNS - Use round-robin DNS lookup. Suitable when DNS server returns multiple addresses in sort order.
      disableTimeFilter: # [boolean] Disable time filter - Disable Collector event time filtering when a date range is specified
      decodeUrl: # [boolean] Decode URL - Decode the URL before sending requests (including pagination requests)
      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as self-signed certificates)
      captureHeaders: # [boolean] Capture response headers - Enable to add response headers to the resHeaders field under the __collectible object
      safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text
      retryRules: # [object] 
        type: # [string] Retry type - Algorithm to use when performing HTTP retries

        # -------------- if type is static ---------------

        interval: # [number; maximum: 20000] Wait (ms) - Time interval between retries. Maximum allowed value is 20,000 ms (1/3 minute).
        limit: # [number; maximum: 20] Retry limit - Maximum number of times to retry a failed HTTP request
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to the `Longest interval between retries (ms)` value, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
        retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
        retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

        # --------------------------------------------------------


        # -------------- if type is backoff ---------------

        interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between a failed request and the first retry
        limit: # [number; maximum: 20] Retry limit - Maximum number of times to retry a failed HTTP request
        multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. Example: base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on.
        maxIntervalMs: # [number] Longest interval between retries (ms)
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to the `Longest interval between retries (ms)` value, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
        retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
        retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset (ECONNRESET) error occurs

        # --------------------------------------------------------

      __scheduling: # [object] 
        stateTracking: # [object] 
          enabled: # [boolean] Enabled - Track collection progress between consecutive scheduled executions

          # -------------- if enabled is true ---------------

          stateUpdateExpression: # [string] State update expression - JavaScript expression that defines how to update the state from an event. Use the event's data and the current state to compute the new state. See [Understanding State Expression Fields](https://docs.cribl.io/stream/collectors-rest#state-tracking-expression-fields) for more information.
          stateMergeExpression: # [string] State merge expression - JavaScript expression that defines which state to keep when merging a task's newly reported state with previously saved state. Evaluates `prevState` and `newState` variables, resolving to the state to keep.

          # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if type is s3 ---------------

    conf: # [object] [required] 
      type: # [string] 
      outputName: # [string] Auto-populate from - Name of the predefined Destination that will be used to auto-populate Collector settings
      bucket: # [string] S3 bucket - S3 Bucket from which to collect data
      parquetChunkSizeMB: # [number; minimum: 1, maximum: 100] Parquet chunk size limit (MB) - Maximum file size for each Parquet chunk
      parquetChunkDownloadTimeout: # [number; minimum: 1, maximum: 3600] Parquet chunk download timeout (seconds) - Maximum time allowed for downloading a Parquet chunk. Processing will stop if a chunk cannot be downloaded within the time specified.
      region: # [string] Region - Region from which to retrieve data
      path: # [string] Path - Directory where data will be collected. Templating (such as 'myDir/${datacenter}/${host}/${app}/') and time-based tokens (such as 'myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/') are supported. Can be a constant (enclosed in quotes) or a JavaScript expression.
      partitioningScheme: # [string] Partitioning scheme - Partitioning scheme used for this dataset. Using a known scheme like DDSS enables more efficient data reading and retrieval.

      # -------------- if partitioningScheme is none ---------------

      recurse: # [boolean] Recursive - Traverse and include files from subdirectories. Leave this option enabled to ensure that all nested directories are searched and their contents collected.

      # --------------------------------------------------------

      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. For example, given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field.
        - key: # [string] Token - A token from the template path, such as epoch
          expression: # [string] Extractor Expression - JavaScript expression that receives token under "value" variable, and evaluates to populate event fields. Example: {date: new Date(+value*1000)}
      awsAuthenticationMethod: # [string] Authentication method - AWS authentication method. Choose Auto to use IAM roles.

      # -------------- if awsAuthenticationMethod is manual ---------------

      awsApiKey: # [string] Access key - Access key. If not present, will fall back to env.AWS_ACCESS_KEY_ID, or to the metadata endpoint for IAM creds. Optional when running on AWS. This value can be a constant or a JavaScript expression.
      awsSecretKey: # [string] Secret key - Secret key. If not present, will fall back to env.AWS_SECRET_ACCESS_KEY, or to the metadata endpoint for IAM creds. Optional when running on AWS. This value can be a constant or a JavaScript expression.

      # --------------------------------------------------------


      # -------------- if awsAuthenticationMethod is secret ---------------

      awsSecret: # [string] Secret key pair - Select or create a stored secret that references AWS access key and secret key.

      # --------------------------------------------------------

      endpoint: # [string] Endpoint - Must point to an S3-compatible endpoint. If empty, defaults to an AWS region-specific endpoint. 
      signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests
      enableAssumeRole: # [boolean] Enable Assume Role - Use AssumeRole credentials
      assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume
      assumeRoleExternalId: # [string] External ID - External ID to use when assuming role
      durationSeconds: # [number; minimum: 900, maximum: 43200] Duration (seconds) - Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes), default is 3600 (1 hour), and maximum is 43200 (12 hours).
      maxBatchSize: # [number; minimum: 1] Batch size limit (objects) - Maximum number of metadata objects to batch before recording as results
      reuseConnections: # [boolean] Reuse connections - Reuse connections between requests to improve performance
      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as a self-signed certificate)
      verifyPermissions: # [boolean] Verify bucket permissions - Disable if you can access files within the bucket but not the bucket itself. Resolves errors of the form "discover task initialization failed...error: Forbidden".
      disableTimeFilter: # [boolean] Disable time filter - Disable Collector event time filtering when a date range is specified

    # --------------------------------------------------------


    # -------------- if type is script ---------------

    conf: # [object] [required] 
      type: # [string] 
      discoverScript: # [string] Discover Script - Script to discover what to collect. Should output one task per line in stdout.
      collectScript: # [string] [required] Collect Script - Script to run to perform data collections. Task passed in as $CRIBL_COLLECT_ARG. Should output results to stdout.
      shell: # [string] Shell - Shell to use to execute scripts.
      envVars: # [array] Environment Variables - Environment variables to expose to the discover and collect scripts.
        - name: # [string] Name - Environment variable name
          value: # [string] Value - JavaScript expression to compute environment variable's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

    # --------------------------------------------------------


    # -------------- if type is splunk ---------------

    conf: # [object] [required] 
      type: # [string] 
      searchHead: # [string] [required] Search head - Search head base URL. Can be an expression. Default is https://localhost:8089.
      search: # [string] Search - Examples: 'index=myAppLogs level=error channel=myApp' OR '| mstats avg(myStat) as myStat WHERE index=myStatsIndex.'
      earliest: # [string] Earliest - The earliest time boundary for the search. Can be an exact or relative time. Examples: '2022-01-14T12:00:00Z' or '-16m@m'
      latest: # [string] Latest - The latest time boundary for the search. Can be an exact or relative time. Examples: '2022-01-14T12:00:00Z' or '-1m@m'
      endpoint: # [string] [required] Search endpoint - REST API used to create a search
      outputMode: # [string] [required] Output mode - Format of the returned output
      collectRequestParams: # [array] Extra parameters - Optional collect request parameters
        - name: # [string] Parameter Name
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values without delimiters (earliest) are evaluated as strings.
      collectRequestHeaders: # [array] Extra headers - Optional collect request headers
        - name: # [string] Header Name
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (`${earliest}`). If a constant, use single quotes ('earliest'). Values without delimiters (earliest) are evaluated as strings.
      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls

      # -------------- if authentication is basic ---------------

      username: # [string] Username - Basic authentication username
      password: # [string] Password - Basic authentication password

      # --------------------------------------------------------


      # -------------- if authentication is basicSecret ---------------

      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your credentials

      # --------------------------------------------------------


      # -------------- if authentication is token ---------------

      token: # [string] Bearer token

      # --------------------------------------------------------


      # -------------- if authentication is tokenSecret ---------------

      tokenSecret: # [string] Bearer token secret - Select or create a stored secret that references your Bearer token

      # --------------------------------------------------------


      # -------------- if authentication is login ---------------

      loginUrl: # [string] Login URL - URL to use for login API call. This call is expected to be a POST.
      username: # [string] Username
      password: # [string] Password
      loginBody: # [string] POST body - Template for POST body to send with login request. ${username} and ${password} are used to specify location of these attributes in the message.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are allowed.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call, this call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST Body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token Attribute - Path to token attribute in login response body. Nested attributes are allowed.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.

      # --------------------------------------------------------

      timeout: # [number; maximum: 1800] Request timeout (secs) - HTTP request inactivity timeout. Use 0 for no timeout.
      useRoundRobinDns: # [boolean] Round-robin DNS - Use round-robin DNS lookup. Suitable when DNS server returns multiple addresses in sort order.
      disableTimeFilter: # [boolean] Disable time filter - Disable collector event time filtering when a date range is specified
      rejectUnauthorized: # [boolean] Reject unauthorized certificates - Reject certificates that cannot be verified against a valid CA (such as self-signed certificates)
      retryRules: # [object] 
        type: # [string] Retry type - Algorithm to use when performing HTTP retries

        # -------------- if type is static ---------------

        interval: # [number; maximum: 20000] Wait (ms) - Time interval between retries. Maximum allowed value is 20,000 ms (1/3 minute).
        limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
        retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
        retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset error (ECONNRESET) error occurs

        # --------------------------------------------------------


        # -------------- if type is backoff ---------------

        interval: # [number; maximum: 20000] Initial retry interval (ms) - Time interval between failed request and first retry (kickoff). Maximum allowed value is 20,000 ms (1/3 minute).
        limit: # [number; maximum: 20] Retry limit - The maximum number of times to retry a failed HTTP request
        multiplier: # [number; minimum: 1, maximum: 20] Backoff multiplier - Base for exponential backoff. For example, base 2 means that retries will occur after 2, then 4, then 8 seconds, and so on.
        codes: # [array of numbers] Retry HTTP codes - List of HTTP codes that trigger a retry. Leave empty to use the default list of 429 and 503.
        enableHeader: # [boolean] Honor Retry-After header - Honor any Retry-After header that specifies a delay (in seconds) or a timestamp after which to retry the request. The delay is limited to 20 seconds, even if the Retry-After header specifies a longer delay. When disabled, all Retry-After headers are ignored.
        retryConnectTimeout: # [boolean] Retry connection timeout - Make a single retry attempt when a connection timeout (ETIMEDOUT) error occurs
        retryConnectReset: # [boolean] Retry connection reset - Retry request when a connection reset error (ECONNRESET) error occurs

        # --------------------------------------------------------


    # --------------------------------------------------------

    destructive: # [boolean] Destructive - Delete any files collected (where applicable)
    encoding: # [string] Encoding - Character encoding to use when parsing ingested data. When not set, Cribl Stream will default to UTF-8 but may incorrectly interpret multi-byte characters.
  input: # [object] 
    type: # [string] 
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event-breaking rulesets that will be applied, in order, to the input data stream
    staleChannelFlushMs: # [number; minimum: 10, maximum: 43200000] Event Breaker buffer timeout (ms) - How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel before flushing the data stream out, as is, to the Pipelines
    sendToRoutes: # [boolean] Send to Routes - Send events to normal routing and event processing. Disable to select a specific Pipeline/Destination combination.

    # -------------- if sendToRoutes is true ---------------

    pipeline: # [string] Pre-Processing Pipeline - An optional Pipeline to process results before sending to Routes

    # --------------------------------------------------------


    # -------------- if sendToRoutes is false ---------------

    pipeline: # [string] Pipeline - Pipeline to process results
    output: # [string] Destination - Destination to send results to

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
  description: # [string] Description
  type: # [string] Job type
  ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
  ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
  removeFields: # [array of strings] Remove Discover fields - List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.
  resumeOnBoot: # [boolean] Resume job on boot - Resume the ad hoc job if a failure condition causes Stream to restart during job execution
  environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  schedule: # [object] Schedule - Configuration for a scheduled job
    enabled: # [boolean] Enabled - Enable to configure scheduling for this Collector

    # -------------- if enabled is true ---------------

    cronSchedule: # [string] Cron schedule - A cron schedule on which to run this job
    maxConcurrentRuns: # [number; minimum: 1] Concurrent run limit - The maximum number of instances of this scheduled job that may be running at any time
    skippable: # [boolean] Skippable - Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits
    run: # [object] Run settings

      # -------------- if type is collection ---------------

      rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors
      maxTaskReschedule: # [number; minimum: 1] Task reschedule limit - Maximum number of times a task can be rescheduled
      logLevel: # [string] Log level - Level at which to set task logging
      jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.
      mode: # [string] Mode - Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.

      # -------------- if mode is list ---------------

      discoverToRoutes: # [boolean] Send to Routes - Send discover results to Routes

      # --------------------------------------------------------


      # -------------- if mode is preview ---------------

      capture: # [object] Capture Settings
        duration: # [number] Capture time (sec) - Amount of time to keep capture open, in seconds
        maxEvents: # [number; minimum: 1, maximum: 10000] Capture up to N events - Maximum number of events to capture
        level: # [string] Where to capture

      # --------------------------------------------------------

      timeRangeType: # [string] Time range

      # -------------- if timeRangeType is absolute ---------------

      timestampTimezone: # [string] Range timezone - Timezone to use for Earliest and Latest times

      # --------------------------------------------------------

      earliest: # [number,string] Earliest - Earliest time to collect data for the selected timezone
      latest: # [number,string] Latest - Latest time to collect data for the selected timezone
      timeWarning: # [object] 
      expression: # [string] Filter - A filter for tokens in the provided collect path and/or the events being collected
      minTaskSize: # [string] Lower task bundle size - Limits the bundle size for small tasks. For example,
        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.
      maxTaskSize: # [string] Upper task bundle size - Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,
        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.

    # --------------------------------------------------------

  streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
executor_job: # [object] 
  executor: # [object] 
    type: # [string] Executor type - The type of executor to run
    storeTaskResults: # [boolean] Store task results - Determines whether or not to write task results to disk
    conf: # [object] Executor-specific settings
  description: # [string] Description
  type: # [string] Job type
  ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
  ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
  removeFields: # [array of strings] Remove Discover fields - List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.
  resumeOnBoot: # [boolean] Resume job on boot - Resume the ad hoc job if a failure condition causes Stream to restart during job execution
  environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  schedule: # [object] Schedule - Configuration for a scheduled job
    enabled: # [boolean] Enabled - Enable to configure scheduling for this Collector

    # -------------- if enabled is true ---------------

    cronSchedule: # [string] Cron schedule - A cron schedule on which to run this job
    maxConcurrentRuns: # [number; minimum: 1] Concurrent run limit - The maximum number of instances of this scheduled job that may be running at any time
    skippable: # [boolean] Skippable - Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits
    run: # [object] Run settings

      # -------------- if type is collection ---------------

      rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors
      maxTaskReschedule: # [number; minimum: 1] Task reschedule limit - Maximum number of times a task can be rescheduled
      logLevel: # [string] Log level - Level at which to set task logging
      jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.
      mode: # [string] Mode - Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.

      # -------------- if mode is list ---------------

      discoverToRoutes: # [boolean] Send to Routes - Send discover results to Routes

      # --------------------------------------------------------


      # -------------- if mode is preview ---------------

      capture: # [object] Capture Settings
        duration: # [number] Capture time (sec) - Amount of time to keep capture open, in seconds
        maxEvents: # [number; minimum: 1, maximum: 10000] Capture up to N events - Maximum number of events to capture
        level: # [string] Where to capture

      # --------------------------------------------------------

      timeRangeType: # [string] Time range

      # -------------- if timeRangeType is absolute ---------------

      timestampTimezone: # [string] Range timezone - Timezone to use for Earliest and Latest times

      # --------------------------------------------------------

      earliest: # [number,string] Earliest - Earliest time to collect data for the selected timezone
      latest: # [number,string] Latest - Latest time to collect data for the selected timezone
      timeWarning: # [object] 
      expression: # [string] Filter - A filter for tokens in the provided collect path and/or the events being collected
      minTaskSize: # [string] Lower task bundle size - Limits the bundle size for small tasks. For example,
        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.
      maxTaskSize: # [string] Upper task bundle size - Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,
        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.

    # --------------------------------------------------------

  streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
scheduledSearch_job: # [object] 
  savedQueryId: # [string] ID of the SavedQuery - Identifies which search query to run
  description: # [string] Description
  type: # [string] Job type
  ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
  ignoreGroupJobsLimit: # [boolean] Ignore Worker Group job limits - When enabled, this job's artifacts are not counted toward the Worker Group's finished job artifacts limit. Artifacts will be removed only after the Collector's configured time to live.
  removeFields: # [array of strings] Remove Discover fields - List of fields to remove from Discover results. Wildcards (for example, aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.
  resumeOnBoot: # [boolean] Resume job on boot - Resume the ad hoc job if a failure condition causes Stream to restart during job execution
  environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  schedule: # [object] Schedule - Configuration for a scheduled job
    enabled: # [boolean] Enabled - Enable to configure scheduling for this Collector

    # -------------- if enabled is true ---------------

    cronSchedule: # [string] Cron schedule - A cron schedule on which to run this job
    maxConcurrentRuns: # [number; minimum: 1] Concurrent run limit - The maximum number of instances of this scheduled job that may be running at any time
    skippable: # [boolean] Skippable - Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits
    run: # [object] Run settings

      # -------------- if type is collection ---------------

      rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors
      maxTaskReschedule: # [number; minimum: 1] Task reschedule limit - Maximum number of times a task can be rescheduled
      logLevel: # [string] Log level - Level at which to set task logging
      jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run. Time unit defaults to seconds if not specified (examples: 30, 45s, 15m). Enter 0 for unlimited time.
      mode: # [string] Mode - Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.

      # -------------- if mode is list ---------------

      discoverToRoutes: # [boolean] Send to Routes - Send discover results to Routes

      # --------------------------------------------------------


      # -------------- if mode is preview ---------------

      capture: # [object] Capture Settings
        duration: # [number] Capture time (sec) - Amount of time to keep capture open, in seconds
        maxEvents: # [number; minimum: 1, maximum: 10000] Capture up to N events - Maximum number of events to capture
        level: # [string] Where to capture

      # --------------------------------------------------------

      timeRangeType: # [string] Time range

      # -------------- if timeRangeType is absolute ---------------

      timestampTimezone: # [string] Range timezone - Timezone to use for Earliest and Latest times

      # --------------------------------------------------------

      earliest: # [number,string] Earliest - Earliest time to collect data for the selected timezone
      latest: # [number,string] Latest - Latest time to collect data for the selected timezone
      timeWarning: # [object] 
      expression: # [string] Filter - A filter for tokens in the provided collect path and/or the events being collected
      minTaskSize: # [string] Lower task bundle size - Limits the bundle size for small tasks. For example,
        if your lower bundle size is 1MB, you can bundle up to five 200KB files into one task.
      maxTaskSize: # [string] Upper task bundle size - Limits the bundle size for files above the lower task bundle size. For example, if your upper bundle size is 10MB,
        you can bundle up to five 2MB files into one task. Files greater than this size will be assigned to individual tasks.

    # --------------------------------------------------------

  streamtags: # [array of strings] Tags - Tags for filtering and grouping in Cribl Stream
```

> The `workerAffinity` internal parameter defaults to `false`. Cribl automatically sets this to `true` on certain jobs, specifying that collection tasks will be both created and run by the same Worker Node.
>
{.box .info}
