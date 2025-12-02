# Miscellaneous Expression Methods


## C.Schema – Schema Methods {#schema}

### `C.Schema`

```js
Schema: (id: string) => SchemaValidator
```

### `C.Schema.validate()`

```js
SchemaValidator.validate(data: any): boolean
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
Secret: (id: string, type?: string): ISecret
Secret(id: string, type: 'keypair') => IPairSecret
Secret(id: string, type: 'text') => ITextSecret
Secret(id: string, type: 'credentials') => ICredentialsSecret
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

See [Securing Cribl Stream > Secrets](/stream/securing-and-monitoring#secrets) for more details.

## C.env – Environment

### `C.env` {#env}

```js
env: {[key: string]: string;}
```

Returns an object containing Cribl Edge's environment variables.

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
Misc.zip(keys: string[], values: any[], dest?: any): any
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
