# Sampling


The Sampling Function filters out events, based on an expression and a sampling rate.

> Each Worker Process executes this Function independently on its share of events. For details, see [Functions and Shared-Nothing Architecture](functions#shared-nothing).
>
{.box .info}

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Sampling rules**: Events matching these rules will be sampled at the rates you specify:

* **Filter**: Filter expression matching events to be sampled. Use `true` to match all.

* **Sampling Rate**: Enter an integer `N`. (Defaults to `1`.) Sampling will pick 1/`N` events matching this rule.

## How It Works

Setting this Function’s **Sampling rate** to `30` would mean that only 1 of every 30 events would be kept.

![Sampling Rate](sampling-config-4101.png)
{border="true"}

Let’s assume that we save this setting, and then capture data from a datagen Source by selecting **Preview** > **Start a Capture** > **Capture**. In the **Capture Sample Data** modal, select: `100` seconds, `100` events, and  **As they come in**. Then start the capture, and **Save as Sample File**. 

Next, in the **Preview** pane, select **Simple** beside the new file’s name. If you then select the **Basic Statistics** (chart) button, you should see that we’ve kept about 4 of the original 100 events, or close to 1 in 30.

![](sampling-result.d1c5b52a80.png)
{border="true"}

## Examples and Scenarios {#examples}

For usage examples, see these Better Practices topics:

- [Sampling](/stream/usecase-sampling): Sampling verbose or voluminous data at ingest time, to make analysis and troubleshooting more efficient.
- [Sample Logs](/stream/usecase-sample-logs/): Sampling event data for efficient onbaording into Cribl Stream.
- [Access Logs: Apache, ELB, CDN, S3, Etc.](/stream/usecase-access-logs/): Sampling voluminous logs from sources like Amazon S3, Amazon Cloudfront, and AWS ELBs.
- [Firewall Logs: VPC Flow Logs, Cisco ASA, Etc.](/stream/usecase-firewall-logs/): Sampling voluminous logs from firewalls.

## See Also {#see-also}

- [Dynamic Sampling Function](dynamic-sampling-function): Filter out events based on an expression, a sample mode, and the volume of events