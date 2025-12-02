# Guard


In Cribl Stream, the Cribl Guard Function allows you to dynamically scan
incoming data for sensitive fields like personally identifiable information
(PII) and then obfuscate those events in a Pipeline. This Function helps you
identify sensitive data in your data streams and prevent it from being exposed
in downstream analytics systems whose primary users don’t need PII access.

Use this Function to:

* **Reduce PII in your data:** and thereby decrease your risk of legal risk in
  the event of a data breach.  
* **Responsibly handle sensitive data:** in highly regulated industries like
  finance, healthcare, law firms, and technology.

## How the Cribl Guard Function Works

The Cribl Guard Function processes data in the following order:

1. **Filter:** Searches events that match the JS filter and that are located
   within the **Apply to** fields and skips the **Fields to ignore**. Applies
   the rules in the selected scanning ruleset(s).   
2. **Scan:** Looks for sensitive data in the matching filter and rules.  
3. **Mitigate:** Applies mitigation to sensitive data based on the rules in your
   ruleset. For example, the Default ruleset redacts sensitive data values.

You can add the Cribl Guard Function to any Pipeline to manually scan incoming
data streams. We recommend adding the Cribl Guard Function in your
Post-Processing Pipeline.



## Pipeline Recommendations for the Cribl Guard Function

There are no hard constraints around where in your Pipeline you add the Cribl
Guard Function. However, there are considerations we suggest you take into
account.

- We strongly recommend that you only use the Cribl Guard
  Function in **one** of your Pipelines (Post-Processing is our formal
  recommendation). This is because Cribl Guard is billed based on GB of data
  scanned. You could inadvertently be double- or triple-charged (or more) if you
  use the Cribl Guard Function in multiple Pipelines that scan the same set of
  data.

### Post-Processing Pipeline

We highly recommend configuring the Cribl Guard Function in a **Post-Processing
Pipeline**. Here’s why:  

- With the Cribl Guard Function in your Post-Processing Pipeline, sensitive data
  is mitigated before Cribl sends it to your Destinations. This means sensitive
  data is much less likely to reach your Destinations, reducing the risk of
  personal data leaks.
- You’ll avoid accidentally incurring multiple charges on processed data. Cribl
  Guard charges based on how many GB of data it scans, so by placing the Guard
  Function in your Post-Processing Pipeline, you only scan data that you’re
  certain you want to send to a Destination. 
- Cribl Guard scans data in its final form: *after* data goes through the
  JavaScript filter in the Pipeline, is enriched, adjusted, enhanced, and so on.
  This ensures you’re scanning the smallest subset of events.

Of course, by placing the Cribl Guard Function in a Post-Processing Pipeline,
there is the possibility that Cribl might scan the same data multiple times as
it routes to different Destinations.

### Pre-Processing Pipeline

By configuring the Cribl Guard Function in your Pre-Processing Pipeline, you can
quickly identify Sources that might be unreliable or untrustworthy. This
enables you to filter and mask data as soon as Cribl Guard identifies it.

However, it’s not possible to keep unredacted and redacted data separate when
you use Cribl Guard in a Pre-Processing Pipeline. Also, you might end up
scanning (and paying for) data that you discard later in the Pipeline.

## Configure the Cribl Guard Function

1. Add a Source and Destination to begin generating data. See
   [Integrations](/stream/integrations) for more information.  
1. Add a new Pipeline or open an existing Pipeline. Add or capture sample data
   and test your Pipeline as you build it. See [Pipelines](/stream/pipelines) for more
   information.
1. At the top of the Pipeline, select Add Function and search for Cribl Guard,
   then select it.
1. In the resulting Cribl Guard Function modal, configure the following general
   settings:
    - **Filter:** A JavaScript filter expression that selects which metrics
     to process through the Function. Defaults to `true`, meaning it
     evaluates all events.
    - **Description:** A brief description of how this Function modifies
     your metrics to help other users understand its purpose later.
     Defaults to empty.
    - **Final:** When enabled, stops data from continuing to downstream
     Functions for additional processing. Default: Off. Toggle **Final** on if
     Cribl Guard is the last Function in your Pipeline.
    - **Scanning Rulesets:** The Rulesets in this Pipeline that scan
     incoming events for sensitive data. You can add as many Scanning
     Rulesets as you like to a Pipeline.
        - **Ruleset ID:** The unique ID of the ruleset used in this scan.
        - **Mitigation Expression:** Content that matches the scanning rule will
          be replaced by a new value defined in this mitigation expression,
          which can be either a JavaScript expression (that manipulates the
          data) or a literal (like ‘REDACTED’). You can use capturing groups to
          isolate specific parts of matching text, then reference them in your
          mitigation expression as `g1`, `g2`, and so on.
        - **Enabled:** You can enable or disable scanning rulesets within a Pipeline.
    - **Apply to fields:** Cribl Guard will only scan data that is included in
      the fields you designate here.
    - **Fields to ignore:** Cribl Guard will not scan data that is included in
      the fields you designate here.
    - **Advanced Settings**
        - **Add fields:** Enter fields that Cribl Guard will add to mitigated events.
          `_sensitive` is `true` by default, which means it will appear in event
          data when sensitive fields have been mitigated.
        - **Add matched rulesets:** When enabled, Cribl adds an internal field
          called `__detected` to the incoming event. This field will contain the
          ruleset ID or array of IDs that match the sensitive data. For example,
          `__detected: rule1`  or `__detected: [rule1, rule2 ….ruleN].`
1. Use the Simple Preview or Full Preview pane combined with a sample data file
   to test that your Function is accurately scanning, filtering, and mitigating
   any sensitive data.
1. Send the newly configured Pipeline Function to your Stream Workers using
   **Commit & Deploy**.

Cribl Guard uses rulesets, rather than relying on rigid pattern matching, which
results in faster, more accurate data processing. You can use a combination of
out-of-the-box rulesets and custom rulesets. To see all the Cribl Guard rulesets
included out-of-the-box, go to the [Cribl Guard Rulesets](/guard/guard-rulesets) reference documentation.

## Event Fields and Internal Fields

There are four fields that the Cribl Guard Function can add to your events:

- `_sensitive`: Included by default in **Advanced Settings**, this field is added
  to mitigated event data.
- `__detected`: When **Add to matched rulesets** is enabled, your events will contain
  this internal field.
- `__potential_sensitive`: This internal field is added to event data when a data string
  matches the **Pattern to match** expression but does not match an anchor term.
  You can then manually adjust the match pattern or anchor terms to mitigate the
  data if it is sensitive.
- `__potential_sensitive_data`: This internal field shows you which events are
  potentially sensitive.

>You must enable **Show internal fields** in the preview settings to see the
>`__potential_sensitive` and `_potential_sensitive_data` fields in your data preview.
>See [Data Preview Advanced Settings](/stream/data-preview#advanced-settings) for setup steps.
{.box .info}

## Usage Examples

A common usage example for Cribl Guard is to redact plain text as replacement
expressions for the sensitive data. For example, this Pipeline uses the Default
ruleset, which applies `REDACTED` as the mitigation expression to matching sensitive
data.

![Example Guard mitigation](guard-example.png)
{border="true" scale="85%"}
