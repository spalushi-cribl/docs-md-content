
<h1 id="cribl-core-api-system-operations">Cribl Core API - System Operations v4.15.0-f275b803</h1>

> Scroll down for example requests and responses.

This API Reference lists available REST endpoints, along with their supported operations for accessing, creating, updating, or deleting resources. See our complementary product documentation at [docs.cribl.io](http://docs.cribl.io).

Base URLs:

* <a href="/">/</a>

Web: <a href="https://portal.support.cribl.io">Support</a> 

# Authentication

- HTTP Authentication, scheme: bearer 

<h1 id="cribl-core-api-system-operations-system-operations">System Operations</h1>

## Reload Cribl settings from the filesystem

<a id="opIdreloadSettings"></a>

> Code samples

`POST /system/settings/reload`

<h3 id="reload-cribl-settings-from-the-filesystem-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal error|None|

<aside class="success">
This operation does not require authentication
</aside>

## Restart Cribl server

<a id="opIdtriggerRestart"></a>

> Code samples

`POST /system/settings/restart`

<h3 id="restart-cribl-server-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Success|None|
|500|[Internal Server Error](https://tools.ietf.org/html/rfc7231#section-6.6.1)|Internal error|None|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

