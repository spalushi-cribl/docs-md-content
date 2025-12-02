# Supported Data Formats


Learn about how Cribl Search processes the most common data formats.

---

## List of Supported Data Formats

You can search data in most of the commonly used formats, including text files, binary files, archives, compressed
files, and more.

| Data Format Example               | File Extension Examples                             |
| --------------------------------- | --------------------------------------------------- |
| [Gzip](#gzip)                     | `.gz` `.gzip` `.tgz` `.tar.gz` `application/x-gzip` |
| [Journal](#journals)              | `.journal` `.journal~`                              |
| [LZ4](#lz4)                       | `.lz4`                                              |
| [Snappy](#snappy)                 | `.snappy`                                           |
| [Parquet](#parquet)               | `.parquet` `.pqt` `.parq`                           |
| [Splunk rawdata](#splunk-rawdata) |                                                     |
| [Tar](#tar)                       | `.tar` `.tgz` `.tar.gz`                             |
| [Text](#text)                     | `.log` `.csv` `.json` `.ndjson` `.txt`              |
| [ZIP](#zip)                       | `.zip`                                              |
| [Zstd](#zstd)                     | `.zst`                                              |

## Text Data Formats {#text}

Text files contain human-readable data. They're often used to store logs, but can also contain structured data, in
formats such as JSON or CSV.

You can search text files in most of the commonly used formats, including `.log`, `.json`, `.ndjson`, `csv`, and more.

## Binary Data Formats {#binary-data-formats}

Binary files are non-human-readable files that use formats optimized for storage and processing efficiency. They're
often used to store structured data, such as Parquet files, or system and application events, such as journals.

When you search files in formats that Cribl Search can't recognize, they're processed as binary files.

### Journals {#journals}

Journals are binary files used for logging and storing system and application events.

You can search both `.journal` and temporary `.journal~` files, which are helpful when investigating unexpected process
failures.

### Parquet {#parquet}

Parquet binary files are columnar storage format files that support nested data structures. This makes them an ideal
choice for big data analytics.

You can search Parquet files with the `.parquet`, `.pqt`, and `.parq` extensions.

#### Using Parquet Time Fields {#using-parquet-time-fields}

When processing Parquet files, by default, the Cribl Search `_time` field is assigned the value of [now](now). You can
instead use a time field of the event, by assigning its value to `_time`:

1. Create or open a [Datatype](datatypes) by navigating to **Data** > **Datatypes**.
1. Select **Add rule**.
1. At the bottom, in the **Add fields to events** section, select **Add field**.
1. Assign the **Name** as `_time` and its **Value expression** to the time field in the event. For example, `_time` =
   `time`.
1. Save the Datatype changes.
1. Navigate to the Dataset that specifies your Parquet data and open the **Processing** section.
1. Assign the Datatype to the Dataset and save your changes.

Now, when searching your Parquet Dataset, the time from the event will be used as the reference for the search's time
range.


> ##### Example: Amazon Security Lake
>
> Amazon Security Lake logs data into Parquet format following the schema defined by OCSF. The time is assigned to the
> field `time` and is an epoch time expressed in milliseconds.
>
> To use this field, you need to set its value to the Cribl Search field `_time`. Since Cribl Search uses seconds
> instead of milliseconds, you also need to convert it by dividing by 1,000. In this case, the field you'd add to a
> Datatype would be: `_time` = `time/1000`.
>
{.box .success}

## Compression Data Formats {#compression-data-formats}

Compression formats are used to reduce the size of data to optimize storage space and speed up data transmission.

You can search files compressed with a wide range of algorithms, including LZ4, zstd, gzip, Snappy, and more.

### LZ4 {#lz4}

LZ4 is a popular compression format that offers multiple compression levels and a good balance between compression ratio
and speed.

### Zstd {#zstd}

Zstandard, also known as zstd, is an open-source compression format developed at Facebook. It's often used in scenarios
where high compression speeds are critical.

### Splunk Rawdata {#splunk-rawdata}

Splunk's compressed rawdata files store event data ingested by indexers. They also contain related journal information.

### Gzip {#gzip}

A popular compression format, gzip is often used to compress log files.

In Cribl Search, you can search both `.gz` and `.gzip` files.

### Snappy {#snappy}

Snappy, previously known as Zippy, is an open-source compression format developed at Google, popular for its high
compression speeds.

In Cribl Search, you can search `.snappy` files.

## Archive Data Formats {#archive-data-formats}

Archive formats are used to package multiple files together into a single file.

You can search archives in most of the commonly used formats, including tar, ZIP, and more.

### Tar {#tar}

The tar (tape archive) file format is used by the UNIX-based utility tar to package files together into sets known as
"tarballs".

### ZIP {#zip}

ZIP is a popular archive format that supports lossless data compression. `.zip` files can contain compressed or
non-compressed data, and can use different compression levels.

### Searching Tarballs and ZIP Archives {#searching-tarballs-and-zip-archives}

When searching tarballs or ZIP archives, you can specify which of the archive's files you want to search.

When processing files with `.tar`, `.tgz`, `.tar.gz`, or `.zip` extensions, Cribl Search processes all files inside the
archive and adds a field named `cribl_archive_source` to each file's events. The value of `cribl_archive_source` is the
name of the file inside the archive.

For example, let's say an archive named `archive.tgz` contains files `dir1/file1.log` and `dir2/file2.log`. An event
from `dir1/file1.log` returned from a search would look like this:

```
{
...
source: archive.tgz
cribl_archive_source: dir1/file1.log
...
}
```

Now, to search a specific file inside the `archive.tgz` archive, you can refer to the `cribl_archive_source` field in
your query. For example: `cribl_archive_source=dir1/file1.log`.

## Internal Fields {#fields}

Cribl Search maintains an internal `__raw` field that contains each original event's state after [Event Breakers](datatypes#event-breaker-types) are applied, but before other [Datatype rules](datatypes). (Where an event contains no `_raw` field, Cribl Search constructs one by serializing all the event's top-level and nested fields.)

You can access this field to determine the size of the original event, including its metadata.
