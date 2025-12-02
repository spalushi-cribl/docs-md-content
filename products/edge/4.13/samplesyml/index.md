# samples.yml


`samples.yml` contains metadata about about stored sample data files (size, number of events, date created, name, etc.). Each sample is listed according to the following pattern:

```yaml {title="samples.yml"}
sample_id: # [object] 
  sampleName: # [string] File Name - Filename to save the sample as. Required.
  pipelineId: # [string] Associate with Pipeline - Select a pipeline to associate with sample with. Select GLOBAL if not sure. Deprecated.
  description: # [string] Description - Brief description of this sample file. Optional.
  ttl: # [number] Expiration (hours) - Time to live for the sample, the TTL is reset after each use of the sample. Leave empty to never expire.
  tags: # [string] Tags - One or more tags related to this sample file. Optional.
```

The corresponding sample files reside in `$CRIBL_HOME/data/samples`.
