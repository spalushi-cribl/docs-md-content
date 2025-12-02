# Chain


The Chain Function does one thing: It chains data processing from a [Pipeline](pipelines) or [Pack](packs) to another Pipeline or Pack. This can be useful for sequential processing, or just to separate groups of related Functions into discrete Pipeline or Pack units that make intuitive sense.

> This Function includes guardrails against circular references. Still, Cribl recommends that you keep chained configurations simple and understandable by all your users. Inserting a Chain Function currently imposes a performance hit of about 10%, compared to including all processing in the original Pipeline or Pack.
>
{.box .warning}

## Control Flow


The chained Pipeline or Pack will, upon completion, normally return control to the parent Pipeline/Pack containing the Chain Function. However, if the chained Pipeline/Pack contains any Function with the **Final** flag enabled, processing will stop there. In this case, back in the parent Pipeline/Pack, no Function below Chain will execute.

## Pipeline Versus Pack Scope

You will see different scope restrictions when using Chain in a Pipeline versus in a Pack:

- In a Pipeline, the **Processor** drop-down displays both Pipelines and Packs as targets to chain to.
- In a Pack, the **Processor** drop-down offers only Pipelines contained within that Pack.

## Usage

**Filter**: Filter expression (JS) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Optionally, add a simple description of this Function's purpose. Defaults to empty.

**Final**: If toggled to `Yes`, stops feeding data to the downstream Functions. Defaults to `No`. (Note that this will not prevent data from flowing to the Function's defined **Processor**.)

**Processor**: Use this drop-down to select a configured Pipeline or Pack through which to forward events.

## Example

This shows a simple preview of a `pipeline-1` Pipeline, which chains to a `pipeinpipe` Pipeline. Notice that each event's added `cribl_pipe` field lists all Pipelines/Packs through which the event was chained.

![cribl_pipe field shows whole processing path](chain-cribl-pipe.32e5c420f6.png)
