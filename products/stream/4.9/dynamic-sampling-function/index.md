# Dynamic Sampling


The Dynamic Sampling Function filters out events based on an expression, a sample mode, and the volume of events. Your sample mode’s configuration determines what percentage of incoming events will be passed along to the next step.

> Each Worker Process executes this Function independently on its share of events. For details, see [Functions and Shared-Nothing Architecture](functions#shared-nothing).
>
{.box .info}

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`.

**Sample mode**: Defines how sample rate will be derived. For formulas and usage details, see [Sample Modes](#sample-modes) below. Supported methods:
 * Logarithmic (the default): `log(previousPeriodCount)`.
 * Square root: `sqrt(previousPeriodCount)`.

**Sample group key**: Expression used to derive sample group key. For example: `${domain}:${httpCode}`. Each sample group will have its own derived sampling rate, based on the volume of events. Defaults to ``` `${host}` ```. 

All events without a host field passing through the Function will be associated with the same group and sampled the same.

### Advanced Settings

   - **Sample period Sec**: How often (in seconds) sample rates will be adjusted. Defaults to `30`.

   - **Minimum events**: Minimum number of events that must be received, in previous sample period, for sampling mode to be applied to current period. If the number of events received for a sample group is less than this minimum, a sample rate of 1:1 is used. Defaults to `30`.

   - **Max sampling rate**. Maximum sampling rate. If the computed sampling rate is above this value, the rate will be limited to this value.

##  How Does Dynamic Sampling Work

Compared to static sampling, where users must first select a sample rate, Dynamic Sampling allows for **automatically adjusting** sampling rates, based on the volume of incoming events per sample group. This Function allows users to set only the aggressiveness/coarseness of this adjustment. Square Root is more aggressive than Logarithmic mode. 

As an event passes through the Function, it's evaluated against the Sample Group Key expression to determine the sample group it will be associated with. For example, given an event with these fields: `...ip=1.2.3.42, port=1234...`, and a Sample Group Key of ``` `${ip}:${port}` ```, the event will be associated with the `1.2.3.42:1234` sample group.

> If the Sample Group Key is left at its ``` `${host}` ``` default, all events without a host will be associated with the same group and sampled the same.
>
{.box .warning}

When a sample group is new, it will initially have a sample rate of `1:1` for `Sample Period` seconds (this value defaults to 30 seconds). Once `Sample Period` seconds have elapsed, a sample rate will be derived based on the configured `Sample Mode`, using the sample group's event volume during the **previous** sample period. 

For example, assuming a Logarithmic Sample Mode: 

**Period 0 (first 30s)** -- Number of events in sample group: `1000`, Sample Rate: `1:1`, Events allowed: `ALL`
Sample Rate calculation for **next** period: `Math.ceil(Math.log(1000)) = 7` 

**Period 1 (next 30s)** -- Number of events in sample group: `4000`, Sample Rate: `7:1`: Events allowed: `572`
Sample Rate calculation for **next** period: `Math.ceil(Math.log(4000)) = 9`   

**Period 2 (next 30s)** -- Number of events in sample group: `12000`, Sample Rate: `9:1`: Events allowed: `1334`
Sample Rate calculation for **next** period: `Math.ceil(Math.log(12000)) = 10`   

**Period 3 (next 30s)** -- Number of events in sample group: `2000`, Sample Rate: `10:1`: Events allowed: `200`
Sample Rate calculation for **next** period: `Math.ceil(Math.log(2000)) = 8`   
...

### Sample Modes {#sample-modes}

1. Logarithmic – The sample rate is derived, for each sample group, using a natural log: `Math.ceil(Math.log(lastPeriodVolume))`. This mode is **less aggressive**, and drops fewer events.

2. Square Root – The sample rate is derived, for each sample group, using: `Math.ceil(Math.sqrt(lastPeriodVolume))`. This mode is **more aggressive**, and drops more events.

## Example
Here’s an example that illustrates the effectiveness of using the Square Root sample mode.

### Settings:
Sample Mode: `Square Root`
Sample Period (sec): `20`
Minimum Events: `3`
Max. Sampling Rate: `3`

### Results:
Events In: 4.23K
Events Out: 1.41K

![](dynamic-sampling-result-crop.4932ba1a96.png)
{border="true"}

In this generic example, we reduced the incoming event volume from 4.23K to 1.41K. Your own results will vary depending on multiple parameters – the **Sample Group Key**, **Sample Period**, **Minimum Events**, **Max Sampling Rate**, and rate of incoming events.

> For further examples, see [Getting Smart and Practical With Dynamic Sampling](https://cribl.io/blog/getting-smart-and-practical-with-dynamic-sampling).
>
{.box .info}

## See Also {#see-also}

- [Sampling Function](sampling-function): Filter out events based on an expression and a sampling rate.