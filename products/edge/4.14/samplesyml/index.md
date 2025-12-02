# samples.yml


`samples.yml` contains metadata about about stored sample data files (size, number of events, date created, name, and so on).

```yaml {title="samples.yml"}
# ID - Unique identifier for the sample
# [string; required]
id:
# Sample Name - Name of the sample file
# [string; required]
sampleName:
# Pipeline ID - Associate with Pipeline - Select a pipeline to associate with sample with. Select GLOBAL if not
# sure. Deprecated.
# [string]
pipelineId:
# Pack ID - ID of the pack this sample belongs to
# [string]
packId:
# Is Pack Only - Whether this sample only exists within this pack and cannot be turned off
# [boolean]
isPackOnly:
# Description - Brief description of this sample file. Optional.
# [string]
description:
# Tags - One or more tags related to this sample file. Optional.
# [string]
tags:
# Library - Library classification for the sample
# [string]
lib:
# Created - Timestamp when the sample was created
# [number]
created:
# Modified - Timestamp when the sample was last modified
# [number]
modified:
# Is Template - Whether this is a template sample
# [boolean]
isTemplate:
# Size - Size of the sample in bytes
# [number]
size:
# Number of Events - Number of events in the sample
# [number]
numEvents:
# TTL - Time to live (TTL) for the sample; reset after each use. Leave empty to never expire.
# [number]
ttl:
# Timestamp Template Field - Field to use for timestamp templating
# [string]
tsTemplateField:
# Context - Sample context information
context:
  # Context ID - Identifier for the context
  # [string]
  id:
  # Context Pipeline ID - Pipeline ID for the context
  # [string]
  pipelineId:
  # Events - Array of events in the context
  events:
```

The corresponding sample files reside in `$CRIBL_HOME/data/samples`.
