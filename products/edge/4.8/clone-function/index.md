# Clone



The Clone Function creates a copy of events in the same Pipeline, with optional added fields. Cloned events go to the same Destination as the original event, because they are in the same Pipeline. If your Destination is an [Output Router](destinations-output-router), you can use filters to send cloned events to different Destinations. 

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Clones**: Create clones with the specified fields added and set.

**Fields**: Set of key-value pairs to add. Nested addressing is supported.

## Examples

### Staging Example

In this example, the Destination will receive a clone with an `env` field set to `staging`.
 
**Field**: `env`
**Value**: `staging`

### Index Example

In this scenario, we insert a Clone Function at the beginning of a Pipeline to create cloned events. We can later use these events as a baseline to compare against the original events, after various Functions have processed them.

You can assign any meaningful fields to the cloned events – anything  that will help you identify them when comparing. This example simply assigns a key-value pair of `index: clones`.

**Field**: `index`
**Value**: `clones`

To keep the cloned events from being processed by Functions later in the same Pipeline, you'll need to specify `index!='clones'` in their **Filter** expressions.