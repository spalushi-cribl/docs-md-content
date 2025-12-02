# Zscaler NSS Integration


Zscaler [Nanolog Streaming Service](https://help.zscaler.com/zia/about-nanolog-streaming-service) (NSS) uses a VM to stream traffic logs in real time from the Zscaler Nanolog to a SIEM. Cribl Stream can take the place of the SIEM in this arrangement. Then you can use Cribl Stream to greatly reduce the size of ZScaler logs.

## Configuring NSS to Send Data to Cribl Stream

Since Nanolog forwards data to a single IP address or FQDN, Cribl recommends that you use a load balancer to distribute data among Cribl Stream Workers.

Nanolog delivers data using a raw TCP connection.



In Zscaler: 
* Go to **Administration** > **Nanolog Streaming Service**.
* In the **NSS Feeds** tab, click **Add NSS Feed** to open the following configuration window:

![Adding a Zscaler NSS feed](st-zscaler-01.5e3b5af542.png)

* Enter a **Feed Name** that identifies this feed as one that sends data to Cribl Stream.
* Enter the IP address or FQDN for either your Cribl Stream instance, or the load balancer you're using with your Cribl Stream instances.
* Select a **Feed Output Type**. `Splunk CIM`, a tab-delimited key/value format, is a typical choice.

Alternatively, you choose a different option, such as CSV:

![Choosing a CSV output format](st-zscaler-02.6f72b5f8d4.png)

## Example Pipeline

Cribl Stream can reduce Zscaler log size by (1) reformatting and reshaping the data, and (2) suppressing, sampling, and dropping appropriate fields.

The following code block shows how to correctly parse tab-delimited key-value pairs.

```js
let temp = {};

// Substr drops the timestamp from _raw, otherwise the split does not work correctly
__e['_raw'].substr(20).split('\t').forEach((element) => {
    // Split K=V on the first equal sign
    let eq = element.indexOf('=')
    let name = element.substr(0, eq);
    let value = element.substr(eq + 1);

    // if value is none or N/A, drop the field
    value !== 'None' && value !== 'NA' ? temp[name] = value : false;
    // otherwise use this line below
    // temp[name] = value;
})

__e['_raw'] = temp;
```

Here's an example Pipeline that uses the parsing code above. (You can directly [import](#pipeline-json) this Pipeline in JSON form.)

A [Code Function](code-function) parses the data:

![Parsing with a Code Function](st-zscaler-03.65811ed67c.png)
{border="true"}

An [Eval Function](eval-function) reshapes the data:

![Reshaping with Eval](st-zscaler-04.267325cf63.png)
{border="true"}

And finally, a [Serialize Function](serialize-function) drops unwanted fields:

![Dropping fields with Serialize](st-zscaler-05.9c98856aa3.png)
{border="true"}

### Example Pipeline JSON {#pipeline-json}

To import the example Pipeline directly, copy and save the JSON below, then follow [these](pipelines#json) instructions.

```json {title="Zscaler Example Pipeline"}
{
  "id": "zscaler",
  "conf": {
    "output": "default",
    "groups": {},
    "asyncFuncTimeout": 1000,
    "functions": [
      {
        "id": "code",
        "filter": "true",
        "disabled": false,
        "conf": {
          "maxNumOfIterations": 5000,
          "code": "let temp = {};\n\n// Substr drops the timestamp from _raw, otherwise the split does not work correctly\n__e['_raw'].substr(20).split('\\t').forEach((element) => {\n    // Split K=V on the first equal sign\n    let eq = element.indexOf('=')\n    let name = element.substr(0, eq);\n    let value = element.substr(eq + 1);\n\n    // if value is none or N/A, drop the field\n    value !== 'None' && value !== 'NA' ? temp[name] = value : false;\n    // otherwise use this line below\n    // temp[name] = value;\n})\n\n__e['_raw'] = temp;"
        }
      },
      {
        "id": "eval",
        "filter": "true",
        "disabled": false,
        "conf": {
          "add": [
            {
              "name": "_raw.hostname",
              "value": "_raw.url.startsWith(_raw.hostname) ? undefined : _raw.hostname"
            },
            {
              "name": "_raw.reason",
              "value": "_raw.reason === _raw.action ? undefined : _raw.reason"
            },
            {
              "name": "_raw.bwthrottle",
              "value": "_raw.bwthrottle === 'NO' ? undefined : _raw.bwthrottle"
            }
          ]
        }
      },
      {
        "id": "serialize",
        "filter": "true",
        "disabled": false,
        "conf": {
          "type": "kvp",
          "fields": [
            "!vendor",
            "!product",
            "!useragent",
            "!location",
            "!responsesize",
            "!requestsize",
            "!event_id",
            "!*transtime",
            "!transactionsize",
            "*"
          ],
          "dstField": "_raw",
          "cleanFields": false,
          "srcField": "_raw"
        }
      }
    ]
  }
}
```
