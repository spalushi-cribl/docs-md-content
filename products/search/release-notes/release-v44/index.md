# Cribl Search 4.4


## New Features

### Query Language

The [join operator](join) merges events from two different data scopes. You can merge data coming from different Dataset Providers, and then process the output for further analysis, gaining insights that would be difficult to obtain otherwise.

The [ip-lookup operator](ip-lookup) retrieves geolocation data of IP addresses using MaxMind's GeoIP2 and GeoLite2 databases to enrich events with details such as city, continent, country, latitude, longitude, postal code, region, and time zone based on the provided IP address field.

The [top-hitters operator](top-hitters) counts distinct value combinations and returns the most frequent combinations in descending order. It provides valuable insights into the most popular and impactful data combinations within your input Dataset.

The [distinct operator](distinct) identifies unique values within each of the specified fields. `distinct` finds unique values within each provided field separately, helping you to analyze and explore data uniqueness within individual fields.

The [let statement](let) allows you to give names to values and expressions, and refer to them in the scope of the same search.

The [match_regex function](match_regex) searches a text string for a specific pattern defined by a regular expression. It returns a Boolean value indicating whether the pattern was found in the text.

Results from the [.show objects command](show-objects) are now filtered by the time range.

### Visualizations

We’ve added a new [Map chart type](chart-map) that displays data points or categories associated with specific locations as geographic maps.

You can now create [interactions](dashboards#add-interactions) that allow Dashboard viewers to drill down into specific chart values within a panel, offering a new dimension of data exploration within your Dashboards.

You can now apply a color threshold to [Single](chart-single) value and [Gauge](chart-gauge) charts.

Dashboards can now be [grouped](managing-sharing-dashboards#collections) into custom collections and [shared](sharing#dashboards) with other users. 

### Search limits

You can now assign users to [Usage Groups](usage-groups) that control limits for each search query. Available limits include, among others, the number of searches a user can run concurrently, maximum time range, and a restriction on result numbers.
