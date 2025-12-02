# jobs.yml


`jobs.yml` maintains parameters for configured [Collectors](/stream/collectors), corresponding to those listed on the **Collectors** page. Each collection job is listed according to the pattern shown below.

```yaml {title="$CRIBL_HOME/local/cribl/jobs.yml"}
collection_job: # [object] 
  workerAffinity: # [boolean] Worker affinity - If enabled tasks are created and run by the same worker node.
  collector: # [object] 
    type: # [string] Collector type - The type of collector to run.

    # -------------- if type is azure_blob ---------------

    conf: # [object] [required] 
      outputName: # [string] Auto-populate from - The name of the predefined Destination that will be used to auto-populate collector settings.
      authType: # [string] Authentication method - Enter authentication data directly, or select a secret referencing your auth data

      # -------------- if authType is manual ---------------

      connectionString: # [string] Connection string - Enter your Azure storage account Connection String. If left blank, Cribl Stream will fall back to env.AZURE_STORAGE_CONNECTION_STRING.

      # --------------------------------------------------------


      # -------------- if authType is secret ---------------

      textSecret: # [string] Connection string (text secret) - Text secret

      # --------------------------------------------------------

      containerName: # [string] Container name - Name of the container to collect from. This value can be a constant or a JavaScript expression that can only be evaluated at init time. E.g. referencing a Global Variable: `myBucket-${C.vars.myVar}`.
      path: # [string] Path - The directory from which to collect data. Templating is supported, e.g.: myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, e.g.: myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. E.g.: given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field
        - key: # [string] Token - A token from the template path, e.g.: epoch
          expression: # [string] Extractor expression - JS expression that receives token under "value" variable, and evaluates to populate event fields, e.g.: {date: new Date(+value*1000)}
      recurse: # [boolean] Recursive - Whether to recurse through subdirectories.
      maxBatchSize: # [number] Max Batch Size (objects) - Maximum number of metadata objects to batch before recording as results.

    # --------------------------------------------------------


    # -------------- if type is filesystem ---------------

    conf: # [object] [required] 
      outputName: # [string] Auto-populate from - Select a predefined configuration (e.g., a Destination) to auto-populate collector settings.
      path: # [string] Directory - The directory from which to collect data. Templating is supported, e.g.: /myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, e.g.: /myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. E.g.: given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field
        - key: # [string] Token - A token from the template directory, e.g.: epoch
          expression: # [string] Extractor expression - JS expression that receives token under "value" variable, and evaluates to populate event fields, e.g.: {date: new Date(+value*1000)}
      recurse: # [boolean] Recursive - Whether to recurse through subdirectories.
      maxBatchSize: # [number] Max Batch Size (files) - Maximum number of metadata files to batch before recording as results.

    # --------------------------------------------------------


    # -------------- if type is google_cloud_storage ---------------

    conf: # [object] [required] 
      outputName: # [string] Auto-populate from - The name of the predefined Destination that will be used to auto-populate collector settings.
      bucket: # [string] Bucket name - Name of the bucket to collect from. This value can be a constant or a JavaScript expression that can only be evaluated at init time. E.g. referencing a Global Variable: `myBucket-${C.vars.myVar}`.
      path: # [string] Path - The directory from which to collect data. Templating is supported, e.g.: myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, e.g.: myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. E.g.: given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field
        - key: # [string] Token - A token from the template path, e.g.: epoch
          expression: # [string] Extractor expression - JS expression that receives token under "value" variable, and evaluates to populate event fields, e.g.: {date: new Date(+value*1000)}
      endpoint: # [string] Endpoint - Google Cloud Storage service endpoint. If empty, the endpoint will be automatically constructed using service credentials.
      disableTimeFilter: # [boolean] Disable time filter - Used to disable collector event time filtering when a date range is specified.
      recurse: # [boolean] Recursive - Whether to recurse through subdirectories.
      maxBatchSize: # [number] Max Batch Size (objects) - Maximum number of metadata objects to batch before recording as results.
      authType: # [string] Authentication method - Enter account credentials manually, or select a secret that references your credentials.

      # -------------- if authType is manual ---------------

      serviceAccountCredentials: # [string] Service account credentials - Contents of Google Cloud service account credentials (JSON keys) file. To upload a file, click the upload button at this field's upper right.

      # --------------------------------------------------------


      # -------------- if authType is secret ---------------

      textSecret: # [string] Service account credentials (text secret) - Select or create a stored text secret that references your credentials.

      # --------------------------------------------------------


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
        discoverDataField: # [string] Discover data field - Within the response JSON, name of the field or array element to pull results from. LeaveÂ blank if the result is an array of values. Sample entry: items, json: { items: [{id: 'first'},{id: 'second'}] }

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
      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls. You can specify API Keyâbased authentication by adding the appropriate CollectÂ headers.

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
      loginBody: # [string] POST Body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token Attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication Headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header value (can be a constant).

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call, this call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST Body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token Attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication Headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header value (can be a constant).

      # --------------------------------------------------------


      # -------------- if authentication is oauth ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      clientSecretParamValue: # [string] Client secret value - Secret value to add to HTTP requests as the 'client secret' parameter. Stored on disk encrypted, and is automatically added to request parameters
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if authentication is oauthSecret ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      textSecret: # [string] Client secret value (text secret) - Select or create a text secret that contains the client secret's value.
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------

      timeout: # [number] Request Timeout (secs) - HTTP request inactivity timeout, use 0 to disable
      defaultBreakers: # [string] Hidden Default Breakers
      safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.

    # --------------------------------------------------------


    # -------------- if type is office365_mgmt ---------------

    conf: # [object] [required] 
      plan_type: # [string] Subscription plan - Office 365 subscription plan for your organization, typically Enterprise
      tenant_id: # [string] [required] Tenant identifier - Directory ID (tenant identifier) in Microsoft Entra ID
      app_id: # [string] [required] Application identifier - Identifier of the registered application in Microsoft Entra ID.
      client_secret: # [string] [required] Client secret - Application key of the registered application.
      publisher_identifier: # [string] Publisher identifier - Optional PublisherIdentifier to use in API requests, defaults to tenant id if not defined.
      content_type: # [string] [required] Content type - The type of content to retrieve from the Office 365 management communications API.

    # --------------------------------------------------------


    # -------------- if type is office365_service ---------------

    conf: # [object] [required] 
      tenant_id: # [string] Tenant identifier - Directory ID (tenant identifier) in Microsoft Entra ID
      app_id: # [string] [required] Application identifier - Identifier of the registered application in Microsoft Entra ID.
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
      scrapePort: # [number] Metrics Port - The port number in the metrics URL for discovered targets.
      scrapePath: # [string] Metrics Path - Path to use when collecting metrics from discovered targets
      searchFilter: # [array] Search Filter - EC2 Instance Search Filter
        - Name: # [string] Filter Name - Search filter attribute name, see: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html for more information. Attributes can be manually entered if not present in the drop down list
          Values: # [array of strings] Filter Values - Search Filter Values
      region: # [string] Region - Region from which to retrieve data.
      awsAuthenticationMethod: # [string] Authentication Method - AWS authentication method
      enableAssumeRole: # [boolean] Enable Assume Role - Use Assume Role credentials
      endpoint: # [string] Endpoint - EC2 service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, used to point to a EC2-compatible endpoint.
      signatureVersion: # [string] Signature version - Signature version to use for signing EC2 requests

      # --------------------------------------------------------


    # --------------------------------------------------------


    # -------------- if type is rest ---------------

    conf: # [object] [required] 
      discovery: # [object] 
        discoverType: # [string] Discover type - Defines how task discovery will be performed. Use None to skip the discovery. Use HTTP Request to make a REST call to discover tasks. Use Item List to enumerate items for collect to retrieve. Use JSON Response to manually define discover tasks as a JSON array of objects. Each entry returned by the discover operation will result in a collect task.

        # -------------- if discoverType is http ---------------

        discoverUrl: # [string] Discover URL - Expression to derive URL to use for the Discover operation (can be a constant).
        discoverMethod: # [string] Discover method - Discover HTTP method.
        discoverRequestHeaders: # [array] Discover headers - Optional discover request headers.
          - name: # [string] Name - Header name.
            value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
        discoverDataField: # [string] Discover data field - Path to field in the response object which contains discover results (e.g.: level1.name), leave blank if the result is an array.

        # --------------------------------------------------------


        # -------------- if discoverType is json ---------------

        manualDiscoverResult: # [string] Discover result - Allows hard-coding the Discover result. Must be a JSON object. Works with the Discover Data field.
        discoverDataField: # [string] Discover data field - Within the response JSON, name of the field or array element to pull results from. LeaveÂ blank if the result is an array of values. Sample entry: items, json: { items: [{id: 'first'},{id: 'second'}] }

        # --------------------------------------------------------


        # -------------- if discoverType is list ---------------

        itemList: # [array of strings] Discover items - Comma-separated list of items to return from the Discover task. Each item returned will generate a collect task, and can be referenced using `${id}` in the collect URL, headers, or parameters.

        # --------------------------------------------------------

      collectUrl: # [string] Collect URL - Expression to derive URL to use for the collect operation (can be a constant).
      collectMethod: # [string] [required] Collect method - Collect HTTP method.

      # -------------- if collectMethod is get ---------------

      collectRequestParams: # [array] Collect parameters - Optional collect request parameters.
        - name: # [string] Name - Parameter name
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if collectMethod is post ---------------

      collectRequestParams: # [array] Collect parameters - Optional collect request parameters.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if collectMethod is post_with_body ---------------

      collectBody: # [string] Collect POST body - Template for POST body to send with the Collect request. Reference global variables, functions, or parameters from the Discover response using template params: `${C.vars.myVar}`, or `${Date.now()}`, `${param}`.

      # --------------------------------------------------------


      # -------------- if collectMethod is other ---------------

      collectVerb: # [string] Collect verb - DIY HTTP verb to use for the collect operation.
      collectBody: # [string] Collect body - Template for body to send with the Collect request. Reference global variables, functions, or parameters from the Discover response using template params: `${C.vars.myVar}`, or `${Date.now()}`, `${param}`
      collectRequestParams: # [array] Collect parameters - Optional collect request parameters.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------

      collectRequestHeaders: # [array] Collect headers - Optional collect request headers.
        - name: # [string] Name - Header Name
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      pagination: # [object] 
        type: # [string] Pagination - Select collect pagination scheme

        # -------------- if type is response_body ---------------

        attribute: # [array,string] Response attribute - The name of the attribute in the response that contains next page information
        maxPages: # [number] Max pages - The maximum number of pages to retrieve, set to 0 to retrieve all pages

        # --------------------------------------------------------


        # -------------- if type is response_header_link ---------------

        nextRelationAttribute: # [string] Next page relation name - Relation name used in the link header that refers to the next page in the result set. In this example link header, rel="next" to the next page of results: <https://myHost/curPage>; rel="self" <https://myHost/nextPage>; rel="next"
        curRelationAttribute: # [string] Current page relation name - Optional relation name used in the link header that refers to the current result set. In this example link header, rel="self" refers to the current page of results: <https://myHost/curPage>; rel="self" <https://myHost/nextPage>; rel="next"
        maxPages: # [number] Max pages - The maximum number of pages to retrieve, set to 0 to retrieve all pages

        # --------------------------------------------------------


        # -------------- if type is request_offset ---------------

        offsetField: # [string] Offset field name - Query string parameter that sets the index from which to begin returning records. E.g.: /api/v1/query?term=cribl&limit=100&offset=0
        offset: # [number] Starting offset - Offset index from which to start request. Defaults to undefined, which will start collection from the first record.
        offsetSpacer: # [null] 
        limitField: # [string] Limit field name - Query string parameter to set the number of records retrieved per request. E.g.: /api/v1/query?term=cribl&limit=100&offset=0
        limit: # [number] Limit - Maximum number of records to collect per request.
        limitSpacer: # [null] 
        totalRecordField: # [string] Total record count field name - Identifies the attribute in the response that contains the total number of records for the query.
        maxPages: # [number] Max pages - The maximum number of pages to retrieve. Set to 0 to retrieve all pages.
        zeroIndexed: # [boolean] Zero-based index -  Toggle on to indicate that the first record in the requested data is at index 0. The default (No) indicates index 1.

        # --------------------------------------------------------


        # -------------- if type is request_page ---------------

        pageField: # [string] Page number field name - Query string parameter that sets the page index to be returned. E.g.: /api/v1/query?term=cribl&page_size=100&page_number=0
        page: # [number] Starting page number - Page number from which to start request. Defaults to undefined, which will start collection from the first page.
        offsetSpacer: # [null] 
        sizeField: # [string] Page size field name - Query string parameter to set the number of records retrieved per request. E.g.: /api/v1/query?term=cribl&page_size=100&page_number=0
        size: # [number] Page size - Maximum number of records to collect per page.
        limitSpacer: # [null] 
        totalPageField: # [string] Total page count field name - The name of the attribute in the response that contains the total number of pages for the query.
        totalRecordField: # [string] Total record count field name - Identifies the attribute in the response that contains the total number of records for the query.
        maxPages: # [number] Max pages - The maximum number of pages to retrieve. Set to 0 to retrieve all pages.
        zeroIndexed: # [boolean] zero-based index -  Toggle on to indicate that the first page in the requested data is at index 0. The default (No) indicates index 1.

        # --------------------------------------------------------

      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls. You can specify API Keyâbased authentication by adding the appropriate CollectÂ headers.

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
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call, this call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if authentication is oauth ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      clientSecretParamValue: # [string] Client secret value - Secret value to add to HTTP requests as the 'client secret' parameter. Stored on disk encrypted, and is automatically added to request parameters
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------


      # -------------- if authentication is oauthSecret ---------------

      loginUrl: # [string] Login URL - URL to use for the OAuth API call. This call is expected to be a POST.
      tokenRespAttribute: # [string] Token attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.
      clientSecretParamName: # [string] Client secret parameter - Parameter name that contains client secret. Defaults to 'client_secret', and is automatically added to request parameters.
      textSecret: # [string] Client secret value (text secret) - Select or create a text secret that contains the client secret's value.
      authRequestParams: # [array] Extra authentication parameters - OAuth request parameters added to the POST body. The Content-Type header will automatically be set to application/x-www-form-urlencoded.
        - name: # [string] Name - Parameter name.
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      authRequestHeaders: # [array] Authentication headers - Optional authentication request headers.
        - name: # [string] Name - Header name.
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.

      # --------------------------------------------------------

      timeout: # [number] Request timeout (secs) - HTTP request inactivity timeout, use 0 to disable
      useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. Suitable when DNS server returns multiple addresses in sort order.
      disableTimeFilter: # [boolean] Disable time filter - Used to disable collector event time filtering when a date range is specified.
      safeHeaders: # [array of strings] Safe headers - List of headers that are safe to log in plain text.

    # --------------------------------------------------------


    # -------------- if type is s3 ---------------

    conf: # [object] [required] 
      outputName: # [string] Auto-populate from - The name of the predefined Destination that will be used to auto-populate collector settings.
      bucket: # [string] S3 bucket - S3 Bucket from which to collect data.
      region: # [string] Region - Region from which to retrieve data.
      path: # [string] Path - The directory from which to collect data. Templating is supported, e.g.: myDir/${datacenter}/${host}/${app}/. Time-based tokens are also supported, e.g.: myOtherDir/${_time:%Y}/${_time:%m}/${_time:%d}/
      extractors: # [array] Path extractors - Allows using template tokens as context for expressions that enrich discovery results. E.g.: given a template /path/${epoch}, an extractor under key "epoch" with an expression {date: new Date(+value*1000)}, will enrich discovery results with a human readable "date" field
        - key: # [string] Token - A token from the template path, e.g.: epoch
          expression: # [string] Extractor expression - JS expression that receives token under "value" variable, and evaluates to populate event fields, e.g.: {date: new Date(+value*1000)}
      awsAuthenticationMethod: # [string] Authentication Method - AWS authentication method. Choose Auto to use IAM roles.

      # -------------- if awsAuthenticationMethod is manual ---------------

      awsApiKey: # [string] Access key - Access key. If not present, will fall back to env.AWS_ACCESS_KEY_ID, or to the metadata endpoint for IAM creds. Optional when running on AWS.
      awsSecretKey: # [string] Secret key - Secret key. If not present, will fall back to env.AWS_SECRET_ACCESS_KEY, or to the metadata endpoint for IAM creds. Optional when running on AWS.

      # --------------------------------------------------------


      # -------------- if awsAuthenticationMethod is secret ---------------

      awsSecret: # [string] Secret key pair - Select or create a stored secret that references AWS access key and secret key.

      # --------------------------------------------------------

      endpoint: # [string] Endpoint - S3 service endpoint. If empty, the endpoint will be automatically constructed from the Region.
      signatureVersion: # [string] Signature version - Signature version to use for signing S3 requests.
      enableAssumeRole: # [boolean] Enable Assume Role - Use Assume Role credentials.
      assumeRoleArn: # [string] AssumeRole ARN - Amazon Resource Name (ARN) of the role to assume.
      assumeRoleExternalId: # [string] External ID - External ID to use when assuming role.
      recurse: # [boolean] Recursive - Whether to recurse through subdirectories.
      maxBatchSize: # [number] Max Batch Size (objects) - Maximum number of metadata objects to batch before recording as results.
      reuseConnections: # [boolean] Reuse Connections - Whether to reuse connections between requests, which can improve performance.
      rejectUnauthorized: # [boolean] Reject Unauthorized Certificates - Whether to reject certificates that cannot be verified against a valid CA (e.g., self-signed certificates).
      verifyPermissions: # [boolean] Verify bucket permissions - Disable if you can access files within the bucket but not the bucket itself. Resolves errors of the form "discover task initialization failed...error: Forbidden".

    # --------------------------------------------------------


    # -------------- if type is script ---------------

    conf: # [object] [required] 
      discoverScript: # [string] Discover Script - Script to discover what to collect. Should output one task per line in stdout.
      collectScript: # [string] [required] Collect Script - Script to run to perform data collections. Task passed in as $CRIBL_COLLECT_ARG. Should output results to stdout.
      shell: # [string] Shell - Shell to use to execute scripts.

    # --------------------------------------------------------


    # -------------- if type is splunk ---------------

    conf: # [object] [required] 
      searchHead: # [string] [required] Search head - Search Head base URL, can be expression, default is https://localhost:8089.
      search: # [string] Search - Enter Splunk search here. For example: 'index=myAppLogs level=error channel=myApp' OR '| mstats avg(myStat) as myStat WHERE index=myStatsIndex.'
      earliest: # [string] Earliest - The earliest time boundary for the search. Can be an exact or relative time. For example: '2022-01-14T12:00:00Z' or '-16m@m'
      latest: # [string] Latest - The latest time boundary for the search. Can be an exact or relative time. For example: '2022-01-14T12:00:00Z' or '-1m@m'
      endpoint: # [string] [required] Search endpoint - REST API used to create a search.
      outputMode: # [string] [required] Output mode - Format of the returned output
      collectRequestParams: # [array] Extra parameters - Optional collect request parameters.
        - name: # [string] Name - Parameter name
          value: # [string] Value - JavaScript expression to compute the parameter's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      collectRequestHeaders: # [array] Extra headers - Optional collect request headers.
        - name: # [string] Name - Header Name
          value: # [string] Value - JavaScript expression to compute the header's value, normally enclosed in backticks (e.g.,Â `${earliest}`). IfÂ a constant, use single quotes (e.g.,Â 'earliest'). ValuesÂ without delimiters (e.g.,Â earliest) are evaluated as strings.
      authentication: # [string] [required] Authentication - Authentication method for Discover and Collect REST calls.

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
      loginBody: # [string] POST Body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token Attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.

      # --------------------------------------------------------


      # -------------- if authentication is loginSecret ---------------

      loginUrl: # [string] Login URL - URL to use for login API call, this call is expected to be a POST.
      credentialsSecret: # [string] Credentials secret - Select or create a stored secret that references your login credentials
      loginBody: # [string] POST Body - Template for POST body to send with login request, ${username} and ${password} are used to specify location of these attributes in the message
      tokenRespAttribute: # [string] Token Attribute - Path to token attribute in login response body. Nested attributes are OK.
      authHeaderExpr: # [string] Authorize Expression - JavaScript expression to compute the Authorization header to pass in discover and collect calls. The value ${token} is used to reference the token obtained from login.

      # --------------------------------------------------------

      timeout: # [number] Request Timeout (secs) - HTTP request inactivity timeout, use 0 to disable
      useRoundRobinDns: # [boolean] Round-robin DNS - Enable to use round-robin DNS lookup. Suitable when DNS server returns multiple addresses in sort order.
      disableTimeFilter: # [boolean] Disable time filter - Used to disable collector event time filtering when a date range is specified.

    # --------------------------------------------------------

    destructive: # [boolean] Destructive - If set to Yes, the collector will delete any files that it collects (where applicable).
  input: # [object] 
    type: # [string] 
    breakerRulesets: # [array of strings] Event Breaker rulesets - A list of event breaking rulesets that will be applied, in order, to the input data stream.
    staleChannelFlushMs: # [number] Event Breaker buffer timeout - The amount of time (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing the data stream out, as-is, to the Pipelines.
    sendToRoutes: # [boolean] Send to Routes - If set to Yes, events will be sent to normal routing and event processing. Set to No if you want to select a specific Pipeline/Destination combination.

    # -------------- if sendToRoutes is true ---------------

    pipeline: # [string] Pre-Processing Pipeline - Pipeline to process results before sending to routes. Optional.

    # --------------------------------------------------------


    # -------------- if sendToRoutes is false ---------------

    pipeline: # [string] Pipeline - Pipeline to process results.
    output: # [string] Destination - Destination to send results to.

    # --------------------------------------------------------

    preprocess: # [object] 
      disabled: # [boolean] Disabled - Enable Custom Command
      command: # [string] Command - Command to feed the data through (via stdin) and process its output (stdout)
      args: # [array of strings] Arguments - Arguments
    throttleRatePerSec: # [string] Throttling - Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as KB, MB, GB, etc. (E.g., 42 MB.) Default value of 0 specifies no throttling.
    metadata: # [array] Fields - Fields to add to events from this input.
      - name: # [string] Name - Field name
        value: # [string] Value - JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)
  type: # [string] Job type - Job type.
  ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
  removeFields: # [array of strings] Remove Discover fields - List of fields to remove from Discover results. Wildcards (e.g.: aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.
  resumeOnBoot: # [boolean] Resume job on boot - Resumes the ad hoc job if a failure condition causes Stream to restart during job execution.
  environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  schedule: # [object] Schedule - Configuration for a scheduled job.
    enabled: # [boolean] Enabled - Determines whether or not this schedule is enabled.

    # -------------- if enabled is true ---------------

    cronSchedule: # [string] Cron schedule - A cron schedule on which to run this job.
    maxConcurrentRuns: # [number] Max concurrent runs - The maximum number of instances that may be running of this scheduled job at any given time.
    skippable: # [boolean] Skippable - Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits.
    run: # [object] Run settings

      # -------------- if type is collection ---------------

      rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors.
      maxTaskReschedule: # [number] Max task reschedule - Max number of times a task can be rescheduled.
      logLevel: # [string] Log Level - Level at which to set task logging.
      jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
      mode: # [string] Mode - Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.

      # -------------- if mode is list ---------------

      discoverToRoutes: # [boolean] Send to Routes - If true, send discover results to routes

      # --------------------------------------------------------


      # -------------- if mode is preview ---------------

      capture: # [object] Capture Settings
        duration: # [number] Capture Time (sec) - Amount of time to keep capture open, in seconds.
        maxEvents: # [number] Capture Up to N Events - Maximum number of events to capture.
        level: # [string] Where to capture

      # --------------------------------------------------------

      timeRangeType: # [string] Time range - Time range for scheduled job.
      earliest: # [number,string] Earliest - Earliest time, in local time.
      latest: # [number,string] Latest - Latest time, in local time.
      timestampTimezone: # [string] Range Timezone - Timezone to use for Earliest and Latest times (defaults to UTC).
      expression: # [string] Filter - A filter for tokens in the provided collect path and/or the events being collected
      minTaskSize: # [string] Lower task bundle size - Limits the bundle size for small tasks. E.g., bundle five 200KB files into one 1M task.
      maxTaskSize: # [string] Upper task bundle size - Limits the bundle size for files above the Lower task bundle size. E.g., bundle five 2MB files into one 10MB task bundle. Files greater than this size will be assigned to individual tasks.

    # --------------------------------------------------------

  streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
executor_job: # [object] 
  executor: # [object] 
    type: # [string] Executor type - The type of executor to run.
    storeTaskResults: # [boolean] Store task results - Determines whether or not to write task results to disk.
    conf: # [object] Executor-specific settings.
  type: # [string] Job type - Job type.
  ttl: # [string] Time to live - Time to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in the Job Inspector.
  removeFields: # [array of strings] Remove Discover fields - List of fields to remove from Discover results. Wildcards (e.g.: aws*) are allowed. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface.
  resumeOnBoot: # [boolean] Resume job on boot - Resumes the ad hoc job if a failure condition causes Stream to restart during job execution.
  environment: # [string] Environment - Optionally, enable this config only on a specified Git branch. If empty, will be enabled everywhere.
  schedule: # [object] Schedule - Configuration for a scheduled job.
    enabled: # [boolean] Enabled - Determines whether or not this schedule is enabled.

    # -------------- if enabled is true ---------------

    cronSchedule: # [string] Cron schedule - A cron schedule on which to run this job.
    maxConcurrentRuns: # [number] Max concurrent runs - The maximum number of instances that may be running of this scheduled job at any given time.
    skippable: # [boolean] Skippable - Skippable jobs can be delayed, up to their next run time, if the system is hitting concurrency limits.
    run: # [object] Run settings

      # -------------- if type is collection ---------------

      rescheduleDroppedTasks: # [boolean] Reschedule tasks - Reschedule tasks that failed with non-fatal errors.
      maxTaskReschedule: # [number] Max task reschedule - Max number of times a task can be rescheduled.
      logLevel: # [string] Log Level - Level at which to set task logging.
      jobTimeout: # [string] Job timeout - Maximum time the job is allowed to run (e.g., 30, 45s or 15m). Units are seconds, if not specified. Enter 0 for unlimited time.
      mode: # [string] Mode - Job run mode. Preview will either return up to N matching results, or will run until capture time T is reached. Discovery will gather the list of files to turn into streaming tasks, without running the data collection job. Full Run will run the collection job.

      # -------------- if mode is list ---------------

      discoverToRoutes: # [boolean] Send to Routes - If true, send discover results to routes

      # --------------------------------------------------------


      # -------------- if mode is preview ---------------

      capture: # [object] Capture Settings
        duration: # [number] Capture Time (sec) - Amount of time to keep capture open, in seconds.
        maxEvents: # [number] Capture Up to N Events - Maximum number of events to capture.
        level: # [string] Where to capture

      # --------------------------------------------------------

      timeRangeType: # [string] Time range - Time range for scheduled job.
      earliest: # [number,string] Earliest - Earliest time, in local time.
      latest: # [number,string] Latest - Latest time, in local time.
      timestampTimezone: # [string] Range Timezone - Timezone to use for Earliest and Latest times (defaults to UTC).
      expression: # [string] Filter - A filter for tokens in the provided collect path and/or the events being collected
      minTaskSize: # [string] Lower task bundle size - Limits the bundle size for small tasks. E.g., bundle five 200KB files into one 1M task.
      maxTaskSize: # [string] Upper task bundle size - Limits the bundle size for files above the Lower task bundle size. E.g., bundle five 2MB files into one 10MB task bundle. Files greater than this size will be assigned to individual tasks.

    # --------------------------------------------------------

  streamtags: # [array of strings] Tags - Add tags for filtering and grouping in Stream.
```

> The `workerAffinity` internal parameter defaults to `false`. Cribl automatically sets this to `true` on certain jobs, specifying that collection tasks will be both created and run by the same Edge Node.
>
{.box .info}
