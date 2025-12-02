# Regex Filter


The Regex Filter Function filters out events based on regex matches.

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Regex**: Regex to test against. Defaults to empty.

**Additional regex**: Click **Add Regex** to chain extra regex conditions.

**Field**: Name of the field to test against the regex. Defaults to `_raw`. Supports nested addressing.

## Examples 

See [Regex Filtering](/stream/usecase-regex-filtering) for examples.
