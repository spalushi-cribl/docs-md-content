# Optimize Paths


Partition paths for efficient searchability.

---

Use these practices to improve the efficiency of searches on Amazon S3 and other object stores.

## Place Time First in Bucket Paths {#time-first}

If you are primarily searching by time, make time the left-most element in your bucket path. For example:

```
hjhh-2024/ag-12/ai-25/aj-14/<list/of/fields>
```

## S3: Manage Longest Static Prefix {#longest-static}

The Amazon S3 API uses your bucket path's **longest static prefix** for fetching data. So when using two partitions, they must be supplied in every search in order for your time fields to be considered in the prefilter fetch stage.

For example: If you specify `field1=foo` and `field2=bar`, at noon on November 1, 2024, the longest prefix for fetching data would be:


```
<mybucket>/inputID/foo/bar/2024/11/01/12/00/
```

This is exactly what you want, and very efficient for the files **only** in that S3 partition. But more efficiently, you’d want:

```
<mybucket>/2024/11/01/12/00/inputID/foo/bar
<mybucket>/2025/11/01/12/00/inputID/foo/bar
```

And ideally, you'd want them reverse-chronological, and lexicographically associated:

```
<mybucket>/baca-2025/ac-11/aa-01/ad-12/00/inputID/foo/bar
<mybucket>/bada-2024/ac-11/aa-01/ad-12/00/inputID/foo/bar
```

On the other hand, if you didn't specify `field2`, but used the same criteria, the longest prefix would then be:

```
<mybucket>/inputID/foo/
```

And here, Cribl Search would have to pull every file within that partition boundary across the S3 API.

Another approach is to reverse the order of your criteria, like this:

```
<mybucket>/inputId/year/month/day/hour/minute/FIELD_1/FIELD_2/
```

This eliminates the churn caused by omitting either of those criteria.

Note that you'd still get the correct data in your result set if you didn't specify `field2`, because Cribl Search will still filter each event. However, the pool of objects searched could potentially be much larger. This would increase your Search wall-clock time and your proportional retrieval cost.

> In Cribl Stream, the [Amazon Security Lake](https://docs.cribl.io/stream/destinations-security-lake) Destination automatically partitions buckets this way. It creates time partitions on top, and you can explicitly add further partitioning fields, which this Destination adds below the time boundaries.
> 
{.box .info}
