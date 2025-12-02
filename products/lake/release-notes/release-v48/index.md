# Cribl Lake 4.8


## New Features

### Hybrid and Azure-based Worker Support

Cribl Lake Collectors and Destinations can now receive data from and send data to all Stream Workers regardless of their type:
whether in customer-managed (hybrid), or Cribl-managed Worker Groups in Cribl.Cloud.
Cribl-managed Worker Groups can now be based in Azure as well as in AWS, and Cribl Lake supports both providers.

See [Stream release notes](/stream/release-notes/release-v48) for more details about Azure-based Worker Groups.

## Improvements

### Typeahead for Datasets 

Cribl Lake now offers typeahead to speed up the way you work with your Datasets.
When [exporting to Lake](collectors-cribl-lake) a Dataset will be suggested based on the first characters you type.
Similarly, when you [search the Lake](search-cribl-lake),
the query box will autosuggest any configured [accelerated fields](managing-datasets#accelerated-fields) once you start typing.
