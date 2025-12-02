# ServiceNow API Collection


This topic covers how to configure Cribl Stream REST Collectors to gather data via [ServiceNow (SNOW) REST APIs](https://developer.servicenow.com/dev.do#!/reference/api/rome/rest/) and then enrich the data using Pipelines and the [Redis](https://docs.cribl.io/docs/redis-function) Function.

From among the 100-or-so SNOW REST APIs, we've chosen the [CMDB Instance](https://developer.servicenow.com/dev.do#!/reference/api/rome/rest/cmdb-instance-api) for this tutorial. You can adapt this material for use with other SNOW REST APIs. You'll need to create a separate Collector for each SNOW API you connect to.

The prerequisites for this setup are a ServiceNow instance, which you can obtain [here](https://developer.servicenow.com/), and the URL for the Redis store you wish to use.

## Using the Discover and Collect Pattern

A common pattern in REST APIs is to expose two related endpoints:

- One endpoint takes a category identifier as a URL parameter, and returns a list of instances of the category, with an individual identifier for each item in the list.

- A second endpoint takes the instance identifier as a URL parameter, and returns full details about the instance.

The SNOW CMDB Instance REST API follows this pattern. The API is called "CMDB" because it exposes a Configuration Management Database which contains records that describe how pieces of hardware are configured. The categories for different kinds of hardware are specified by "classnames," and the description of an individual piece of hardware is called a [Configuration Item (CI) record](https://docs.servicenow.com/bundle/orlando-servicenow-platform/page/product/configuration-management/reference/r_CMDBRecordTypes.html). The relevant API calls look like this:

- The `GET /now/cmdb/instance/{classname}` call takes a `classname` (for example, `cmdb_ci_appl`, `cmdb_ci_linux_server`, `cmdb_ci_apache_web_server`) as a URL parameter, and returns a list of instances of the classname. Each instance has a `sys_id` and a `name`.

- The `GET /now/cmdb/instance/{classname}/{sys_id}` call takes a `sys_id` as a URL parameter, and returns the detailed CI record for the instance that the `sys_id` refers to.

In this tutorial, you'll create a Collector that uses the first API call to **discover** the systems for a given classname. Then you'll iterate the second API call over the list of `sys_id`s we discovered, to **collect** the CI records of interest. This **discover and collect** pattern works for any Cribl Stream Collector.

You'll have the Collector store the CI data in Redis. Then whatever Source you choose that has events to enrich, can do that by pulling the collected CI record data from Redis. That's the goal: enriching data. 

How might this be useful? Here's one scenario: Suppose you have log data in which server names appear. Assuming that the servers in question have records in your CMDB, you can enrich the events with data about the server, such as its `sys_id` or the name of the user who updated it last. 

## Preparing Pipelines

Before creating the SNOW Collector itself, you'll set up the two Pipelines that the Collector needs. Both pipelines use the Cribl Stream [Redis](redis-function) Function.

* The **process** Pipeline attaches to the Collector and performs a `set` to add events to Redis.

* The **enrichment** Pipeline performs a Redis `get` to retrieve the values with which we'll enrich events in the Collector.

### Create the Process Pipeline

Create a Pipeline called `SNOW_Instance_Process` with the following Functions and values:

#### Parser Function

* **Operation Mode**: `Extract`
* **Type**: `JSON Object`
* **Source Field**: `_raw`

Note that each event this Function will parse is the reponse body from a `GET /now/cmdb/instance/{classname}/{sys_id}` call—i.e., a `result` object. That object has an attribute called `attributes` whose value is itself an object containing dozens of key-value pairs. To enrich our data, we'll treat one of those (`name`, meaning server name) as a key whose value will be two more (`sys_id` and `sys_updated_by`), comma-separated. We'll specify all three as **Evaluate Fields** in the next Function.

#### Eval Function

**Evaluate Fields**:

| **Name** | **Value Expression** |
|-------------|------------------------|
| `name` | `result.attributes.name`  | 
| `sys_id` | `result.attributes.sys_id` | 
| `sys_updated_by` | `result.attributes.sys_updated_by`  | 

#### Redis Function

The Redis function stores our fields in the key/value relationship described earlier. 

* **Command**: `set`.
* **Key**: `` `${name}` ``. 
* **Args**: `` `${sys_updated_by},${sys_id}` ``.
* **Redis URL**: The URL for your Redis store, for example, `redis://10.0.0.1:6379`.

### Create the Enrichment Pipeline

Create a Pipeline called `SNOW_Instance_Enrich` with the following Functions and values:

#### Redis Function

* **Command**: `get`.
* **Result field**: `Args`.
* **Key**: `` `${name}` ``.
* **Args**: Leave blank.
* **Redis URL**: The URL for your Redis store, for example, `redis://10.0.0.1:6379`.

#### Parser Function

* **Operation Mode**: `Extract`.
* **Type**: `CSV`.
* **Source Field**: `Args`.
* **List of Fields**: `sys_updated_by` `sys_id`.

#### Eval Function

* **Remove Fields**: `Args`.

## Configuring a SNOW Collector 

1. Navigate to **Products** > **Stream** > **Worker Groups**.
1. Select a Worker Group, then go to **Data** > **Sources**. Choose the Collector and select **Add Collector**.

> The sections described below are spread across several tabs. Select the tab links to navigate among tabs. Select **Save** when you've configured your Collector.
>
{.box .info}

### Collector Settings

These settings determine how data is discovered and collected before processing. You'll see the power of the **discover and collect** pattern here: from the results of a single Discover API call, we will generate an entire set of Collect API calls. There is more about this pattern in the [REST Collector documentation](usecase-rest#5-http-discover-and-collect-with-login-authentication).

**Collector ID**: Unique ID for this Collector. For example, `snow_42-a`.

#### **Discover** Settings

**Discover type**: `HTTP Request`

**Discover URL**: An expression that produces a URL for the discover operation. We can enter this URL as a constant, for example:

`'https://dev111111.service-now.com/api/now/cmdb/instance/cmdb_ci_appl'`

**Discover method**: `GET`

**Discover data field**: `result`

We specify `result` for the **Discover data field** because that is the key of the JSON array that our Discover call retrieves. For example:

```json
"result": [
  {
    "sys_id": "3a290cc60a0a0bb400000bdb386af1cf",
    "name": "PS LinuxApp01"
  },
  {
    "sys_id": "3a5dd3dbc0a8ce0100655f1ec66ed42c",
    "name": "PS LinuxApp02"
  }
]
```

This array becomes the list of items for which Collect tasks will be created. We'll reference one of its attributes, namely `sys_id`, as a variable in the Collect task's URL.

#### **Collect** Settings

**Collect URL**: An expression that produces a URL for the collect operation. We enter the first part of the URL as a constant, then for the final URL parameter, we use an expression to reference the `sys_id` returned by the Discover call:

```js
'https://dev111111.service-now.com/api/now/cmdb/instance/cmdb_ci_appl'+ C.Encode.uri(`${sys_id}`)
```

We use the [C.Encode.uri Cribl expression](cribl-reference#cencode--data-encoding-functions) to encode the `sys_id` in case any `sys_id` contains unsafe characters.

**Collect method**: `GET`.

#### Authentication

Use the **Authentication method** drop-down to select one of these options:

- **None**: Don't use authentication.

- **Basic**: Use HTTP token authentication.

- **Basic (credentials secret)**: Select a stored text secret in the resulting drop-down, or select **Create** to configure a new secret.

- **Login**: This option requires you to provide:

    - The **Login username** and **Login password** fields for your HTTP Basic authentication credentials.
    - The **Login URL** that SNOW should use to connect to Cribl Stream.
    - A **POST body** template for the request SNOW uses when logging in. You must edit this if your credentials' location differs from that specified by default.
    - The **Token attribute**, which is the path to the token attribute in login response body. Nested attributes are OK.
    - The **Authorize expression**, a JavaScript expression that computes the `Authorization` header to pass in Discover and Collect calls. The value `${token}` references the token obtained from login.

- **Login (credentials secret)**: Provide username and password credentials referenced by a secret. Select a stored text secret in the resulting **Credentials secret** drop-down, or select **Create** to configure a new secret. You must also provide the **Login URL**, **POST body**, **Token attribute**, and **Authorize expression**, as in the plain **Login** option.

### Result Settings

These settings enable the Collector you've been configuring to use your **process** Pipeline. 

#### Result Routing

Toggle **Send to Routes** off.

Choose the `SNOW_Instance_Process` Pipeline from the dropdown. 

Choose whatever **Destination** suits your purposes. A DevNull Destination would make sense given that the purpose of this Collector is simply to get data into Redis.

## Adding a Source with Data to Enrich

At this point, you have a Collector which stores CI record data in Redis. Assuming that you have a Source with events you want to enrich, you should now configure that Source to use the **enrich** Pipeline you created earlier.

In **Processing Settings** > **Pre-Processing**, choose `SNOW_Instance_Enrich` from the **Pipeline** drop-down.

## Verifying the Enrichment of Events

This procedure should demonstrate that your setup is working, or help you troubleshoot.

1. Send a request to the SNOW Instance API to verify that it returns data.

    * Try the [query builder](https://docs.servicenow.com/bundle/rome-servicenow-platform/page/product/configuration-management/task/use-cmdb-query-builder.html). 

2. Run the Collector once. Since you have attached the **process** Pipeline to the Collector, it should update Redis with `set` calls.

    * When you're ready to move towards production, you can run the Collector on a schedule, to update Redis periodically.

3. Run a `get` [command](https://redis.io/commands/get) on the Redis command line, using the key for your chosen API, to verify that Redis was updated as expected.

4. Check the Source whose data you are enriching, to verify that the fields you added are showing up.
