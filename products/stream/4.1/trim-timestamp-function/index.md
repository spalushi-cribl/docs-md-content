# Trim Timestamp (deprecated)


The Trim Timestamp Function removes timestamp patterns from events, and (optionally) stores them in a specified field. 

> This Function is deprecated as of Cribl Stream 4.7, and will be removed in the next major version. Please use the [Eval](eval-function) Function instead.
>
{.box .warning}

This Function looks for a timestamp pattern that exists between the characters indicated by numeric `timestartpos` and `timeendpos` fields. **From events whose** `timestartpos` **value is set to** `0`, the Function removes `timestartpos` and `timeendpos` along with the timestamp pattern.

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this step in the Pipeline. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Field name**: Name of field in which to save the timestamp. (If empty, timestamp will not be saved to a field.)

## Example

Remove the timestamp pattern (indicated by `timestartpos` and `timeendpos`) from `_raw`, and stash it in a field called `time_field`.

**Field name**: `time_field`

**Example event before**:

```
{"_raw": "2020-05-22 16:32:11,359 Event [Event=UpdateBillingProvQuote, timestamp=1581426279, properties={JMSCorrelationID=NA, JMSMessageID=ID:ESP-PD.D2BB2D95F857B:FA323D61, orderType=RatePlanFeatureChange, quotePriority=NORMAL}",
"timestartpos":0,
"timeendpos":23
}
```

To create this example payload, we selected **Sample Data** > **Paste**, pasted the `_raw` field's original contents into the resulting modal, and then added the two required position fields:

![Example event setup](trim-timestamp-paste.5084d9ebdb.png)
{border="true"}

**Example event after**:

```
{"_raw": "Event [Event=UpdateBillingProvQuote, timestamp=1581426279, properties={JMSCorrelationID=NA, JMSMessageID=ID:ESP-PD.D2BB2D95F857B:FA323D61, orderType=RatePlanFeatureChange, quotePriority=NORMAL}",
"time_field":"2020-05-22 16:32:11,359"
}
```

In the Preview pane's **OUT** view, the original timestamp has been removed from `_raw`, and lifted into the new `time_field` we specified in the Function. The `timestartpos` and `timeendpos` fields have been removed.

![Example event, saved and transformed](trim-timestamp-out.d6eb614f95.png)
{border="true"}
