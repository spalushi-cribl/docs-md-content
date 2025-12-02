# Build Custom Logic to Route and Process Your Data


Throughout Cribl Stream, many settings let you use custom JavaScript code and [Cribl Expressions](cribl-reference) to control how data gets routed or processed. You will encounter JavaScript-enabled fields like these in several parts of Cribl Stream:

- **Routes:** Build custom logic to dynamically control where data gets routed.
- **Functions:**
  - Every Function has a **Filter** field that accepts JavaScript to determine which data the Function will process.
  - Some Functions also include additional fields for applying custom logic to event data.

This screenshot shows an example of two common types of JavaScript expression fields in the [Eval Function](eval-function): a filter expression and a value expression. These fields have a `JS` badge to visually indicate that they accept JavaScript input:

![Examples of JavaScript Expression fields](javascript-expression-example.png)
{border="true"}

You can use JavaScript syntax in any field that allows JavaScript expressions. You can also use [Cribl Expressions](cribl-reference), which are pre-built helpers designed for common operations in Cribl Stream. Cribl Expressions begin with `C.`, such as `C.Lookup` or `C.Time`.

> The [JavaScript Expressions](https://sandbox.cribl.io/course/expressions) Sandbox tutorial provides hands-on experience if you would like to learn more about them through personal experimentation and exploration.
>
{.box .info}

## Example Use Case

You can use JavaScript Expression fields to create custom logic for handling data based on your use case. To give a specific use case as an example, perhaps you want to enrich your incoming data by adding a new field called `phoneCompany`, which will assign a value of `Apple` or `Other` based on the contents of another field named `phoneType`.

You could use the [Eval Function](eval-function), which has a **Value Expression** field that accepts custom JavaScript expressions. In that field, you could use the expression `phoneType.startsWith('iPhone') ? 'Apple' : 'Other'`. This JavaScript expression checks if the string `phoneType` starts with `iPhone`. If it evaluates to true, it sets the return value to `Apple`. If not, it sets it to `Other`.

![Adding a field to enrich data](eval-fn-enrich-4101.png)
{border="true"}

## Types of JavaScript Fields

### Filter Expressions {#filters}

You will use filter expressions in:

- [Routes](routes) to dynamically control which data goes to which Pipelines.
- [Functions](functions) to set the scope or narrow down which data gets processed by that Function.

The **Filter** field on Functions or Routes can accept custom filter expressions to control whether a Function or Route processes each event. Filter expressions must evaluate to either `true` or `false`:

- If the result is `true`, the data gets processed by that Route or Function.
-	If the result is `false`, the data will pass through unchanged.

By default, the **Filter** field is set to `true`, meaning the Route or Function applies to all events. You can replace this with a custom JavaScript expression to apply the Route or Function only to data that matches specific conditions.

### Value Expressions

Use value expressions in Functions to assign or calculate values dynamically. Use these expressions to create new fields, modify existing ones, or perform transformations on data.

For example, you can:

- Convert a timestamp to an hour-based value: `Math.floor(_time / 3600)`
- Mask part of a Source string: `source.replace(/.{3}/, 'XXX')`

Anywhere a Function lets you define a value with JavaScript, you can use a value expression to apply custom logic based on the content of the event.

## Test and Validate Expressions in Advanced Mode {#advanced-mode}

To test your filter expression against sample input data, click the **Advanced mode** button within the **Filter expression** field. The **Filter expression** modal will open.

Here, you can paste in your JavaScript expression to filter the data. You can also select a sample JSON input from the drop-down or enter your own sample data directly in the **Sample input** field.

![The **Advanced mode** window](process-metrics-advanced-mode.851caed8ad.png)
{scale="90%" border="true"}

The **Output** shows you whether your expression is valid or not:

- For filter expressions, the output will resolve to `true` if it is a valid expression, which means that it can correctly apply the custom filter to the sample data. A value of `false` means you need to fix the syntax of the expression or ensure it applies to the sample data.
- For value expressions, the output will show you if the filter expression returns any matching processes, based on the filter expressions and JSON input.

In most cases, you can use Advanced Mode to test and validate JavaScript that uses Cribl Expressions as well.

> If you are testing an expression that uses the [Lookup](lookup-function) Cribl Expression (`C.Lookup`), be aware of lookup limitations. To avoid overloading your web browser, lookup files larger than 10 MB are not loaded and will not evaluate in Advanced Mode. However, if the expression is valid, the updated values are still correctly applied to route and process data.
>
{.box .warning}

## Best Practices to Create Predictable Expressions {#best-practices}

- In a value expression, ensure that the source variable is not `null`, `undefined`, or `empty`. For example, assume you want to have a field called `len`, to be assigned the length of a second field called `employeeID`. But you're not sure if `employeeID` exists. Instead of `employeeID.length`, you can use a safer shorthand, such as: `(employeeID || '').length`.

- If a field does not exist (undefined), and you're doing a comparison with its properties, then the boolean expression will **always** evaluate to false. For example, if  `employeeID` is undefined, then both of these expressions will evaluate to false: `employeeID.length > 10 `, and  `employeeID.length < 10 `.

- `==` means "equal to," while `===` means "equal value **and** equal type." For example, `5 == 5` and `5 == "5"` each evaluate to **true**, while `5 === "5"` evaluates to **false**.

- A ternary operator is a very powerful way to create conditional values. For example, if you wanted to assign either `minor` or `adult` to a field `groupAge`, based on the value of `age`, you could do: `(age >= 18) ? 'adult' : 'minor'`.

### Fields with Non-Alphanumeric Characters {#special-chars}

If there are fields whose names include non-alphanumeric characters, such as `@timestamp` or `user‑agent` or `kubernetes.namespace_name`, you can access them using `__e['<field-name-here>']`. (Note the single quotes.) More details are available on the [Mozilla Developer Docs topic on Property accessors.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors).

In any other places that reference the field, such as the [Eval](eval-function) function's field names, you should use a single-quoted literal, of the form: `'<field-name-here>'`.

### Wildcard Lists

Wildcard lists appear in various Functions, such as [Eval](eval-function), [Mask](mask-function), [Publish Metrics](publish-metrics-function), [Parser](parser-function).

Wildcard lists accept strings with asterisks (`*`) to represent one or more terms. They also accept strings that start with an exclamation mark (`!`) to **negate** one or more terms. This allows for implementing any combination of allowlists and blocklists.

Wildcard lists are order-sensitive, evaluated from left to right. This is especially relevant when you use negated terms. For negations to take precedence over wildcards when evaluated, you must list negations before wildcards.

Some examples:

| Wildcard List | Value | Meaning |
| ------------- | ----- | ------- |
| List 1 | `!foobar, foo*` | All terms that start with **foo**, except **foobar**. |
| List 2 | `!foo*, *` | All terms, except for those that start with **foo**. |
| List 3 | `*, !foo` | All terms (wildcard matches first, negation isn't evaluated). |

> You cannot use wildcards to target Cribl Stream internal fields that start with `__`  (double underscore). You must specify these fields individually. For example, `__foobartab` cannot be removed by specifying `__foo*`.
>
{.box .warning}
