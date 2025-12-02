# Windows Event Logs Source


Cribl Edge supports collecting local Windows Event Logs in batches. You can collect the standard event logs – Application, Security, and System – and any other event logs on the machine.

For guidance on workflow, see our better practices doc: [Windows Observability Using Cribl Edge](usecase-windows-observability).

> Type: **Pull**  |  TLS Support: **N/A** | Event Breaker Support: **No** | Uses Key-Value Store
> 
> Cribl Edge Workers support Windows Event Logs only when running on Windows, not on Linux.
>
{.box .info}

## Configuring Cribl Edge to Collect Windows Event Logs

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

### Configure via [QuickConnect](quickconnect)

1. Click **Collect**, and then click **Add Source**. 
1. Select **Windows Event Logs** from the list of tiles. 
1. Click either **Add New** or (if displayed) **Select Existing**. 

The **General Settings** drawer will open.
 
### Configure via [Routing](routes) 

1. Click **More** and select **Sources**. 
1. Select **Windows Event Logs** from the list of tiles or the **Sources** left nav.
1. Click **Add Source** to open the **New Source** modal.

#### General Settings

**Input ID**: Enter a unique name to identify this Source definition. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.

**Event logs**: Enter one or more event logs to collect. `Security` is prefilled
as a default. Click **Add log** to specify more event logs (e.g., `Application`
or `System`). 

- This Source can collect events from the Forwarded Events log when **Event
  format** is set to **XML**. 

- For more information about how to locate Windows event logs on a Windows
  server, see [Locate Windows Events Logs in the Server](usecase-windows-observability#locate-windows-event-logs-in-the-server).

#### Optional Settings

**Read mode**: Select **Entire Log** (the default) to read all of the historical
events and new events. Select **From last entry** to read only new events.

**Event format**: Select JSON or XML as the event format. JSON is the default.
You can change this setting at any time.

  - It may be faster to render events as XML.
  - JSON format includes the rendered message string, while XML does not.

    Sample JSON

  ```JSON
  {
    "_raw": "
    {\"Id\":1234,\"Version\":0,\"Qualifiers\":null,\"Level\":0,\"Task\":9999,\"Opcode\":0,\"Keywords\":-999999999999999,\"RecordId\":999999,\"ProviderName\":\"Microsoft-Windows-Security-Auditing\",\"ProviderId\":\"GUID-12345678-1234-1234-1234-123456789012\",\"LogName\":\"Security\",\"ProcessId\":123,\"ThreadId\":5678,\"MachineName\":\"EC2-HOST\",\"UserId\":null,\"TimeCreated\":\"\\/Date(1000000000000)\\/\",\"ActivityId\":\"ACTIVITY-12345678-1234-1234-1234-123456789012\",\"RelatedActivityId\":null,\"ContainerLog\":\"Security\",\"MatchedQueryIds\":[],\"Bookmark\":{},\"LevelDisplayName\":\"Information\",\"OpcodeDisplayName\":\"Info\",\"TaskDisplayName\":\"User Account Management\",\"KeywordsDisplayNames\":[\"Audit Success\"],\"Properties\":[{\"Value\":\"Administrator\"},{\"Value\":\"EC2-HOST\"},{\"Value\":\"SID-12345678\"},{\"Value\":\"SID-87654321\"},{\"Value\":\"Administrator\"},{\"Value\":\"EC2-HOST\"},{\"Value\":9999999},{\"Value\":9999},{\"Value\":\"C:\\\\Program Files\\\\generic.exe\"}],\"Message\":\"A message about user and process information.\"}",
    "source": "Security",
    "host": "EC2-HOST",
    "_time": "1000000000.000",
    "cribl_breaker": "windows event logs (json)"
  } 
  ```      

  Sample XML
    
  ```XML
   {
    "_raw": "<Event xmlns='[http://schemas.microsoft.com/win/2004/08/events/event](http://schemas.microsoft.com/win/2004/08/events/event)'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{GUID-12345678-1234-1234-1234-123456789012}'/><EventID>1234</EventID><Version>0</Version><Level>0</Level><Task>12345</Task><Opcode>0</Opcode><Keywords>0x8020000000000000</Keywords><TimeCreated SystemTime='2000-01-01T00:00:00.0000000Z'/><EventRecordID>123456</EventRecordID><Correlation ActivityID='{12345678-1234-1234-1234-123456789012}'/><Execution ProcessID='1234' ThreadID='5678'/><Channel>Security</Channel><Computer>GENERIC-HOST</Computer><Security/></System><EventData><Data Name='TargetUserName'>GenericUser</Data><Data Name='TargetDomainName'>GenericDomain</Data><Data Name='TargetSid'>SID-12345678</Data><Data Name='SubjectUserSid'>SID-87654321</Data><Data Name='SubjectUserName'>Administrator</Data><Data Name='SubjectDomainName'>EC2-Host</Data><Data Name='SubjectLogonId'>0x123456</Data><Data Name='CallerProcessId'>0x7890</Data><Data Name='CallerProcessName'>C:\\Program Files\\PowerShell\\7\\pwsh.exe</Data></EventData></Event>",
    "source": "Security",
    "host": "EC2-Host",
    "_time": "1000000000.000",
    "cribl_breaker": "windows event logs (xml)"
   }
  ```  
**Polling interval**: Specify how often, in seconds, to check for new event
logs. Defaults to `10` seconds, and must be at least `1` second. Polling will
read each log, up to the **Batch Size**, in their specified order. This Source
will not make a polling request if it’s still reading events from the previous
request.

**Batch size**: Set the maximum number of events to read per polling request. By
default, each request reads up to `500` events. Set as high as you need to;
however, if you configure the Source to pull from multiple event logs, be aware
that setting **Batch Size** higher can keep one log waiting longer while the
Source collects a batch from another log.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Max event bytes**: The maximum number of bytes that can be in an event before it is flushed to the pipelines. Defaults to `51200`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

**Send to Routes**: Enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

**QuickConnect**: Send this Source’s data to one or more Destinations via independent, direct connections.

> This Source defaults to [QuickConnect](quickconnect).  
>
{.box .info}

## Key-Value Store

The Windows Event Logs Source uses key-value store for cursors for each Windows Event Log.
Log names are used as keys.

## Troubleshooting

If you are running this Source to collect events on a Windows Node without admin access, change the permission on the registry entry for the `EventLog` key: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog` to `Read.` 