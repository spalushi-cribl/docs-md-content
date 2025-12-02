# Eval


The Eval Function adds, modifies, or removes event fields. Eval is optimized for simple, single-value assignments and transformations using JavaScript expressions. For complex logic or multi-step processing, use the [Code Function](code-function).

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Evaluate fields**: Table of event fields to evaluate and add/set. Select **Add Field** to add each field as a key-value pair, in a row with the following controls:
- **Name**: Enter the new (or modified) field's name.
- **Value Expression**: Enter a JavaScript expression to set the field's value – this can be a constant. Expressions can contain nested addressing. When you insert JavaScript template literals, strings intended to be used as values must be delimited with single quotes, double quotes, or backticks. For specific constraints, see [Value Expression Limitations](#limitations). For details on fields with non-alphanumeric characters, see [Build Custom Logic to Route and Process Your Data: Fields with Non-Alphanumeric Characters](filter-and-transform-data#special-chars).
- **Enabled**: Toggle this off to disable evaluating individual expressions, while retaining their configuration in the table. Useful for iterative development and debugging.

**Keep fields**: List of fields to keep in the event after processing by this Function. Takes precedence over the **Remove fields** setting. The field accepts wildcards (`*`), which supports both:

- Wildcards (`*`) to include multiple fields.
- Nested addressing, where you can reference a parent field and all its children using a wildcard. For example, if you convert `_raw` into an object, use `_raw*` to refer to `_raw` and all nested fields inside it. See [Wildcard Lists](filter-and-transform-data#wildcard-lists) for more information.

**Remove fields**: List of fields to remove from the event. Supports wildcards (\*) and nested addressing. Cannot remove fields that match against the **Keep fields** list. Cribl Stream internal fields that start with `__`  (double underscore) **cannot** be removed via wildcard. Instead, you must specify them individually. For example, `__myField` cannot be removed by specifying `__myF*`.



### Value Expression Limitations {#limitations}

The **Value Expression** supports simple, single-value assignments and transformations. For advanced logic and complex operations, use the [Code Function](code-function).

The following limitations apply to expressions:

1.  **Reserved Words**: Expressions cannot utilize JavaScript **reserved words** like `constructor`, `prototype`, and `__proto__`.

    Cribl responds with: `Error: Expression cannot make use of the reserved word '<word>'`.

1.  **Setters**: The definition or use of JavaScript **setters** is not allowed within expressions.

    Cribl responds with: `Error: Expression cannot use setter`.

1.  **Assignment Operators**: Assignment operators like `=`, `+=`, `-=` are not permitted, except for direct assignment to the configured field. For example, `myField = <expression>`.

    Cribl responds with: `Error: Unallowed assignment operator found in expression`.

1.  **Nested Scopes**: Expressions cannot introduce **nested scopes**. This includes, but is not limited to:
    * Loops (`for`, `while`, `do-while`)
    * Array iteration methods (`.map`, `.forEach`, `.reduce`, and so on)
    * Control structures (`if`, `switch`, `try-catch`)

    Cribl responds with: `Error: Expression cannot have nested scopes`.

1.  **Internal Expressions**: Expressions are prevented from referencing internal Cribl expressions, such as `C.internal`.

    Cribl responds with: `Error: Expression cannot reference 'C.internal'`.

1.  **Declarations**: Variable or function declarations are not allowed. This includes:
    * Variable declarations (`let`, `const`, `var`)
    * Function definitions (function declarations, arrow functions, function expressions)

    Cribl responds with: `Error: Expression cannot have any declarations`.

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

**Scenario C**: Create a multivalued field called `myTags` (array):
- **Name:**  `myTags`
- **Value Expression:** `['failed', 'blocked']`

**Scenario D**: Add value `error` to the multivalued field `myTags`:
- **Name:**  `myTags`
- **Value Expression:** `login=='error' ? [...myTags, 'error'] : myTags`

(The above expression is literal, and uses JavaScript [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).)

**Scenario E**: Rename an `identification` field to the shorter `ID` – copying over the original field's value, and removing the old field:
- **Name**: `ID`
- **Value Expression**: `identification`
- **Remove Field**: `identification`

## See Also {#see-also}

For more usage examples, see these Better Practices topics:

- [Ingest-time Fields](/stream/usecase-ingest-time-fields), which demonstrates both adding and removing fields.

- [Reducing Windows XML Events](/stream/usecase-win-xml/), which demonstrates using Eval with the `C.Text.parseWinEvent` expression method to parse `_raw` Windows XML events into prettified JSON objects.

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

To parse JSON, enter `Object.assign(__e, JSON.parse(_raw))` on the right-hand side (and left-hand side empty).  `__e` is a special variable that refers to the `(context)` event **within** a JavaScript expression. In this case, content parsed from `_raw` is added at the top level of the event.


