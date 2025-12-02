# Variables Library


Variables are reusable JavaScript expressions that can be accessed in [Functions](functions) in any [Pipeline](pipelines).

## Variable Scope

Variables are scoped to different levels of the application: Single-instance deployment, Fleet, or Pack.
You can only use a variable in the scope in which it is defined.

### Single-Instance Deployment Variables

To access variables in a [Single-instance deployment](deploy-single-instance):

1. Select **Home** from the sidebar.
1. On the submenu, select **Processing**, then **Knowledge**.
1. In the sidebar, select **Variables**.

You cannot access these variables from a Pack in this deployment.

### Fleet Variables {#group-variables}

To access variables specific to a given Fleet:

1. Select **Fleets** from the sidebar, and choose a Fleet.
1. On the **Fleets** submenu, select **More**, then **Knowledge**.
1. In the sidebar, select **Variables**.

You cannot access these variables from a Pack in this Fleet or deployment.

### Pack Variables

To access variables that belong to a specific [Pack](packs):

1. Select **Fleets** from the sidebar, and choose a Fleet.
1. On the **Fleets** submenu, select **More**, then **Packs**.
1. Select the desired Pack.
1. In the Pack submenu, select **Knowledge**, then in the sidebar select **Variables**.

You cannot access these variables from outside a Pack (for example, from a Pipeline configured in a Fleet).

## Use Cases

Typical use cases for variables include: 

- Storing a constant that you can reference from any Function in any Pipeline.
- Storing a relatively long value expression, or one that uses one or more arguments. 

Variables can be of the following types: 

- Number
- String
- Encrypted String
- Boolean
- Object
- Array 
- Expression

Variables can be accessed via `C.vars` – which can be called anywhere in Cribl Edge that JS expressions are supported. Typeahead is provided. See [Cribl Expressions](cribl-reference) for more information.

> When calling `C.vars`, the value used will depend on the [scope](#variable-scope) of the variable.
> For example, if you call `C.vars.redis` from a Pipeline Function in a Pack,
> the value returned will be the `redis` Pack variable, not `redis` as defined in the Fleet.
{.box .info}

## Examples 

### Scenario 1

Assign field `foo` the  value in `theAnswer` variable.

- Variable Name: `theAnswer`
- Variable Value: `42` 
- **Sample Eval Function:** `foo = C.vars.theAnswer` 

### Scenario 2

Assign field `nowEpoch` the current time, in epoch format.

- Variable Name: `epoch`
- Variable Value: `Date.now()/1000` 
- **Sample Eval Function:** `nowEpoch = C.vars.epoch()` 

### Scenario 3

Create a new field called `storage`, by converting the value of event field `size` to human-readable format.

- Variable Name: `convertBytes`
- Variable Value: ``` `${Math.round(bytes / Math.pow(1024, (Math.floor(Math.log(bytes) / Math.log(1024)))), 2)}${['Bytes', 'KiB', 'MiB', 'GiB', 'TiB', 'PiB', 'EiB', 'ZiB', 'YiB'][(Math.floor(Math.log(bytes) / Math.log(1024)))]}` ``` <br/>
Note the use of quotes or backticks around values. Use the opposite delimiter for the enclosing expression.
- Variable Argument: `bytes` 
- **Sample Eval Function:** `storage = C.vars.convertBytes(size)`  <br/>
Note the use of `bytes` here as an argument.
