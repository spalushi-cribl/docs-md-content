# Code Function Examples


The [Code Function](code-function) is a powerful way to transform events without needing to write a [custom function](https://cribl.io/blog/extending-cribl-building-custom-functions/). Using JavaScript methods such as `map`, `reduce`, `forEach`, `some`, and `every` is possible.

Cribl still recommends that you use Cribl Stream's basic, built-in Functions, like [Eval](eval-function), as much as possible. But these use cases demonstrate some basic ways to use the new Code Function to solve questions asked in the Cribl's Slack Community.

## Basic Examples

### Example Event Data

The first several examples below use the following JSON object as a sample event. You can copy and paste this event into Cribl Stream's **Sample Data** pane. Add the `Do Not Break` Ruleset to your Source, select the `noBreak1MB` Event Breaker, and under your Source's **Advanced Settings**, enable `Parse JSON Event`.

```json
{
  "cpus": [
    {"number": 1, "name": "CPU1", "value": 2.3},
    {"number": 2, "name": "CPU2", "value": 3.1},
    {"number": 3, "name": "CPU3", "value": 5.1},
    {"number": 4, "name": "CPU4", "value": 1.3}
  ],
  "arch": "Intel x64"
}
```

### Accessing Fields in an Event

To access a field inside an event, you can use the `__e`  [special variable](eval-function#e). The `__e` prefix allows for access to the  `(context)` event inside a JavaScript expression. For example, if you want to access the extracted field `field‑name`, use the following syntax:

```js
__e['field-name']
```

In other words, think of your code executing in a context like this:

```js
function(__e: Event) {
 // your code here
}
```

### Eval a Field

To create a new field, it is as simple as assigning the field to a value. For example, if you want to create a field `test` with the value `Hello, Goats!`, use the following syntax:

```js
__e['test'] = 'Hello, Goats!'
```

### JSON Filter

To filter the `cpus` array inside the event, you can use the `filter` function to keep only certain values, based on a logic condition. In the example below, only entries where the value is greater than or equal to `3` are kept, and placed into a new field called `cpus_filtered`.

```js
__e['cpus_filtered'] = __e['cpus'].filter(entry => entry.value >= 3)
```

![Code Function example: JSON Filter](code-fn-1-filter.257d58181d.png)
{border="true"}

### Reduce

The `reduce` method allows you to summarize data across an array, with a returned accumulator value. In the example below, a new field, `cpus_reduce`, will be created with a value of `11.8`.

```js
__e['cpus_reduce'] = __e['cpus'].reduce((accumulator, entry) => accumulator + entry.value, 0)
```

![Code Function example: Reduce](code-fn-2-reduce.a25cfd4461.png)
{border="true"}

### Some/Every

The `some` and `every` methods return a boolean result (`true`/`false`).

The `some` method checks that there is at least one logic validation that returns `true`. In the example below, `cpus_some` would be set to `true` because there is at least one object with a value greater than or equal to `3`.

```js
__e['cpus_some'] = __e['cpus'].some(entry => entry.value >= 3)
```

The `every` method  checks that every entry in the array returns `true`. If so, the result is `true`; otherwise, this returns `false`. In the example below, the value for `cpus_every` would be `false`, because not all values in the event are greater than or equal to `10`.

```js
__e['cpus_every'] = __e['cpus'].every(entry => entry.value >= 10)
```

![Code Function example: Some/Every](code-fn-3-some-every.41b963bc7b.png)
{border="true"}

## Advanced Examples

### Transform a Specific Field

In this example, each `cpus` member will have its `name` field transformed to lowercase. [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) is used to keep the original object, while assigning the `name` field to the desired value.

```js
__e['cpus'] = __e['cpus'].map(entry => Object.assign(entry, {'name': entry.name.toLowerCase()}))
```

![Code Function example: Transform a Field](code-fn-4-assign.83f39b0af9.png)
{border="true"}

### forEach

The `forEach` method loops over each element of an array. However, unlike the `map` method, it does not return the value, and it requires a separate temporary variable for result collection.

```js
let test = {};
__e['cpus'].forEach((entry, index) => test[entry.name] = entry.value);
__e['cpus_foreach'] = test;
```

![Code Function example: forEach](code-fn-5-forEach.d4471f8bbe.png)
{border="true"}

You could also accomplish the same result by replacing the middle line above with the `map` method below:

```js
__e['cpus'].map(item => test[item.name] = item.value);
```

### Building a New Array

In this example, we create a new array to include some of the original values from each object in the `cpus` array, but we also dynamically inject a new field containing the `arch` field (CPU architecture) from the original event's top level.

```js
__e['cpus_new'] = __e['cpus'].map(entry => {
    const container = {};

   for (const [key, value] of Object.entries(entry)) {
        container[key] = value;
    }

   container['arch'] = __e['arch'];

   delete container.name;
    return container;
})
```

![Code Function example: Build a new array](code-fn-6-map-array.7fc698a42b.png)
{border="true"}

## Managing and Troubleshooting Code Execution

Cribl Stream provides the following options for tuning the logic you build with the Code Function.

### Logging

Users can access the log messages generated by their Function from the **Preview Log**.

![Log messages from a Code Function](code-fn-7-log-error.f456b3cc8b.png)
{border="true"}

### Debugging

The Code Function’s execution context defines a helper function called `debug` that can be used for debugging purposes.

Messages logged by this `debug` helper function are shown in the Preview Log by default. 

You can also route these messages to regular logs, although this requires setting the Function's log level (`func:code`) to `debug`.

### Previewing

By using the expanded editor, users can run the expression against a sample event, and can preview the transformation:

![Previewing a Code Function's transformation of an event](code-fn-11-expression-editor.b33b6a3f1f.png)
{border="true"}

### Infinite-Loop Protection

The Code Function keeps watching user-defined functions to detect infinite loops that would cause processing to hang. To limit the number of iterations allowed per instance of your Code Function, adjust the **Advanced Settings >** **Maximum number of iterations** option. This defaults to `5,000`; the maximum number allowed is `10,000`. Once the limit is reached, the Code Function will stop processing whatever is after the statement that exhausted the allowed maximum.

![Infinite-Loop protection in a Code Function](code-fn-12-max-iterations.fda8558296.png)
{border="true"}

### Loops

All JavaScript loops and statements are allowed: `for`,` for-of`, `while`, `do-while`, `switch`, etc.

### Functions

Users can define their own functions to better organize their code. Both traditional and arrow functions are allowed.

### Access to `C.*` Global Object

From the Code’s Function body, you can access Cribl Stream's global `C.*` object and its [methods/expressions](cribl-reference).

![The global `C.*` object of a Code Function](code-fn-13-internal-methods.dd80c115d1.png)
{border="true"}




