# Eval


The Eval Function adds or removes fields from events. (In Splunk, these are index-time fields.)

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Evaluate fields**: Set of key-value pairs to add. The left-hand side input (**Name**) is the key name. The right-hand side input (**Value Expression**) is a JS expression to compute the value – this can be a constant. Nested addressing is supported. Strings intended to be used as values must be single- or double-quoted. (For details, see [Cribl Expression Syntax](introduction-reference#special-chars).) 

**Keep fields**: List of fields to keep. Wildcards (\*) and nested addressing are supported. Takes precedence over **Remove fields** (below). To reference a parent object and all children requires using the (\*) wildcard. For example, if `_raw` is converted to an object then use `_raw*` to refer to itself and all children.

**Remove fields**: List of fields to remove. Wildcards (\*) and nested addressing are supported. Cannot remove fields matching **Keep fields**. Cribl Stream internal fields that start with `__`  (double underscore) **cannot** be removed via wildcard. Instead, they need to be specified individually. For example, `__myField` cannot be removed by specifying `__myF*`.



### Using Keep and Remove

A field matching an entry in *both* **Keep** (wildcard or not) and **Remove** will *not* be removed. This is useful for implementing “remove all but” functionality. For example, to keep only `_time, _raw, source, sourcetype, host`, we can specify them all in **Keep**, while specifying `*` in **Remove**.

Negated terms are supported in both **Keep fields** and **Remove fields**. The list is order-sensitive when negated terms are used. Examples:

  * `!foobar, foo*` means "All fields that start with 'foo' except `foobar`."
  * `!foo*, *` means "All fields except for those that start with 'foo'." 

## Examples

Note that Functions use the special variable `__e` to access the `(context)` event inside JavaScript expressions. 

**Scenario A**: Create field `myField` with static value of `value1`:
- **Name:**  `myField`   
- **Value Expression:** `'value1'`

**Scenario B**: Set field `action` to `blocked` if `login==error`:
- **Name:**  `action`   
- **Value Expression:** `login=='error' ? 'blocked' : action`

**Scenario C**: Create a multivalued field called `myTags`. (i.e., array):
- **Name:**  `myTags`   
- **Value Expression:** `['failed', 'blocked']`

**Scenario D**: Add value `error` to the multivalued field `myTags`:
- **Name:**  `myTags`   
- **Value Expression:** `login=='error' ? [...myTags, 'error'] : myTags`

(The above expression is literal, and uses JavaScript [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).)

**Scenario E**: Rename an `identification` field to the shorter `ID` – copying over the original field’s value, and removing the old field:
- **Name**: `ID`
- **Value Expression**: `identification`
- **Remove Field**: `identification`

> See [Ingest-time Fields](/stream/usecase-ingest-time-fields) for more examples.
>
{.box .info}

## Usage Notes {#advanced-usage-notes}

Consider the following when working with Eval Functions. 

###  Create Parent Objects First {#parent-object-first}

Before you can use the Eval function on a new child object, you must create the parent object – then define the children using Eval Functions.

##### Example: 

```js
parent  =  (parent || { child1: child1Value })
parent.child2 = child2Value
parent.child3 = child3Value
<some other Evals, if you need them>
```

##### To Append:

```js
parent =  Object.assign(parent, { child2: child2Value })
```

##### To Create a New Field:

```js
parent2 =  Object.assign(parent, { child2: child2Value, child3: child3Value })
```

### Execution Without Assignment  {#execute-only}

The Eval Function can execute expressions **without** assigning their value to the field of an event. You can do this by simply leaving the left-hand side input empty, and having the right-hand side do the assignment.

##### Example: Parse and Merge to Existing Field  {#parse-merge}

`Object.assign(foo, JSON.parse(bar), JSON.parse(baz))` on the right-hand side (and left-hand side empty) will JSON-parse the strings in `bar` and `baz`, merge them, and assign their value to `foo`, an already existing field.

##### Example: Reference Event with `__e`  {#e}

To parse JSON, enter `Object.assign(__e, JSON.parse(_raw))` on the right-hand side (and left-hand side empty).  `__e` is a special variable that refers to the `(context)` event **within** a JS expression. In this case, content parsed from `_raw` is added at the top level of the event.


