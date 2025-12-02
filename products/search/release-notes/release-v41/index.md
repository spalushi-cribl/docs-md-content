# Cribl Search 4.1


## New Features

### New Data Format Support

We’ve added support for some binary data types and formats: Systemd/Journald, Parquet, and Splunk rawdata files.

### Search-and-Forward

Search results can be sent to Cribl Stream for preconfigured shaping and routing. A new **send** operator enables sending Search results to Stream, where the results can be further processed and routed using any of the usual Stream features.

### New Stream Destination: Data Lake > Amazon S3

A new Destination, under **Data Lake > Amazon S3** now appears in Stream. This is an AWS S3-based Destination that has a Cribl Search-friendly partitioning scheme.

### Enhanced User Experience

**New operators**: [`count`](operators-count), top, sort, and [`centralize`](centralize).
* The [`count`](operators-count) operator returns the number of all events up to that point in the Search pipeline.
* The **top** operator returns the first N events, sorted by the specified fields, allowing you to identify the most relevant events in your data quickly. 
* The **sort** operator arranges events into order by one or more fields, enabling you to better organize and analyze their data. 
* The [`centralize`](centralize) operator is useful in scenarios where executors lack network visibility to the destination. Now you can easily streamline and optimize the flow of data from search results, enabling faster and more efficient analysis.

**Operator Preview**: You can now preview operators to see their effect on searched data. Easily modify your query and immediately see how it affects your results, enabling you to quickly and easily test different scenarios.

**Performance enhancement**:
* **Multi-region support**: significant performance improvement by moving search processes closer to where your data resides. This significantly reduces the time it takes to search and analyze data, making searches faster and more efficient.
* **Compute Infrastructure Enhancements**: we’ve enhanced the way compute is managed, resulting in faster executions for interactive search sessions.

**Datatyping**: 
* Automatically detect supported data formats.
* Parsers for the **extract** operator now support Regular Expressions and Grok.
* Parsers are now included in the Dataset definition, allowing automatic parsing of recognized data formats.

**Post-process UI for Tables and Charts**: Customize and control how aggregate results are displayed on the results table with new options. You can modify column formatting with prefixes, suffixes, decimal places, and column colors/heatmaps, filter results based on search terms, change plot settings, sort columns, see stats, and easily copy values. With these new features, you can tailor your results to meet your specific needs and better analyze your data.