# Connect Cribl Search to a Generic HTTP API


Configure Cribl Search to query any HTTP API.

---

This guide shows you how to set up a [Dataset Provider](basic-concepts#dataset-providers) and
[Datasets](basic-concepts#datasets) to query static or dynamic HTTP API endpoints.


> You can also quickly fetch data from an HTTP(S) URL by using the [`externaldata`](externaldata) operator.
{.box .info}

## Add a Generic HTTP API Dataset Provider {#provider}

A [Dataset Provider](basic-concepts#dataset-providers) tells Cribl Search where to query, and contains access
credentials.

To add a new HTTP API Dataset Provider:

1. In Cribl Search, select **Data**, then **Dataset Providers**, then **Add Provider**.
1. In **ID**, enter the ID for the new Dataset Provider. This is how you'll reference the provider when assigning your
   [HTTP API Datasets](#dataset) to it. Start the ID with a letter; the rest of the ID can use letters, numbers, and
   underscores (for example, `my_dataset_provider_1`).
1. In **Description**, you can add a few words to make your new Dataset Provider easy to recognize.
1. Set **Dataset Provider Type** to **Generic HTTP API**. You'll see two new sections: **Endpoints** and
   **Authentication**.
1. In **Endpoints**, specify the endpoints that your Datasets will be able to reference. For each endpoint, set the
   following:
   - **Name**: This is how you'll recognize the endpoint when setting up your HTTP API Datasets.
   - **Data field**: Enter the name of the field in the response JSON to pull data from. Leave blank if the result is
     not JSON or is an array of values. You can define [parameters](#parameters), in the form of `${parameterName}`.
   - **URL**: The URL for the endpoint. This is where Cribl Search will query for data. You can define
     [parameters](#parameters), in the form of `${parameterName}`.
   - **Method**: The HTTP method to use when making API requests.
   - **Headers**: Any HTTP headers needed to authenticate or authorize requests. <br>For example, you might need to add
     an `Authorization` header with a bearer token. You can define [parameters](#parameters), in the form of
     `${parameterName}`.
1. In **Authentication**, you can select and configure an optional [authentication method](#authentication) to use when
   making API requests. The default option is **None**.
1. When done, select **Save**.

Now, you can add a Dataset that tells Cribl Search what data to search from your new Dataset Provider.

## Add a Generic HTTP API Dataset {#dataset}

A [Dataset](basic-concepts#datasets) tells Cribl Search what data to search from the Dataset Provider.

To add a new HTTP API Dataset:

1. In Cribl Search, select **Data**, then **Datasets**, then **Add Dataset**.
1. In **ID**, enter the ID for the new Dataset. This is how you'll reference the Dataset to
   [search](build-a-search#syntax) it. Start the ID with a letter; the rest of the ID can use letters, numbers, and
   underscores (for example, `my_dataset_1`).
1. In **Description**, you can add a few words to make your new Dataset easy to recognize.
1. Set **Dataset Provider** to the **ID** of an [HTTP API Dataset Provider](#provider).
1. Select the endpoints that you want to search with this Dataset. <br>To configure more endpoints, go to the
   [Dataset Provider](#provider).
1. In **Processing**, you can apply rules for breaking data into discrete events. For more information, see
   [Datatypes](datatypes).
1. In **Snapshots**, you can set up [API Snapshots](set-up-apis#snapshots).
1. When done, select **Save**.

## Authentication Methods for Generic HTTP API {#authentication}

Cribl Search supports the following authentication methods for Generic HTTP API Dataset Providers:

- **None**: No authentication. This is the default option.
- [Basic](#basic)
- [OAuth](#oauth)
- [Login](#login)

### Basic Authentication {#basic}

To configure HTTP Basic authentication for a Generic HTTP API Dataset Provider in Cribl Search:

1. Start [adding](#provider) a Generic HTTP API Dataset Provider, or editing an existing one.
1. In the **Authentication** section, set **Authentication method** to **Basic**.
1. Enter the HTTP Basic **Username** and **Password** to use when making API requests.
1. Select **Save**.

### Login Authentication {#login}

**Login** is the best authentication method to use for all services that return a JSON web token (JWT) or other
authentication token whose content type is `application/json` or `content/plain`.

To configure Login authentication for a Generic HTTP API Dataset Provider in Cribl Search:

1. Start [adding](#provider) a Generic HTTP API Dataset Provider, or editing an existing one.
1. In the **Authentication** section, set **Authentication method** to **Login**.
1. Configure the following:
   - **Login URL**: URL for the Login API call, which is normally a POST call.
   - **Login username**: Login username.
   - **Login password**: Login password.
   - **POST body**: Template for POST body to send with the login request. The `${username}` and `${password}` variables
     specify the corresponding credentials' locations in the message.
   - **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes. If the
     response from the auth request provides the JWT as `plain/text`, leave this field blank.
   - **Authorization header**: Authorization header key to pass to each endpoint's API request. Defaults to the literal
     name `Authorization`.
   - **Authorize expression**: JavaScript expression used to compute an Authorization header to pass to each endpoint's
     API request. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request.
   - **Authentication headers**: Optionally, select **Add Header** for each custom auth header you want to define.
     > Enter each header's **Name** and **Value**. In **Value**, you can define [parameters](#parameters), in the form
     > of `${parameterName}`.


### OAuth Authentication {#oauth}

To configure OAuth 2.0 authentication for a Generic HTTP API Dataset Provider in Cribl Search:

1. Start [adding](#provider) a Generic HTTP API Dataset Provider.
1. When you get to the **Authentication** section, set **Authentication method** to **OAuth**.
1. Configure the following:
   - **Login URL**: Endpoint for the OAuth API call, which is normally a POST call.
   - **Client secret parameter**: The name of the parameter to send with the **Client secret value**.
   - **Client secret value**: The OAuth client secret to authorize for an access token.
   - **Extra authentication parameters**: Optionally, select **Add parameter** for each additional OAuth request
     parameter you want to send in the body of POST requests. Automatically sets the `Content‑Type` header to
     `application/x‑www‑form‑urlencoded`.
     > Enter each parameter's **Name** and **Value**. In **Value**, you can define [parameters](#parameters), in the
     > form of `${parameterName}`.
   - **Token attribute**: Path to the token attribute in the login response body. Supports nested attributes.
   - **Authorization header**: Authorization header key to pass to each endpoint's API request. Defaults to the literal
     name `Authorization`.
   - **Authorize expression**: JavaScript expression used to compute an Authorization header to pass to each endpoint's
     API request. Uses `${token}` to reference the token obtained from a Login or OAuth authorization request.
   - **Authentication headers**: Optionally, select **Add Header** for each custom auth header you want to define.
     > Enter each header's **Name** and **Value**. In **Value**, you can define [parameters](#parameters), in the form
     > of `${parameterName}`.

## Search a Generic HTTP API Dataset {#search}

Now that you have a [Dataset Provider](#provider) and [Dataset](#dataset), you're ready to start
[searching](build-a-search).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

## Dynamically Query HTTP Endpoints {#parameters}

You can dynamically query different HTTP API endpoints, without having to change the [Dataset Provider](#provider)
configuration for each query.

To be able to do that, configure your [Generic HTTPI API Dataset Provider](#provider) with parameters in the form of
`${parameterName}`. Then you can provide values for those parameters as key-value pairs (`parameterName=parameterValue`)
in your query.

For example:

1. Start creating a Generic HTTP API [Dataset Provider](#provider).
2. In the **Available endpoints** section, let's define two sets of parameters, as shown below:

   - In **URL**, enter: `https://${domain}/${path}`.
   - In **Headers**, set **Value** to: `${headerVal1} ${headerVal2}`.

> (Note that you can use parameters to define the **Data field**, too.)


![Generic HTTP API: Configuring dynamic query parameters](search_dynamic_http.png)
{scale="90%"}

3. Use the new Dataset Provider to add a Generic HTTP API [Dataset](#dataset). Let's call the Dataset `my_http_api`.
4. Now, when searching the `my_http_api` Dataset, you'll be able to write queries like this one:
   ```kusto
   dataset="my_http_api" domain='cat-fact.herokuapp.com' path='facts' headerVal1='foo' headerVal2='bar'
   ```
