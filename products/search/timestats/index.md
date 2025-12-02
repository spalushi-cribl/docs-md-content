# timestats



# ![](page/agg-icon.svg) timestats

The `timestats` operator aggregates events by time periods or bins.

## Syntax

<div style={{paddingLeft: "2%"}}>
<code>[... |] timestats [ span=Time[ SnapToTime ] | numBins=Bins ] [ timeSource=TimeSource ] [ timeLabel=TimeLabel ] [[ FieldName= ] Aggregation... ] [ by GroupExpression [, GroupExpression ]... ]</code>
<br/><br/>
</div>

You can define either `span` or `numBins`, not both. If you define neither, this operator will automatically compute a suitable time span based on the search's time range.

### Arguments

* **Time**: Time period. Supports these relative times – `s[econds]`, `m[inutes]`, `h[ours]`, `d[ays]`, `w[eeks]`, `mon[ths]`, and `y[ears]`. Values without units get interpreted as seconds. For example, `1` = `1s`.
* **SnapToTime**: Round down (backward) to the nearest instance of **Time**. Append the `@` modifier, followed by the same or more granular `Time`. For example,
  * `1d@d` snaps back to the beginning of today, 12:00 AM (midnight) UTC.
  * `1d@h` snaps to 4:00 PM the next day if it's currently 4:00 PM.
* **Bins**: Numeric literal, desired number of bins. Bins are split as close as possible based on the search's time range.
* **TimeSource**: String. Name of the time field to use for aggregation. Defaults to `_time`. The field needs to have time in seconds.
* **TimeLabel**: String. Name of the output time field. Defaults to the same field as **TimeSource**.
* **FieldName**: String. Name to give to the field with the results created by **Aggregation**.
* **Aggregation**: [Cribl](cribl-functions) and [statistical](statistical-functions) functions. Wildcards are not supported for field names in aggregation functions.
* **GroupExpression**: A scalar expression that can reference the input data. The output will have as many events as there are distinct values of all the group expressions.

## Examples

* Create two fields per host in the format `Host:totalWaitTime` and `Host:avgResponseTime`, showing the aggregates for the time-bins (rows) of one minute:

    ```kusto
    timestats span=1m totalWaitTime=sum(responseTime), avgResponseTime=avg(responseTime) by host
    ```

* Create roughly 42 time bins, labeled as `when`, and create output fields in the form `srcaddr:totalRequest` and `dstaddr:totalRequest`:

  ```kusto {runSearch=true}
  dataset="cribl_search_sample"
  | timestats timeLabel="when" numBins=42 totalRequest=count() by srcaddr, dstaddr
  ```



* Aggregate time durations by interface, bin every minute:

  ```kusto {runSearch=true}
  dataset="cribl_search_sample" 
  | timestats span=1m totalTime=sum(end-start) by interface
  ```

* Count events over time, bin every minute:

  ```kusto {runSearch=true}
  dataset=$vt_dummy event<600 
  | extend _time=_time-rand(600) 
  | timestats span=1m count()
  ```

* Count events over time, using the `timeSource` option:

  ```kusto {runSearch=true}
  dataset=$vt_dummy event<600 
  | extend rando=_time-rand(600) 
  | timestats span=1m timeSource=rando count()
  ```
  
* Count events over time, split by the field `method`, bin every minute:

  ```kusto {runSearch=true}
  dataset=$vt_dummy event<600 
  | extend _time=_time-rand(600), method=iif(event%2>0, "GET", "POST") 
  | timestats span=1m count() by method
  ```
