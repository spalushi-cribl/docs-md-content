# Cribl Search 4.6.1


Starting with Cribl Search 4.6.1, you can accelerate your Datasets, write inline subqueries, and create more elaborate
Macros.

### Dataset Acceleration

You can now significantly speed up the largest of your object-store Datasets, using
[Dataset Acceleration](acceleration). When enabled, Cribl Search periodically prescans selected portions of your data to
optimize future search performance.

The following data provider types are supported:

- [Amazon S3](set-up-s3)
- [Azure Blob Storage](set-up-azure-blob)
- [Google Cloud Storage](set-up-google-cloud-storage)

### Inline Subqueries

You can now perform [unions](union) and [joins](join) without using [`let`](let) statements, thanks to
[inline subqueries](common-examples#inline-subqueries):

```kusto
// instead of this:
let subquery = dataset=$vt_dummy event<10 | extend bar=24;
dataset=$vt_dummy event<10 | extend foo=42 | union subquery

// you can do this:
dataset=$vt_dummy event<10 | extend foo=42 | union (
  search in($vt_dummy) event<10 | extend bar=24
);
```

### Macro Parameters

You can now enhance your Macros with [parameters](macros#macro-parameters). They work similarly to Bash positional
parameters:

```kusto
// define in a Macro:
action=$1 answer=$2 $3

// use in a query:
${filterByActionAndAnswer, 'reject', 42, ips="172.16.*"}
```
