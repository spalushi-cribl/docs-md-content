# Using REST/API Collectors


The [REST/API Endpoint Collector](collectors-rest) is powerful, but complex. This use case demonstrates several examples of building and running REST Collectors to pull data from public and simulated REST endpoints.

> Check out the example REST Collector configurations in Cribl's Collector Templates [repository](https://github.com/criblio/collector-templates/tree/main). For many popular Collectors, the repo provides configurations (with companion Event Breakers, and event samples in some cases) that you can import into your Cribl Stream instance, saving the time you'd have spent building them yourself.
{.box .success}

To configure a Collector for Netskope Events and Alerts, see [this Netskope Community topic](https://community.netskope.com/t5/Additional-Discussions/Cribl-Netskope-Events-and-Alerts-Integration/td-p/6438).

## 1. Basic HTTP GET

This example performs an HTTP GET operation against an external Joke API. This API uses a license key header to authenticate the user.

**Discover type**: None

**Collect URL**: `'https://matchilling-chuck-norris-jokes-v1.p.rapidapi.com/jokes/random'`

**Collect parameters**: None

**Collect headers**:

  `accept: 'application/json'`

  `x-rapidapi-key: 'e4068647ffmsh65536596798f49dp17e998jsn342bac862377'`

  `x-rapidapi-host: 'matchilling-chuck-norris-jokes-v1.p.rapidapi.com'`

  `useQueryString: true`

**Pagination**: None

**Authentication**: None

**Event Breaker**: JSON Newline Delimited – use Cribl Stream's built-in **Cribl > ndjson** rule, and associate it with the Collector to parse the JSON document.

![Collector configuration for basic HTTP GET](rest-ex-1b.00a5ffc358.png)
{border="true"}

#### Results

When run (in Preview mode), the Collector should return a single JSON record. If the Collector is set up with an NDJSON Event Breaker, it will look like this:

![Returned event](rest-ex-1c.5ecd5b86ee.png)
{border="true"}

##  2. HTTP GET with Pagination via URL Attribute  {#ex-2}

The REST Collector's Pagination feature (available in Cribl Stream 2.4.3 and above) allows collection to retrieve 1–*N* pages of data, using attributes returned in either the response body or response header. The returned attribute can either be a URL (referencing the next page), or a token that can be added to subsequent request headers or parameters.

In this example, a returned response-body attribute contains a URL that references the next page. Pagination will continue until either the Collector's **Page limit** setting is reached, or no more pages are present (that is, the returned attribute is not present in the response body).

This example's API retrieves near-Earth asteroid data from NASA. The example uses a JSON Array Event Breaker to extract individual records from an array attribute in the response.

**Discover type**: None

**Collect URL**: `'http://www.neowsapp.com/rest/v1/neo/browse?api_key=oDa6w0fjsKEb1N3bMA5dMLhatMJ4WC5XtOBTrLrk'`

**Collect parameters**: None – Parameters in this example are added to the header. Static parameters (that is, parameters that don’t reference variables) can safely be added to the URL. Any parameters that do reference variables should always be added in the **Collect parameters** section, to allow filtering of values that evaluate as undefined.

**Collect headers**: None

**Pagination**: Response Body Attribute

**Response attributes**: `next`

**Authentication**: None

**Event Breaker**: JSON Array

![Event Breaker configuration](rest-ex-2a.png)
{scale="30%" border="true"}

![Collector configuration for HTTP GET, paginated via URL Attribute](rest-ex-2b.d3f44d7d29.png)
{border="true"}

When run (in Preview mode), the Collector should return multiple records extracted from the Event Breaker. In this example, we limited output to 10 pages of data. This particular dataset has over 1,000 total pages, so it’s a good idea to limit output to avoid a job that runs too long.

![Paginated events](rest-ex-2c.fe34a77441.png)
{border="true"}

> This API allows a certain number of calls/month. Cribl recommends that you not schedule this Collector – run it ad-hoc, for testing only.
>
{.box .warning}

## 3. HTTP GET with Pagination via Response Body Attribute

This example uses Response Body Attribute pagination, which returns a token that is passed as a request parameter to retrieve subsequent pages of data. The only difference between this example and [Example 2](#ex-2) is how the Response Body Attribute is used.

> To authenticate against the GreyNoise endpoint used in this example, set up a trial account according to GreyNoise's [Setting Up a Trial Account](https://developer.greynoise.io/docs/setting-up-an-account) documentation.
>
{.box .success}

**Discover type**: None

**Collect URL**: `'https://api.greynoise.io/v2/experimental/gnql'`

**Collect method**: `GET`

**Collect parameters**:

`query: 'last_seen:1d'`

`` scroll: `${scroll}` ``

**Collect headers**:

`accept: 'application/json'`

`key: '<your-GreyNoise-API-key-here>'`

**Pagination**: Response Body Attribute

**Response attributes**: `scroll`

**Page limit**: `10` (or `0` to pull all data)

**Authentication**: None

**Event Breaker**: JSON Array – use the configuration shown here:

![Event Breaker configuration](rest-ex-3-0-breaker-config.png)
{scale="30%" border="true"}

In this example, the response body returns an attribute named `scroll`, which is a token that references the next page of data to fetch. We reference the attribute in **Collect parameters** using the JavaScript expression: `` `${scroll}` ``. If present, this will be passed to retrieve subsequent pages of data, until either the Collector's **Page limit** setting is reached, or no more pages are present.

![Collector configuration for HTTP GET, paginated via Response Body Attribute](rest-ex-3a.872f1659a5.png)
{border="true"}

#### Collector Output

![Paginated events](rest-ex-3b.74f311a4d8.png)
{border="true"}

> This API allows a certain number of calls/month. Cribl recommends that you not schedule this Collector – run it ad-hoc, for testing only.
>
{.box .warning}

For a more detailed use case around this particular API, see Cribl's [Enrichment at Scale!](https://cribl.io/blog/enrichment-at-scale/) blog post.

## 4. Pagination via Nested Response Body Attributes

When using Response Body Attribute pagination with nested attributes for determining the next page, the extracted attributes from the response will have dots (`.`) included in their names within `__collectible`. This requires different syntax for accessing these attributes within the **Collect URL**, **Collect parameters**, **Collect POST body**, **Collect headers**, and **Last-page expression** fields.

For example, the following shows attributes two levels deep:

![Attributes two levels deep](nested-response-body-ex1.png)
{border="true"}

The attributes are added to `__collectible` with dots (`.`) in the name:

![Attributes added to `__collectible`](nested-response-body-ex2.png)
{border="true"}

[Special syntax](https://docs.cribl.io/stream/introduction-reference/#special-chars) is needed to reference the attributes because they have dots in the name, like this `__e['attribute.name.with.dots']`.

The same syntax referenced in example 3 above, `__e['response.endOffset']`, can be used in the **Collect URL**, **Collect parameters**, **Collect POST body**, **Collect headers**, and **Last-page expression** fields. Here are examples of each.

**Collect URL**: `` `http://0.0.0.0:1111/pageit` + (__e['meta.pagination.after'] ? `?$after=${__e['meta.pagination.after']}` : '')``

![Collect URL](nested-response-body-ex3.png)
{border="true"}

**Collect parameters**:

* **Name**: `after`

* **Value**: `` `${__e['meta.pagination.after'] ? __e['meta.pagination.after'] : ''}` ``

![Collect parameters](nested-response-body-ex4.png)
{border="true"}

**Collect headers**: 

* **Name**: `after`

* **Value**: `` `${__e['meta.pagination.after'] ? __e['meta.pagination.after'] : ''}` ``

![Collect headers](nested-response-body-ex-headers.png)
{border="true"}

**Collect POST body** and **Last-page expression**:

* **Collect POST body**: `` `{ "query": { "startOffset": ${+__e['response.endOffset'] || 0}, "endOffset": ${(+__e['response.endOffset'] || 0) + 100} } }` `` 

* **Last page expression**: `(+__e["response.returnedRecords"]) === 0`

Here the API passes the `startOffset` and `endOffset` parameters in the POST body to page through data until `response.returnedRecords == 0`. The **Last page expression** field is used to determine when all the available data has been consumed.

![Collect POST body and Last-page expression](nested-response-body-ex5.png)
{border="true"}

Here’s sample output from the API showing partial results returned for the first page of data:

``` json
{
  "response": {
    "totalRecords": "1275",
    "returnedRecords": "100",
    "startOffset": "0",
    "endOffset": "100",
    "data": [
      {
        "one": 1
      },
      {
        "two": 2
      },
      ...
    ]
  }
}
```

Notice that the Collector Response attributes use dotted notation to access `endOffset` and `returnedRecords` as `response.endOffset` and `response.returnedRecords` respectively. This tells the Collector that the attributes of interest are named `response.endOffset` (dot in name) or nested JSON attributes: `{ response : { endOffset: 100 ... } }`. However, the response is structured (dots in attribute name or nested) the Collector will add the attributes to the second page’s `__collectible` attribute using dots in the attribute name, for example:

![`__collectible` second page of attributes](nested-response-body-ex7.png)
{border="false"}

Now that you have the attributes available in `__collectible`, you can use them in the **Collect URL**, **Collect parameters**, **Collect POST body**, **Collect headers**, and **Last-page expression** fields. The syntax is the same across all these fields. You'll use a special variable, `__e`, which acts like a shorthand for `__collectible` (since you can't use it directly here). Here's how to access specific attributes:

* response.endOffset: Use `__e['response.endOffset']`
* response.returnedRecords: Use `__e['response.returnedRecords']`

## 5. HTTP GET with Pagination via Response Header URL

This example leverages pagination using a Response Header Attribute value. The value returned can be either a URL (of the next page) or a token value (a request attribute that is passed to retrieve the next page of data). 

This example is based around a local Web server on port 3001. The server returns a response header when another page of data is available, and the header contains the URL of the next page. Here's how the header looks in developer tools:

![Next-page URL passed as Response Header Attribute](rest-ex-4c.78efbe4b02.png)
{border="true"}

#### Collector Configuration

**Discover type**: None

**Collect URL**: 'http://localhost:3001/api/v1/pagination/nextLinkHeader?num=1&maxPages=16'

**Collect parameters**: None

**Collect headers**: None

**Pagination**: Response Header Attribute

**Response attributes**: `nextLink`

**Authentication**: None

**Event Breaker**: None

> You can modify the `maxPages` URL parameter to control how many pages this call returns.
>
{.box .success}

![Collector configuration for HTTP GET, paginated via Response Header URL](rest-ex-4a.png)
{border="true"}

#### Collector Output

![Paginated events](rest-ex-4b.eb25c3789f.png)
{border="true"}

## 6. HTTP Discover and Collect with Login Authentication

In some cases, you must run an HTTP Request discovery to identify the items to collect. This example will do the following:

1. Perform a Login (POST with body containing the login credentials), to obtain an auth token that will passed in the Authorization header in all subsequent REST calls.
2. Run a REST call to discover items to be collected – in this case, log files.
3. For each log file discovered, collect the contents of that file.
4. We'll also demonstrate URL-encoding of a path element. You'd need to manually encode part of the URL in cases where unsafe ASCII characters might be present in the path element (for example, space, `$`, `/`, or `=`).

**Discover type**: `HTTP Request`

**Discover URL**: 'http://localhost:9000/api/v1/system/logs'

**Discover method**: GET

**Discover parameters**: None

**Discover headers**: None

**Discover data field**: `items`

**Collect URL**: ``'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`) ``

**Collect method**: GET

**Collect parameters**: None

**Collect headers**: None

**Pagination**: None

**Authentication**: Login

**Login URL**: `'http://localhost:9000/api/v1/auth/login'`

**Login username**: `admin` (or other user)

**Login password**: `admin` (or other user's corresponding password)

**[Authentication] POST Body**: `` `{ "username": "${username}", "password": "${password}" }` ``

**Token attribute**: `token`

**Authorize expression**: `` `Bearer ${token}` ``

**Event Breaker**: JSON Array 

  - **Array Field**: `items.events`

![Event Breaker configuration](rest-ex-5a.png)
{scale="30%" border="true"}

![Collector configuration for HTTP Discover and Collect with Login authentication](rest-ex-5b.600a1e5ee1.png)
{border="true"}

### Login

The login call sends a POST to the login URL, passing the string derived from the POST Body JavaScript expression. Note that the variables `${username}` and `${password}` are available to this call, and are taken from the `username` and `password` text fields. 

Upon successful login (`200` response code), the login token will be extracted from the response body’s token attributes, as specified by the **Token attribute** field. 

Finally, the value derived from the **Authorize expression** field will be added to the Authorization header for all subsequent calls (here, both Discover and Collect). Set this to `${token}` to reference the token obtained from the login POST request.

### Discover

The Discover call here is used to discover the list of log files that can be collected. The data returned by this call has this format:

```json
{
  "count": 0,
  "items": [
	{
  	"id": "logFileName",
  	"path": "pathToFile"
	}
  ]
}
```

The **Discover Data Field** is used to define the array in Discover results that contains the list of items to discover. Here, each item is an object, with an attribute ID that is referenced in the Collect calls. So the Discover call generates a list of items for which Collect tasks will be created. 

### Collect

From the Discover task's returned list of items, each item will cause one Collect task to be created and run. An object containing the Discover item (along with some internal variables) will be passed to the Collect task. 

You can reference this object's attributes as variables in the Collect task’s URL, request parameters, and request headers. When running a preview, you can see the object's contents in the `__collectible` internal variable. (Enable **Show Internal Fields**, and expand `__collectible` to view the variables available). 

For example, here’s one of the events returned by this example's Collect operation. The `__collectible` attribute contains details identifying the page number and the URL used to obtain the data:

![__collectible internal variable, expanded to show its contents](rest-ex-5c.9e54bfc525.png)
{border="true"}

As you can see,  `__collectible` contains a `__pageNum` variable, which shows which page of data the event was received in. Also, `__collectible` contains an `id` variable, available for use in the Collect operation. Here's how this variable is referenced in the Collect operation’s URL:

`` 'http://localhost:9000/api/v1/system/logs/' + C.Encode.uri(`${id}`) ``

Because the variable is used in the path, and it might contain unsafe ASCII characters (specifically, space), we need to URL-encode the variable. This is the only case where the REST Collector requires URI encoding – variables that are defined directly as part of the URL. (Request parameters, not contained directly in the URL, are automatically encoded.)

The data returned by the Collect call has the following format:

```json
{
  "items": [
	{
  	"file": "access.log",
  	"nextOffset": "",
  	"previousOffset": "0:2236637",
  	"events": [
    	{
      	"time": "2021-02-15T23:39:23.043Z",
      	"src": "127.0.0.1",
      	"user": "admin",
      	"method": "GET",
      	"url": "/api/v1/jobs/1613432361.24",
      	"status": 200,
      	"message": "GET /api/v1/jobs/1613432361.24",
      	"response_time": 2
    	},
    	{
      	"time": "2021-02-15T23:39:22.366Z",
      	"src": "127.0.0.1",
      	"user": "admin",
      	"method": "GET",
      	"url": "/api/v1/system/logs/worker%2F7%2Fcribl.log",
      	"status": 200,
      	"message": "GET /api/v1/system/logs/worker%2F7%2Fcribl.log",
      	"response
...
```

The real data that we want to access is located at `items.events`. We can use a JSON Array event breaker to convert data from `events.items` into individual events that will be sent to Routes and processed by Cribl Stream. The output looks like this in Preview:

![Collected data](rest-ex-5d.e760e145fa.png)
{border="true"}

> If this example fails with errors of the form `statusCode: 429...Too many requests` – see [Common Errors and Warnings](common-errors#rest-api-collector) to resolve this by relaxing the login rate limit.
>
{.box .info}

## 7. Item List Discovery

This example demonstrates situations where the Item List discovery mechanism is useful: enabling collection based on a predefined list of items. Here, we want to collect weather information for a static list of states – each returned from Discover results as a single collection task. 

Let's assume we are interested in weather for the following U.S. locations: Nashville, Dallas, and Denver. When the Discover operation runs, it will return a `__collectible` object for each location (each representing its own collection task): `{ id: ‘’}, {id: ‘TX’}, {id: ‘TN’}`.

**Discover type**: `Item List`

**Discover items**: `Nashville, Dallas, Denver`

**Collect URL**: `'https://community-open-weather-map.p.rapidapi.com/find'`

**Collect parameters**:

`type: 'link'`

`units: 'imperial'`

`` q: `${id}` ``

**Collect headers**:

`x-rapidapi-host: 'community-open-weather-map.p.rapidapi.com'`

`x-rapidapi-key: '78934c846cmsh70cb53f75a8a54bp119d21jsn29df549b4fd6'`

`useQueryString: true`

**Pagination**: None

**Authentication**: None

**Event Breaker**: JSON Newline Delimited – Use a rule like **Cribl > ndjson** to parse each event and extract fields.

**Fields**:

`job: weather-${__collectible.id}`

`city: ${__collectible.id}`

![Collector configuration for Discovery via Item List](rest-ex-6a.e4761d8a05.png)
{border="true"}

![Fields configuration](rest-ex-6b.0aab9f0351.png)
{border="true"}

#### Collector Output

One interesting thing about this example is the addition of **Fields** to each event, using content from the internal `__collectible` attribute. This `__collectible` attribute contains results from the Discover operation, and is available in each event collected. 

This demonstrates how information from the Discover operation can be transferred to events generated during the Collect operation. Note the attributes `__collectible`, `city`, and `job` in the Collector output below:

![Collected events](rest-ex-6c.8f171e8f37.png)
{border="true"}

> This API allows a certain number of calls/month. Cribl recommends that you not schedule this Collector – run it ad-hoc, for testing only.
>
{.box .warning}

## 8. JSON Response Discovery

Like Item List discovery, **Discover type: JSON Response** allows you to discover a predefined, static list of items. JSON Response's advantage is its ability to return an object containing more than one attribute that the Collect operation can use. 

Sticking with our weather example above, imagine that we needed to use both longitude and latitude (instead of just city or state) when performing collection. This is the perfect use case for JSON Response discovery.

**Discover type**: `JSON Response`

**Discover result**: `{"items": [{"city": "Nashville", "lat": 36.174465, "lon": 86.767960},{"city": "Dallas", "lat": 32.779167, "lon": -96.808891}, {"city": "Denver", "lat": 39.742043, "lon": -104.991531}] }`

**Discover data field**: `items`

**Collect URL**: `'http://api.openweathermap.org/data/2.5/weather'`

**Collect headers**: None

**Collect parameters**:

`` lat: `${lat}` ``

`` lon: `${lon}` ``

`appid: '438d61a1db9e713240b30140e9ddfea2'`

**Pagination**: None

**Authentication**: None

**Event Breaker**: JSON Newline Delimited – Use a rule like **Cribl > ndjson** to parse each event and extract fields.

**Fields**:

``job: `weather-${__collectible.city}` ``

`` city: `${__collectible.city}` ``

![Collector configuration for JSON Response Discovery](rest-ex-7a.1dc2b4891b.png)
{border="true"}

Notice how attributes present in the **Discover Result** JSON object’s `items` array (`` `${lat}` ``, `` `${lon}` ``, `` `city` ``) are used in **Collect Request Parameters**, and in metadata **Fields**. Any other attribute present in the `items` array can similarly be referenced in the URL, request parameters, or request headers.

#### Collector Output

![Item List preview](rest-ex-7b.73b9e829a2.png)
{border="true"}



> This API allows a certain number of calls/month. Cribl recommends that you not schedule this Collector – run it ad-hoc, for testing only.
>
{.box .warning}

## 9. HTTP Response Discover Result with Custom Code {#discovery-code}

Assume the Discover REST API returns a list of individual record IDs matching search criteria, such as:

    `{ "ids": [1,2,3,4,5,6,7,8,...,49,50] }`

The collect API can accept a list of 1 to 10 individual IDs to collect data: `http://abc.com/collect?Ids=1,2,3,4,5,6,7,8,9,10`

By default, using the `ids` attribute from the Discover call results in 50 individual collect tasks, each retrieving data for a single ID. However, the API supports 1 to 10 IDs at a time. Using the **Format discover results** option, you can manipulate results to use 10 collect tasks, each retrieving data for 10 IDs. Here's how:

``` javascript
const pageSize = 10;
let arr = [];
__e['resultOut'] = []; // Store results in the original Discover results object.
__e['ids'].forEach(id => {
  if (arr.length === pageSize) {
    __e['resultOut'].push({ "ids": arr.join(',') });
    arr = [];
  }
  arr.push(id);
});
if (arr.length) __e['resultOut'].push({ "ids": arr.join(',') }); // Add last batch
```

Given 50 unique IDs, the output stores results in `resultOut`:

``` json
{
  "ids": [1, 2, 3, ..., 49, 50],
  "resultOut": [
    { "ids": "1,2,3,4,5,6,7,8,9,10" },
    { "ids": "11,12,13,14,15,16,17,18,19,20" },
    { "ids": "21,22,23,24,25,26,27,28,29,30" },
    { "ids": "31,32,33,34,35,36,37,38,39,40" },
    { "ids": "41,42,43,44,45,46,47,48,49,50" }
  ]
}
```

Set the **Discover data field** on the REST Collector to `resultOut` to use the array of 5 items during the collect phase, resulting in 5 collect tasks. The collect URL or request parameters can include the `ids` attribute from each array element: `http://abc.com/collect?Ids=${ids}`

## 10. State Tracking by Latest Time {#state-tracking-time}

[REST Collector state tracking](collectors-rest#state-tracking) can help prevent both duplicate data and gaps in data for subsequent collection runs. To demonstrate how to configure a Collector to use state tracking, we'll access Stream's `/system/metrics` endpoint. This guide uses an out-of-the-box Stream configuration – you may need to update endpoints and authentication parameters to match your instance's configuration.

### Configure the Event Breaker

For this example we'll use an Event Breaker from Cribl's [Collector Templates](https://github.com/criblio/collector-templates) repository. For more information on Event Breakers, check out [the documentation](event-breakers).

1. Click **Processing**, then **Knowledge**, then **Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** to open a text editor view.
4. Paste in the breaker config found [here](https://github.com/criblio/collector-templates/blob/main/collectors/rest/cribl_metrics/breaker.json).
5. Click **OK**.
6. Click **Save**.

### Configure the Collector

Set the following fields in the `Collector Settings` section

* **Collector ID**: `state-tracking-test`
* **Collect URL**: `'http://localhost:9000/api/v1/system/metrics'`
* **Collect parameters**:
  * **Name**: `earliest` | **Value**: `` `${state.latestTime * 1000}` ``
* **Authentication**: `Login`
* **Login URL**: `'http://localhost:9000/api/v1/auth/login'` (note that this is updated from the default `https` value)
* **Login username**: `admin`
* **Login password**: `admin`
* **Token attribute**: `token`

Click **Result Settings**, then **Event Breakers**. Then click **Add ruleset** and choose the `criblApi` ruleset from the earlier step.

At this point, your configuration should look like this:

![Collector Config - Collector Settings](usecase-rest-state-tracking-conf1.png)
{border="true"}

![Collector Config - Event Breakers](usecase-rest-state-tracking-conf2.png)
{border="true"}

Finally, click **Save**.

### Run the Collector

1. From the **Actions** column, click **Run** for your newly configured Collector.
2. Click **Full Run** to select the right collection mode.  Preview and Discovery runs do not support state tracking.
3. Expand the **State Tracking** section, then set **Enabled** to `Yes`. We can use the default values for **State update expression** and **State merge expression**. For more information, see [Understanding State Expression Fields](collectors-rest#state-tracking-expression-fields).
4. Click **Run** to start the collection.

### Check the Results

Since this is the first run of the Collector, there was no state value to derive `earliest` from. As a result, many events were likely returned. You can check how many by clicking the link to **Latest Ad Hoc Run** from the Collector list view. From here, check the **Events collected** result.

![First Run Results](usecase-rest-state-tracking-results1.png)

Now, here's where we can see the value of state tracking. Complete the steps in the **Running the Collector** section again. If you check the `Latest Ad Hoc Run` results again, you'll see that fewer events were collected. This is because the `earliest` parameter resolved to the latest time from the first run, meaning that we only picked up new metrics that were generated since the last run!

![Second Run Results](usecase-rest-state-tracking-results2.png)
