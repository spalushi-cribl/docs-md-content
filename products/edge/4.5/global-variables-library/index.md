# Global Variables Library


## What Are Global Variables

Global Variables are reusable JavaScript expressions that can be accessed in [Functions](functions) in any [Pipeline](pipelines). You can access the library from Cribl Edge's top nav under **Processing** > **Knowledge** > **Global Variables**.

Typical use cases for Global Variables include: 

* Storing a constant that you can reference from any Function in any Pipeline.
* Storing a relatively long value expression, or one that uses one or more **arguments**. 

Global Variables can be of the following types: 

* Number
* String
* Boolean
* Object
* Array 
* Expression

Global Variables can be accessed via `C.vars.` – which can be called anywhere in Cribl Edge that JS expressions are supported. Typeahead is provided. More on Cribl Expressions [here](cribl-reference).

## Examples 

### Scenario 1:

Assign field `foo` the  value in `theAnswer` Global Variable. 
* Global Variable Name: `theAnswer`  <-- ships with Cribl Edge by default.
* Global Variable Value: `42` 
* **Sample Eval Function:** `foo = C.vars.theAnswer` 

### Scenario 2:

Assign field `nowEpoch` the current time, in epoch format. 
* Global Variable Name: `epoch`   <-- ships with Cribl Edge by default.
* Global Variable Value: `Date.now()/1000` 
* **Sample Eval Function:** `nowEpoch = C.vars.epoch()` 

### Scenario 3:

Create a new field called `storage`, by converting the value of event field `size` to human-readable format. 
* Global Variable Name: `convertBytes`  <-- ships with Cribl Edge by default

* Global Variable Value: ``` `${Math.round(bytes / Math.pow(1024, (Math.floor(Math.log(bytes) / Math.log(1024)))), 2)}${['Bytes', 'KiB', 'MiB', 'GiB', 'TiB', 'PiB', 'EiB', 'ZiB', 'YiB'][(Math.floor(Math.log(bytes) / Math.log(1024)))]}` ``` <br/>
Note the use of quotes or backticks around values. Use the opposite delimiter for the enclosing expression.

* Global Variable Argument: `bytes` 

* **Sample Eval Function:** `storage = C.vars.convertBytes(size)`  <br/>
Note the use of `bytes` here as an argument.
