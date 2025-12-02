# Code


If you need to operate on data in a way that can't be accomplished with Cribl Stream's out-of-the-box Functions, the Code Function enables you to encapsulate your own JavaScript code. This Function is available in Cribl Stream 3.1+, and imposes some restrictions for security reasons.

## Restrictions

Generally speaking, anything forbidden in JavaScript [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) is forbidden in the context of the Code Function. Specifically, the following are **not allowed**:

- `console`, `eval`, `uneval`, `Function (constructor)`, `Promises`, `setTimeout`, `setInterval`, `global`, `globalThis`, `window`, and `set`.

Code Functions **can** include `for` loops, `while` loops, and JavaScript methods such as `map`, `reduce`, `forEach`, `some`, and `every`. For further details, see [Supported JavaScript Options](#supported).

Cribl Stream's predefined Functions, such as [Eval](eval-function), cover the vast majority of scenarios that users typically need to implement. You should use Code Functions only as a last resort, when you need to construct a complex block of code. 

Also, only skilled JavaScript developers should define Code Functions. This is to avoid unintended results – such as creating infinite loops, or otherwise failing to return – that could needlessly add to your throughput burden.

## Usage

When added to a Pipeline, the Code Function offers the following configuration options:

**Filter**: JavaScript filter expression that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Optionally, add a simple description of this Function.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Code**: The mini-editor where you type your JavaScript code.

#### Advanced Settings

**Maximum number of iterations**: The maximum number of iterations per instance of this Code Function. Defaults to `5,000`; highest allowed value is `10000`.

**Error log sample rate**: Specifies the rate at which this Code Function logs errors. For example, a value of `1` logs every error; a value of `10` logs every tenth error. The highest allowed value is `5000`. Defaults to `1`.

**Use unique log channel**: When enabled, Cribl Stream sends logs from this function to a unique channel in the form `func:code:${pipelineName}:${functionIndex}`. Toggle off to use the generic `func:code` log channel instead.

## Notes and Examples

Functions (including the Code Function) always use the special variable `__e` to access the `(context)` event inside JavaScript expressions. 

Possibly the simplest Code Function creates a new field and then assigns it a value:

``` {title="A Basic Code Function"}
__e['foo'] = 'Hello, Goats!'
```

For more ambitious implementations, see [Code Function Examples](/stream/usecase-code-function).


##  JavaScript Support  {#supported}

Cribl Stream supports the [ECMAScript® 2015 Language Specification](https://262.ecma-international.org/6.0/).

With some exceptions, the Code Function supports the options described in the following [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) topics:

 - [Expressions and Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)
 - [Global variables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_Types#global_variables)
 - [Control flow and error handling](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
 - [Loops and iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)
 - [Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
 - [Numbers and dates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Numbers_and_dates)
 - [Text formatting](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Text_formatting)
 - [Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
 - [Indexed collections](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Indexed_collections)
 - [Keyed collections](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Keyed_collections)
 - [Working with Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects)
