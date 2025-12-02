# Schemas Library


Cribl Stream supports two kinds of schemas:
- [JSON schemas](#schema-library) for validating JSON events. Manage these in the UI at **Knowledge** > **Schemas**.
- [Parquet schemas](parquet-schemas) for writing data from a Cribl Stream Destination to Parquet files. Manage these in the UI at **Knowledge** > **Parquet Schemas**.

These schemas obviously serve different purposes. Beware of assuming that operations that work for one kind of schema can be used with the other. For example, the validation method for JSON schemas – `C.Schema('<schema_name>').validate(<object_field>)` – can't be used to validate Parquet schemas.

## JSON Schemas {#schema-library}

JSON schemas are based on the popular [JSON Schema standard](https://json-schema.org/), and Cribl Stream supports schemas matching that standard's [Drafts 0 through 7](https://json-schema.org/specification-links.html).

You can access the schema library from Cribl Stream's top nav under **Processing** > **Knowledge** > **Schemas**. Its purpose is to provide an interface for creating, editing, and maintaining schemas. 

You validate a schema using this built-in method: `C.Schema('<schema_name>').validate(<object_field>)`. 

You can call this method anywhere in Cribl Stream that supports JavaScript expressions. Typical use cases for schema validation: 
* Making a decision before sending an event down to a destination. 
* Making a decision before accepting an event. (E.g., drop an event if invalid.)
* Making a decision to route an event based on the result of validation.

### JSON Schema Example {#json-example}

To add this example JSON Schema, go to **Processing** > **Knowledge** > **Schemas** and click **New Schema**.
Enter the following:
* ID: `schema1`.
* Description: (Enter your own description here.)
* Schema: Paste the following schema.

```json {title="JSON Schema - Sample"}
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Person",
  "type": "object",
  "required": ["firstName", "lastName", "age"],
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The person's first name."
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years which must be equal to or greater than zero.",
      "type": "integer",
      "minimum": 0,
      "maximum": 42
    }
  }
}
```

Assume that events look like this:

``` {title="Events"}
{"employee":{"firstName": "John", "lastName": "Doe", "age": 21}}
{"employee":{"firstName": "John", "lastName": "Doe", "age": 43}}
{"employee":{"firstName": "John", "lastName": "Doe"}}
```

To validate whether the `employee` field is valid per `schema1`, we can use the following:  
`C.Schema('schema1').validate(employee) ` 

Results: 
* First event is **valid**.
* Second event is **not valid** because `age` is greater than the maximum of `42`. 
* Third event is **not valid** because `age` is missing. 

![Schema validation results for the above events](cribl-json-schema-validator.ac16141edc.png)
{border="true"}
