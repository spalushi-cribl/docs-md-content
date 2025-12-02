# Regex Filter


The Regex Filter Function filters out events based on regex matches.

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Regex**: Regex to test against. Defaults to empty.

**Additional regex**: Select **Add Regex** to chain extra regex conditions.

**Field**: Name of the field to test against the regex. Defaults to `_raw`. Supports nested addressing.

## Examples 

For a usage example, see our [Regex Filtering](/stream/usecase-regex-filtering) topic.
