# Miscellaneous Expression Methods


## C.Schema – Schema Methods {#schema}

### `C.Schema`

```js
C.Schema: (id: string) => SchemaValidator
```

### `C.Schema.validate()`

```js
C.SchemaValidator.validate(data: any): boolean
```

Validates the given object against the schema.

Returns `true` when schema is valid; otherwise, `false`.

|Parameter|Type|Description|
|---|---|---|
|`data` | any | Object to be validated.

#### Examples

To validate whether a `myField` conforms to `schema1`, you can use:

```js
C.Schema('schema1').validate(myField)
```

See [Schema Library](schema-library) for more details.

## C.Secret – Secrets‑Management Methods {#secret}

### `C.Secret()`

```js
C.Secret: (id: string, type?: string): ISecret
C.Secret(id: string, type: 'keypair') => IPairSecret
C.Secret(id: string, type: 'text') => ITextSecret
C.Secret(id: string, type: 'credentials') => ICredentialsSecret
```

Returns a secret matching the specified ID.

|Parameter|Type|Description|
|---|---|---|
|`id` | string | ID of the secret.
|`type` (optional) | 'text' \| 'keypair' \| 'credentials' | Type of the secret.

#### Examples

To return a text secret with ID `victorias` (or with undefined, if no such secret exists), use:

```js
C.Secret('victorias', 'text')
```

You can also get attributes of secrets with the following expressions:

```js
C.Secret('api_key', 'keypair').secretKey
C.Secret('secret_hash', 'text').value
C.Secret('user_pass', 'credentials').password
```

Common returned attributes for `ISecret` objects:

- `secretType` – one of `keypair`, `text`, or `credentials`.
- `description` (optional) – the secret description.
- `tags` (optional) – a comma separated list of tags.

Additional returned attributes for `IPairSecret` objects:

- `apiKey` – the API key value
- `secretKey` – the Secret key value

Additional returned attributes for `ITextSecret` objects:

- `value` – the text value

Additional returned attributes for `ICredentialsSecret` objects:

- `username` – the username value
- `password` – the password value

See [Securing Cribl Stream > Secrets](/stream/securing-secrets) for more details.

## C.env – Environment

### `C.env` {#env}

```js
C.env: {[key: string]: string;}
```

Returns an object containing Cribl Edge's [environment variables](environment-variables).
All internal variables, that is, those starting with `CRIBL_`, are accessible in this method.

#### Examples

To return the parent of Cribl's `bin` directory (generally `/opt/cribl`), use:

```js
C.env.CRIBL_HOME
```

To return the hostname of the machine where Cribl Edge is running, use:

```js
C.env.HOSTNAME
```

## C.os – System Methods {#os}

### `C.os.hostname()`

Returns hostname of the system running this Cribl Edge instance.

## C.vars – Global Variables {#vars}

See [Global Variables Library](global-variables-library) for more details.

## C.version – Cribl Edge Versions {#version}

### `C.version`

Returns the Cribl Edge version currently running.

### `C.confVersion`

Returns the commit hash of the Edge Node's current config version. (Evaluates only against live data sent through Edge Nodes. Values will be undefined in the Leader's Preview pane.)

## C.Misc – Miscellaneous Utility Methods {#misc}

### `C.Misc.zip()`

```js
C.Misc.zip(keys: string[], values: any[], dest?: any): any
```

Sets the given keys to the corresponding values on the given `dest` object. If `dest` is not provided, a new object will be constructed.

Returns object on which the fields were set.

|Parameter|Type|Description|
|---|---|---|
|`keys` | string[] | Field names corresponding to keys.
|`values` | any[] | Values corresponding to values.
|`dest` | any | Object on which to set field values.

#### Examples

Let's take the following expression:

```js
people = C.Misc.zip(titles, names)
```

If sample data contains: `titles=['ceo', 'svp', 'vp']`, `names=['foo', 'bar', 'baz']`,
this expression create an object called `people`, with key names from elements in `titles`,
and with corresponding values from elements in `names`.

Result: `"people": {"ceo": "foo", "svp": "bar", "vp": "baz"}`

### `C.Misc.uuidv4()`

```ts
C.Misc.uuidv4(): string
```

Returns a version 4 (random) UUID in accordance with [RFC-4122](https://www.ietf.org/rfc/rfc4122.txt).

#### Examples

To create a UUIDv4, use:

```ts
C.Misc.uuidv4()
```

Result: a conforming UUIDv4, such as `58d8be36-4db0-4b1c-ac80-28bb03c45e0d`. It is highly improbable that two
version 4 UUIDs will ever have the same value.

### `C.Misc.uuidv5()`

```ts
C.Misc.uuidv5(name: string, namespace: string): string
```

Returns a version 5 (namespaced) UUID in accordance with [RFC-4122](https://www.ietf.org/rfc/rfc4122.txt) for the
given `name` and `namespace`. If `namespace` is not a valid UUID, this function will fail.

|Parameter|Type|Description
|---|---|---|
|`name`| string | Any arbitrary name to use in UUID generation.
|`namespace`| string | One of `DNS`, `URL`, `OID`, or `X500` to use a predefined namespace, or else a valid UUID.

#### Examples

To create a UUIDv5 with the predefined DNS namespace, use:

```ts
C.Misc.uuidv5('example', 'DNS')
```

Result: a UUID that is highly likely to be the same for the same name and namespace. In this case, the
result would be `7cb48787-6d91-5b9f-bc60-f30298ea5736`.

### `C.Misc.validateUUID()`

```ts
C.Misc.validateUUID(maybeUUID: string): boolean
```

Returns `true` if `maybeUUID` is a valid UUID of any version, and false otherwise.


|Parameter|Type|Description
|---|---|---|
|`maybeUUID`| string | A string to test for a valid UUID.

#### Examples

```ts
C.Misc.validateUUID(C.Misc.uuidv4())
```

Result: returns `true`

```ts
C.Misc.validateUUID('clearly not a UUID')
```

Result: returns `false`

### `C.Misc.getUUIDVersion()`

```ts
C.Misc.getUUIDVersion(uuid: string): number
```

Returns a `number` of the UUID version given by `uuid` if it is a valid UUID, otherwise `undefined`.

|Parameter|Type|Description
|---|---|---|
|`uuid`| string | A UUID for which to determine the version.

#### Examples

```ts
C.Misc.getUUIDVersion(C.Misc.uuidv4())
```

Result: `4`

```ts
C.Misc.getUUIDVersion(C.Misc.uuidv5('example', 'X500'))
```

Result: `5`

## `C.WorkerGroupId`

```js
C.WorkerGroupId: string
```

Returns the name of the current Fleet. Use this expression to enrich logs or metrics with the Fleet identity, enabling filtering and grouping in dashboards and analytics tools. This method always returns a string, even in single-node deployments.

> Prefer `C.WorkerGroupId` over `C.env.CRIBL_GROUP_ID` where possible. The environment variable may be unset or stale, whereas `C.WorkerGroupId` always reflects the current Fleet assignment from your configuration.
>
{.box .info}

#### Example

To add the current Fleet name to an event field:

```js
__e.worker_group = C.WorkerGroupId
```
