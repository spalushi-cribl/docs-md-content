# Cribl Guard Performance Considerations

When you enable Cribl Guard – either through the Guard homepage or by using the
Cribl Guard Function – you may notice that Cribl Guard consumes additional
processing resources. Here, we answer some questions you may have about
performance in your Cribl Stream environment.

## Key Performance Implications

The existing [Cribl Stream performance guidelines](/stream/deploy-planning)
still apply when you're using Cribl Guard. However, Cribl Guard does consume additional
processing resources. Depending on the number of rules you have enabled, you may
need to increase your resource capacity.

As you add more Cribl Guard rules to a Function, you must plan for proportional
resource utilization and scale accordingly. 

## Ways to Optimize Performance

You can optimize the performance of Cribl Stream while Cribl Guard protects your
Destinations using these recommendations:

- **Filter data**: Use the **Filter** field in the Cribl Guard Function to ensure
  you're only sending data through the Function that you definitely want to be 
  scanned.
- **Apply to the right fields**: Cribl Guard scans events in `_raw` by default. 
  You can narrow down scanning to specific fields and disable scanning to limit
  the volume of data flowing through the Cribl Guard Function.
- **Use a Post-Processing Pipeline**: Apply Cribl Guard in a Post-Processing
  Pipeline ahead of your Destination, after you've dropped unnecessary fields.
  This ensures you're scanning as little data as possible.
- **Create custom rulesets**: Tailor custom rulesets that apply to your specific
  data flowing through your Pipelines.

## Monitor and Review your Configuration

To stay ahead of potential performance implications, monitor and review your 
Cribl Guard configuration:

- Reevaluate the scope of scanned data to ensure it's the most relevant to your
  business case.
- Review the Cribl Guard rules and rulesets in your Cribl Guard Function, making
  sure they're tailored to the needs of your environment.
- Monitor the [Cribl Guard homepage metrics](guard-configure#explore-the-cribl-guard-homepage)
  for efficient sensitive data scanning and mitigation.
