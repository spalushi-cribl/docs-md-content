# Lookup Geographic Data from IP Addresses


In many observability and security use cases, raw event data includes an IP address, but not much context about where that IP originated from. By performing lookups with geographic metadata, you can enrich data events with human-readable fields such as country, region, city, or network ownership. These fields could help you route or filter data or normalize data for a downstream alerting or reporting tool.

Cribl Stream supports geographic enrichment using the [GeoIP Function](geoip-function), which adds location-related fields to events based on an IP address. The GeoIP Function works with binary IP address databases from either [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/) (GeoLite2 or GeoIP2 `.mmdb` files) or [IPinfo](https://ipinfo.io/products/ip-database-download).

Enriching events with geographic data lookups unlocks a wide range of operational and security workflows. For example:

- **Add geographic context to events using IP addresses:** Use IP-to-location data to convert raw IP addresses into meaningful fields like `geo_country`, `geo_city`, `geo_latitude`, and `geo_org`. This added context allows you to group, filter, or analyze events based on location. For example, enriching `client_ip="192.0.2.0"` could yield `geo_country="US"` and `geo_city="Chicago"`, helping you detect geographic anomalies or route the event to a specific Destination.
- **Detect suspicious or unexpected access patterns:** Identify events coming from countries or regions that fall outside expected patterns for a given service or customer base, such as sudden spikes in traffic from high-risk countries or VPN exit nodes.
- **Support compliance and data sovereignty policies:** Tag or route events differently based on geographic origin to comply with regulations like GDPR or regional data residency requirements.
- **Enable geo-based reporting and dashboards:** Aggregate and visualize metrics by country, region, or city to gain insight into user distribution, application access, or network flow patterns.
- **Correlate with external threat intelligence or allowlists:** Use geographic fields to compare against known malicious IP ranges or to filter out traffic from unapproved regions.

> See [About Lookups](lookups-about) for information about how to generally use lookups in Cribl Stream.
>
{.box .info}

## How to Use MaxMind Geographic Databases with Cribl Stream

This section explains how to configure geographic lookups using [MaxMind](https://dev.maxmind.com/geoip/geoip2/geolite2/).

### Step One: Download and Extract the MaxMind Database

We'll illustrate this with an example that often combines all three conditions: setting up the free, popular [MaxMind GeoLite2 City](https://dev.maxmind.com/geoip/geoip2/geolite2/) database to support Cribl Stream's [GeoIP](geoip-function) Function. This example anticipates a Cribl Stream production [distributed deployment](deploy-distributed), where the GeoLite database is updated nightly across multiple Workers.

This example includes complete instructions for this particular setup. However, you can generalize the example to other MaxMind databases, and to other large lookup files – including large `.csv`'s that similarly receive frequent updates.

To enable the GeoIP Function using the MaxMind GeoLite 2 City database, your first steps are:

1. Create a free MaxMind account, at the page linked above.

2. Log in to your MaxMind [account portal](https://www.maxmind.com/en/account/login) and select **Download Databases**.

3. On the Download page, look for the database you want. (In this example, you'd locate the **GeoLite2 City** section.) Note the **Format: GeoIP2 Binary**, and select **Download GZIP**.

![GeoLite2 City database: Download binary GZIP](geolite2-city-download-iframe.ba3ebccd13.png)
{border="true"}

4. Extract the archive to your local system.

5. Change to the directory created when you extracted the archive. This directory's name will correspond to the date you downloaded the file, so in the above `2020-10-06` example, you would use: `$ cd GeoLite2-City_20201006`

### Step Two: Download and Extract the IPinfo Database

IPinfo's [IP Geolocation](https://ipinfo.io/products/ip-geolocation-database), [IP to privacy detection](https://ipinfo.io/products/anonymous-ip-database), [ASN](https://ipinfo.io/products/asn-database), [IP to Company](https://ipinfo.io/products/ip-company-database), [Abuse contact](https://ipinfo.io/products/ip-abuse-contact-database), and [WHOIS](https://ipinfo.io/products/whois-database) databases are supported. IPinfo also provides the following free IP databases: [IP to Country](https://ipinfo.io/developers/ip-to-country-database), [IP to ASN](https://ipinfo.io/developers/ip-to-asn-database), and [IP to Country ASN](https://ipinfo.io/developers/ip-to-country-asn-database) databases.

To enable the [GeoIP](geoip-function) Function using the IPinfo database:

1. Download the MMDB format database from your [IPinfo account dashboard](https://ipinfo.io/account/data-downloads).
1. Extract the archive to your local system.
1. Change to the directory created when you extracted the archive.

### Step Three: Copy the Database File to the Leader and Worker Nodes

Use the UI or the API to add the database file to each Worker in the Worker Group where you intend to use the GeoIP Function. You can only upload Maxmind database files as an [in-memory lookup file type](lookups-about#storage-mode). See [Configure Lookups](lookups-configure) for information about uploading and updating lookup files.

### Step Four: Update the Database File

MaxMind updates its database files regularly, so consult the MaxMind documentation for update schedules.

Use the [GeoIP Update](https://github.com/maxmind/geoipupdate) program MaxMind provides to automatically update your databases. See MaxMind's [Automatic Updates for GeoIP2 and GeoIP Legacy Databases](https://dev.maxmind.com/geoip/geoipupdate/) documentation for details.

You'll need two modifications specific to Cribl Stream:

- GeoIP Update must be set up on the Leader, and on each Worker in any Group that uses GeoIP lookups.

- You need to configure GeoIP Update to point to the directory where the MaxMind database file resides. The program uses a configuration file named `GeoIP.conf` and you utilize the `DatabaseDirectory` parameter to define the location for storing the database file. See MaxMind's [GeoIP.conf](https://github.com/maxmind/geoipupdate/blob/main/doc/GeoIP.conf.md) documentation for details.

    For example, the default setting in `GeoIP.conf` writes output to `/usr/local/share/GeoIP`. You must change this setting to the path where your databases actually reside. If you're using the [recommended architecture](#recommended) above, you'd set: `DatabaseDirectory <$CRIBL_HOME>/state/`.

> A Cribl Worker restart is necessary for the new data to be loaded. Even though the update happens automatically, the cached version loaded at the Worker's startup persists until the Worker is restarted. To ensure that updated MaxMind databases are used for processing, schedule regular Worker restarts to coincide with the automatic updates. This applies to all Cribl Stream deployments, both on-prem and Cribl.Cloud.
>
{.box .warning}

## Upload and Update IPinfo Databases

You can download the IPinfo database through its [storage URI](https://ipinfo.io/developers/database-filename-reference). The storage system simply accepts the IPinfo authentication token and the database filename to allow you to download your IP database.

For example, to download the [IPinfo’s Free IP to Country ASN](https://ipinfo.io/developers/ip-to-country-asn-database) database, the URI/API command is as follows:

`curl -L <https://ipinfo.io/data/free/country_asn.mmdb?token=><YOUR_TOKEN> -o country_asn.mmdb`

You can also use Wget or any tool you prefer. You can also stream the output to a file.
