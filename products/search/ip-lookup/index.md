# ip-lookup


The `ip-lookup` operator retrieves geolocation data of IP addresses using MaxMind's [GeoIP2 and GeoLite2](https://dev.maxmind.com/geoip) or [IPinfo's IP address](https://ipinfo.io/products/ip-database-download) databases to enrich events with details such as city, continent, country, latitude, longitude, postal code, region, and time zone based on the provided IP address field. You can customize which fields to return, add prefixes, and their language.

> You can display `ip-lookup` results in a [Map Chart](chart-map).
>
{.box .info}

{#prerequisite}
## Prerequisites

The `ip-lookup` operator needs a `.mmdb` database file. You need to download the file from MaxMind or IPinfo and then create a [lookup table](lookups) with it. This file contains the geolocation data needed for IP address enrichment.

* MaxMind's [GeoIP2 City](https://www.maxmind.com/en/geoip2-city), [GeoIP2 Country](https://www.maxmind.com/en/geoip2-country-database), and [GeoLite2](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data) are supported. The GeoIP2 databases are a more accurate version of the free GeoLite2 City database.

* To download the free GeoLite2 City database you need a free GeoLite2 account. For details, see MaxMind's [Accessing GeoLite2 Free Geolocation Data](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data#accessing-geolite2-free-geolocation-data) documentation.

* IPinfo's [IP Geolocation](https://ipinfo.io/products/ip-geolocation-database), [IP to privacy detection](https://ipinfo.io/products/anonymous-ip-database), [ASN](https://ipinfo.io/products/asn-database), [IP to Company](https://ipinfo.io/products/ip-company-database), [Abuse contact](https://ipinfo.io/products/ip-abuse-contact-database), and [WHOIS](https://ipinfo.io/products/whois-database) databases are supported.
 
* IPinfo also provides the following free IP databases: [IP to Country](https://ipinfo.io/developers/ip-to-country-database), [IP to ASN](https://ipinfo.io/developers/ip-to-asn-database), and [IP to Country ASN](https://ipinfo.io/developers/ip-to-country-asn-database).

## Syntax

<div style={{paddingLeft: "2%"}}>
<code>Scope | ip-lookup [ output=OutputField[, ...] ] [ prefix=Prefix ] [ lang=Lang ] LookupTable [ on IPField ]</code>
<br/><br/>
</div>

### Arguments

* **Scope**: The events to search.
* **OutputField**: Field(s) to return from the lookup. By default, all available fields are returned.
    * `city` – for example, `Georgetown`
    * `continent` – for example, `North America`
    * `continent_code` – for example, `NA`
    * `country` – for example, `United States`
    * `country_code` – for example, `US`
    * `lat` – numeric latitude coordinate
    * `lon` – numeric longitude coordinate
    * `postal` – for example, `40324`
    * `region` – for example, `Kentucky`
    * `region_code` – for example, `KY`
    * `time_zone` – for example, `America/Los_Angeles`
* **Prefix**: A prefix to add to the output fields. For example, if the prefix is set to `ip_`, output fields will be named `ip_city` `ip_country`, etc.
* **Lang**: Specifies the language for data retrieval. Defaults to `en` for English. Supports Brazilian Portuguese (pt-BR), English (en), French (fr), German (de), Japanese (ja), Russian (ru), Simplified Chinese (zh-CN), and Spanish (es).
* **LookupTable**: The [lookup](lookups) filename that contains the database. Do not add the `.mmdb` file extension. For example, a filename of `GeoLite2-city.mmdb` is entered as `GeoLite2-city`.
* **IPField**: The field name with an IP address. Defaults to `ip`.

## Examples

The runnable examples below require you to first acquire and upload a compatible `.mmdb` file, as outlined above in [Prerequisites](#prerequisites). 

* Lookup geolocation data on the `dstaddr` field:

  ```kusto {runSearch=true}
  dataset="cribl_search_sample" dataSource=*vpc* | limit 1000  | ip-lookup geocity on dstaddr
  ```

* Lookup latitude and longitude data on the `dstaddr` field:

  ```kusto {runSearch=true}
  dataset="cribl_search_sample" dataSource=*vpc* | limit 1000  | ip-lookup output=lat,lon geocity on dstaddr
  ```

* Lookup geolocation data on the `ip` field:
  ```kusto
  dataset=myDataset
  | ip-lookup 'GeoLite2-City'
  ```
* Lookup geolocation data on the `my_ip` field:
  ```kusto
  dataset=myDataset
  | ip-lookup 'GeoLite2-City' on my_ip
  ```
* Lookup geolocation data on the `ip_address` field and return only the prefixed `ip_lat` and `ip_lon` fields:
  ```kusto
  dataset=myDataset
  | ip-lookup output=lat,lon prefix=ip_ 'GeoLite2-City' on ip_address
  ```
* Lookup geolocation data on the `ip_address` field and return only the `city` and `region` fields in Spanish:
  ```kusto
  dataset=myDataset
  | ip-lookup output=city,region lang=es 'GeoIP2-City' on ip_address
  ```
