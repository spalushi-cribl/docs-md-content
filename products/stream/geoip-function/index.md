# GeoIP


The GeoIP Function enriches events with geographic fields, given an IP address. It works with [MaxMind's GeoIP](https://dev.maxmind.com/geoip/geoip2/geolite2/) or [IPinfo's IP address](https://ipinfo.io/products/ip-database-download) binary database.

## Prerequisite

You need to host the `.mmdb` database file. The following steps cover this process at a high level. They link to our [Lookup Geographic Data from IP Addresses](/stream/usecase-lookups-ip-addresses)
 topic, where you can find additional details.

1. [Download and extract](/stream/usecase-lookups-ip-addresses#download-and-extract-the-database) the `.mmdb` database file.

1. Determine where to place the database file. We recommend the `$CRIBL_HOME/state/` subdirectory, which is already listed in the default `.gitignore` file that ships with Cribl Stream.

    You always have the option to upload the file to Cribl Stream's [Lookups Library](lookups-library). In a [Cribl.Cloud](/stream/deploy-cloud) deployment, this is currently the only option. For details, see [About Lookups](/stream/lookups-about).

1. Place the database file on your Worker Node(s). In a [distributed deployment](/stream/deploy-distributed), we [recommend](/stream/usecase-lookups-ip-addresses#recommended) having the file on the Leader and all Worker Nodes. Smaller deployments can [get away with](/stream/usecase-lookups-ip-addresses#copy-the-database-file-only-to-the-leader-alternative) hosting the file only on the Leader Node.

1. Optionally, set up automatic updates to the [MaxMind](/stream/usecase-lookups-ip-addresses#automatic-updates-to-the-maxmind-database) and [IPinfo](/stream/usecase-lookups-ip-addresses#automatic-updates-to-the-ipinfo-database) database files.

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description about this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**GeoIP file (.mmdb)**: Path to the database, in binary format, with `.mmdb` extension.

> If the database file is located within the lookup directory (`$CRIBL_HOME/data/lookups/`), the **GeoIP fIle** does not need to be an absolute path.
>
> In [distributed deployments](/stream/deploy-distributed), ensure that the database file is in the same location on the Leader Node and all Worker Nodes. However, if you've uploaded `.mmdb` files via Cribl Stream's [Lookups Library](lookups-library) UI, just select this box to display and select them on a drop-down list. Your selection here will handle the path management automatically.
>
{.box .info}

**IP field**: Field name in which to find an IP to look up. Can be nested. Defaults to `ip`.

**Result field** : Field name in which to store the GeoIP lookup results. Defaults to `geoip`.

## Examples

Assume that you are receiving SMTP logs, and need to see geolocation information associated with IPs using the SMTP service.

Here's a sample of our data, from IPSwitch IMail Server logs:

`03:19 03:22 SMTPD(00180250) [192.168.1.131] connect 74.136.132.88 port 2539
03:19 03:22 SMTPD(00180250) [74.136.132.88] EHLO msnbc.com
03:19 03:22 SMTPD(00180250) [74.136.132.88] MAIL FROM:<info-jjgcdshx@test.us>
03:19 03:22 SMTPD(00180250) [74.136.132.88] RCPT To:<user@domain.com>`

In this example, we’ll chain together three Functions. First, we’ll use a Regex Extract Function to isolate the host’s IP. Next, we’ll use the GeoIP Function to look up the extracted IP against our geoIP database, placing the returned info into a new `__geoip` field. Finally we’ll use an Eval Function to parse that field’s city, state, country, ZIP, latitude, and longitude.

### Function 1 – Regex Extract

Regex: `\[(?<ip>\S+)\]`
Source field: `_raw`
Result: `74.136.132.88`

### Function 2 – GeoIP

Event’s IP field: `ip`
Result field: `__geoip`

### Function 3 – Eval

**Name** | **Value Expression**
--- | ---
`City` | `__geoip.city.names.en`
`Country` | `__geoip.country.names.en`
`Zip` | `__geoip.postal.code`
`Lat` | `__geoip.location.latitude`
`Long` | `__geoip.location.longitude`

In the Eval Function’s **Remove fields** setting, you could specify the `__geoip` field for removal, if desired. However, its `__` prefix makes it an internal field anyway.

> For a hosted tutorial on applying the GeoIP Function, see Cribl's [GeoIP and Threat Feed Enrichment](https://sandbox.cribl.io/course/enrichment) Sandbox.
>
{.box .success}
