# externaldata



The `externaldata` operator fetches external data from HTTP(S) URLs, including public APIs.

> If you frequently use `externaldata` to search the same API endpoints, consider setting up a [Generic HTTP API](set-up-generic-http-api) Dataset. See also the [Usage Note](#usage) below.
{.box .info}

## Syntax

    `externaldata [ ConnectionURL [, ...] ] [ with ( PropertyName = PropertyValue [, ...]) ]`

> The outer brackets surrounding **ConnectionURL** are required and literal. They do not indicate an optional argument (as the inner brackets do).
>
{.box .warning}

### Arguments

| Name | Type | Required | Description |
|--|--|--|--|
| **ConnectionURL** | string | Yes | The HTTP(S) URL from which to fetch data. |
| **PropertyName**, **PropertyValue** | string | No | A list of optional [properties](#properties) that determine how to interpret the retrieved data.

#### Properties

| Property | Type | Description |
|---|---|---|
| dataField | string | The name of the field (in the response JSON) to pull data from. Leave blank if the result is an array. |
| datatype | string | The data type to use to parse the data. If none is specified, tries to parse the data as a JSON array. |
| headers | string | A JSON object containing the headers to send with the request. |
| method | `GET` or `POST` | The HTTP method used when making the API request. |

## Usage Note {#usage}

Unlike most other operators, `externaldata` includes – in the query – all that is needed to establish connectivity to remote data. This might include sensitive key information in cleartext. 

The query is logged (like other queries), and the log events might include this sensitive information. The Dataset where it’s logged is readable only by users with elevated privileges. However, to avoid any exposure of credentials in cleartext, consider configuring a [generic HTTP API](/set-up-generic-http-api) Dataset as an alternative to this operator.

## Examples

### Simple

```kusto
externaldata
[
  "https://cat-fact.herokuapp.com/facts"
]
```

### With a dataField

```kusto
externaldata
[
  "https://vpic.nhtsa.dot.gov/api/vehicles/getallmanufacturers?format=json"
]
with(
  dataField="Results"
)
```

### With a Datatype (ndjson file)

```kusto
externaldata
[
  "https://gist.githubusercontent.com/rfmcnally/0a5a16e09374da7dd478ffbe6ba52503/raw/095e75121f31a8b7dc88aa89dbd637a944ce264a/ndjson-sample.json"
]
with(
  datatype="Cribl Search"
)
```

### Specifying HTTP headers

```kusto
externaldata
[
  "https://vpic.nhtsa.dot.gov/api/vehicles/getallmanufacturers?format=json"
]
with(
  dataField="Results",
  headers='{"user-agent": "Foo", "otherHeader": "Bar"}'
)
```

Get weather data for New York City.

```kusto {runSearch=true}
externaldata ["https://wttr.in/NewYork?format=j1"] 
| mv-expand weather.0.hourly
```

