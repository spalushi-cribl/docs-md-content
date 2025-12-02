# Cribl Expression Syntax


As data travels through a Cribl Stream Pipeline, it is operated on by a series of Functions. Functions are fundamentally [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) code.

Functions that ship with Cribl Stream are configurable via a set of inputs. Some of these configuration options are literals, such as field names, and others can be JavaScript [expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions).

Expressions are **valid units** of code that resolve to a value. Every syntactically valid expression resolves to some value, but conceptually, there are two types of expressions: those that **assign** value to a variable (a.k.a., with side effects), and those that **evaluate** to a value.

Assigning a value | Evaluating to a value
--- | ---
`x = 42`<br/>`newFoo = foo.slice(30)` | `(Math.random() * 42)`<br/>`3 + 4`<br/>`'foobar'`<br/>`'42'`

## Filters and Value Expressions
* * * * 

### Filters

Filters are used in [Routes](routes)  to select a stream of the data flow, and in [Functions](functions) to scope or narrow down the applicability of a Function. Filters are expressions that **must** evaluate to either `true` (or [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)) or `false` (or [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)). Keep this in mind when creating Routes or Functions. For example:
* `sourcetype=='access_combined' && host.startsWith('web')`
* `source.endsWith('.log') || sourcetype=='aws:cloudwatchlogs:vpcflow'` 

This table shows examples of truthy and falsy values. 

Truthy | Falsy
--- | ---
`true`<br/>`42`<br/>`-42`<br/>`3.14`<br/>`"foo"`<br/>`Infinity`<br/>`-Infinity` | `false`<br/>`null`<br/>`undefined`<br/>`0`<br/>`NaN`<br/>`''`<br/>`""`

### Value Expressions

Value expressions are typically used in [Functions](functions) to assign a value – for example, to a new field. For example: 
* `Math.floor(_time/3600)`
* `source.replace(/.{3}/, 'XXX')`

##  Best Practices for Creating Predictable Expressions  {#best-practices}

* In a value expression, ensure that the source variable is not `null`, `undefined`, or `empty`. For example, assume you want to have a field called `len`, to be assigned the length of a second field called `employeeID`. But you're not sure if `employeeID` exists. Instead of `employeeID.length`, you can use a safer shorthand, such as: `(employeeID || '').length`.

* If a field does not exist (undefined), and you're doing a comparison with its properties, then the boolean expression will **always** evaluate to false. For example, if  `employeeID` is undefined, then both of these expressions will evaluate to false: `employeeID.length > 10 `, and  `employeeID.length < 10 `.

* `==` means "equal to," while `===` means "equal value **and** equal type." For example, `5 == 5` and `5 == "5"` each evaluate to **true**, while `5 === "5"` evaluates to **false**.

* A ternary operator is a very powerful way to create conditional values. For example, if you wanted to assign either `minor` or `adult` to a field `groupAge`, based on the value of `age`, you could do: `(age >= 18) ? 'adult' : 'minor'`.

###  Fields with Non-Alphanumeric Characters  {#special-chars}

If there are fields whose names include non-alphanumeric characters – e.g., `@timestamp` or `user‑agent` or `kubernetes.namespace_name` – you can access them using `__e['<field-name-here>']`. (Note the single quotes.) More details [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors).

In any other place where the field is referenced – e.g., in the [Eval](eval-function) function's field names – you should use a single-quoted literal, of the form: `'<field-name-here>'`.

## Wildcard Lists

Wildcard Lists are used throughout the product, especially in various Functions, such as [Eval](eval-function), [Mask](mask-function), [Publish Metrics](publish-metrics-function), [Parser](parser-function), etc.

Wildcard Lists, as their name implies, accept strings with asterisks (`*`) to represent one or more terms. They also accept strings that start with an exclamation mark (`!`) to **negate** one or more terms. 

Wildcard Lists are order-sensitive **only** when negated terms are used. This allows for implementing any combination of allowlists and blocklists. 

For example:

Wildcard List | Value | Meaning
--- | --- | ---
List 1 | `!foobar, foo*` | All terms that start with **foo**, except **foobar**.
List 2 | `!foo*, *` | All terms, except for those that start with **foo**.



> You cannot use wildcards to target Cribl Stream internal fields that start with `__`  (double underscore). You must specify these fields individually. For example, `__foobartab` cannot be removed by specifying `__foo*`.
>
{.box .warning}
