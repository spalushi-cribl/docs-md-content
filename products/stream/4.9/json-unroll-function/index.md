# JSON Unroll


The JSON Unroll Function processes a JSON object string in the `_raw` field. It unrolls (explodes) an array of objects within this JSON into individual events, while inheriting the top-level fields from the original JSON object. This function is ideal for scenarios where processing individual items within an array is required while retaining the context provided by parent fields.

For efficiency, we recommend avoiding the JSON Unroll Function for certain types of data, such as CloudTrail, Office 365, or Kubernetes CloudWatch logs, which are often collected as JSON arrays. Instead, use an [Event Breaker](event-breakers) for inputs that support it. However, Event Breakers like JSON Array do not retain parent fields from the original JSON object. If retaining parent fields is critical, use the JSON Unroll Function or use a pre-processing Pipeline to extract parent fields before breaking events.

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Path**: Path to array to unroll, e.g., `foo.0.bar`.

**New name**: The name that the exploded array element will receive in each new event. Leave empty to expand the array element with its original name.

## Example(s) 


Assume you have an incoming event that has a `_raw` field as a JSON object string like this: 

```json {title="Sample _raw field"}
 {"date":"9/25/18 9:10:13.000 PM",
    "name":"Amrit",
    "age":42,
    "allCars": [
        { "name":"Ford", "models":[ "Fiesta", "Focus", "Mustang" ] },
        { "name":"GM", "models":[ "Trans AM", "Oldsmobile", "Cadillac" ] },
        { "name":"Fiat", "models":[ "500", "Panda" ] },
        { "name":"Blackberry", "models":[ "KEY2", "Bold Touch 9900" ] }
    ]
 }
```

### Settings: 

**Path**: `allCars` 
**New Name**: `cars`

### Output Events:

```json {title="Resulting Events"}
Event 1:
{"_raw":"{"date":"9/25/18 9:10:13.000 PM","name":"Amrit","age":42,"cars":{"name":"Ford","models":["Fiesta","Focus","Mustang"]}}"}

Event 2:
{"_raw":"{"date":"9/25/18 9:10:13.000 PM","name":"Amrit","age":42,"cars":{"name":"GM","models":["Trans AM","Oldsmobile","Cadillac"]}}"}

Event 3:
{"_raw":"{"date":"9/25/18 9:10:13.000 PM","name":"Amrit","age":42,"cars":{"name":"Fiat","models":["500","Panda"]}}"}

Event 4:
{"_raw":"{"date":"9/25/18 9:10:13.000 PM","name":"Amrit","age":42,"cars":{"name":"Blackberry","models":["KEY2","Bold Touch 9900"]}}"}
```

Each element under the original **allCars** array is now placed in a **cars** field in its own event, inheriting original top level fields; **date**, **name**, and **age**

## See Also {#see-also}

- The [Cribl Knowledge Pack](https://packs.cribl.io/packs/cribl-knowledge-learning-pack) provides sample Pipelines that demonstrate converting a JSON string into an object literal, and validating JSON data against a schema.
