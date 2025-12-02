# Reducing Windows XML Events


Here, we demonstrate how to use just a few Cribl Stream Functions to parse WindowsXML events and reduce their volume by 34–70%, dramatically reducing your downstream infrastructure requirements.

## Using Eval and C.Text.parseWinEvent

Cribl Stream's internal [C.Text.parseWinEvent](cribl-reference#parse-win-xml) method parses Windows XML strings and returns a prettified JSON object. You can use this function within an [Eval](eval-function) Function to parse an event or an ad hoc XML string. It works like [C.Text.parseXml](cribl-reference#parse-xml), but with Windows events, it produces more-compact output.

As you can see from its signature, `C.Text.parseWinEvent` accepts an optional `nonValues` parameter that can further reduce an event's size by discarding characters that you specify as redundant.

> (method) `C.Text.parseWinEvent(xml: string, nonValues?: string[]): any`
>
> @param – `xml` – an XML string; or an event field containing the XML.
>
> @param – `nonValues` – array of string values. Elements whose value equals any of the values in this array will be omitted from the returned object. Defaults to `['-']`, meaning that elements whose value equals `-` will be discarded.
>
> @returns – an object representing the parsed Windows Event; or `undefined` if the input could not be parsed.
>
{.box .info}

## XML: Threat or Menace?

When working with XML, an anonymous [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/1mbgih/xml/) user's quote sums up the challenge: "Some languages can be read by humans, but not by machines, while others can be read by machines but not by humans. XML solves this problem by being readable to neither." An example of a Windows XML event only reinforces this quote:

![](j7XbO5Y.8a1e12a60c.png)
{border="true"}

This Windows XML `_raw` event is 1.36KB in size:

![](fOgkzdv.76e57ed7d3.png)
{border="true"}

## Eval to the Rescue

In our initial Eval Function below, the **Value Expression**  uses `C.Text.parseWinEvent` to simply parse the `_raw` Windows XML event and turn it into a prettified JSON object:

![](MVaV8Kx.73e1f3d678.png)
{border="true"}

The resulting JSON event is now down to 921.00B in size, a 34.07% reduction of the event:

![](dYW4DLQ.107f803b97.png)
{border="true"}

> The `C.Text.parseWinEvent(_raw,[])` call yields verbose output that includes XML attributes. If you want flatter output, you can substitute `C.Text.parseXml(_raw, false)`.
>
{.box .info}

## Removing Unnecessary Fields with a Better Eval

But we can do better. The fields containing essentially null values (`'0','0x0',` or `'‑'`) bloat events, demanding extra infrastructure and storage:

![](cdKZZvX.eaadc35ab5.png)
{border="true"}

Let's amplify the reduction by removing all of the fields whose values are in the set: `['0','0x0','-']`. This improved version of our Eval Function parses the Windows XML event, and discards `['0','0x0','-']` values. (Its preceding row also tidies up events by removing tabs and curly braces, and replacing newlines and returns with commas.) The result is an even **smaller** prettified JSON object.

If you compare this Preview-pane screenshot to the Preview screenshot above, you can confirm that the fields with values matching `[‘0’,‘0x0’,’-’] `are removed:

![](ASddcDM.862d539009.png)
{border="true"}

The event is now down to 678.00B in size, translating to a 51.47% reduction from the original event:



![](eval-win-xml-results.71c9fafaa4.png)
{border="true"}

## Flatten Function

Cribl Stream's [Flatten](flatten-function) Function is designed to flatten fields out of a nested structure. Let's flatten the JSON object within `_raw`, to see if we can further reduce the event's size before we send it to our preferred destinations.

Using Flatten, we've successfully created top-level fields from the nested JSON structure:

![](nVDEwbX.645bd5422e.png)
{border="true"}

> Don't worry about the `_raw` field's deletion (red strikeout). This is the Flatten Function's default behavior. We'll restore `_raw` after we clean and reduce the event even more.
>
{.box .success}

The flattened field names are extracted from `_raw`  and delimited with `_`. These field names are quite long. We can optimize them using the Rename Function.

## Rename Function

[Rename](rename-function) is designed to change fields' names, or to reformat their names (e.g., to normalize names to camelcase). You can use Rename to change specified fields (much like Eval), or to accomplish bulk renaming based on a JavaScript expression (much like the [Parser](parser-function) Function). But Rename offers a streamlined way to alter only field names, without other effects.

Let's use Rename to remove any unnecessary prefixes from the field names, to further shrink our events. In the **Rename Expression**, we build a JavaScript expression to match the field names' prefixes (up to the underscore):

![](Q49vv67.f4a8eefdbd.png)
{border="true"}

The resulting field names are now much more compact, and easier to work with and manage:


![](tvjhUGG.49af9cfbf8.png)
{border="true"}

## Serialize Function

We started with bloated Windows XML data, and we've parsed and prettified it into JSON. Next, we'll extract key-value pairs. We'll use the [Serialize](serialize-function) Function, which serializes an event's content into a predefined format.

We set Serialize to change the **Type** to key-value pairs. (The Function's other supported target Types include JSON Object and CSV.) Serialize takes the extracted fields and puts them back into `_raw`.

In the Preview pane, the `_raw` field is now back, serialized into compact, tidy key-value pairs:

![](CI7dZXu.ec083455a1.png)
{border="true"}

The last step is to remove any of the extracted fields you don't need before sending events to your destinations. We'll again call on the Eval Function, which adds or removes fields in events. (For a Splunk destination, these are index-time fields.) This final Eval Function looks like this:

![](dtG5wut.5325c4bb83.png)
{border="true"}

To sum up, we've successfully transformed the original Windows XML event into key-value pairs:

![](2kI2HlF.38e2ef1695.png)
{border="true"}

And we've dramatically reduced the event's size, while retaining all of the necessary fields. The event is now down to 513.00B in size, translating to a 63.28% reduction from the original Windows XML:

![](HuHytQy.c724e818e6.png)
{border="true"}

## Try This at Home

Below is an export of the whole Cribl Stream Pipeline presented here. [Import](pipelines#json) this JSON to experiment with it and modify it to match your own needs:

```json {title="Win XML Pipeline"}
{
  "id": "Windows_Security_Events",
  "conf": {
    "output": "default",
    "groups": {},
    "asyncFuncTimeout": 1000,
    "functions": [
      {
        "filter": "true",
        "conf": {
          "comment": "This Cribl Stream Pipeline reduces Microsoft Windows XML events, retains full fidelity of the data, and dramatically reduces license and storage costs by up to 70%.\n\nAuthor: David Maislin - (david@cribl.io) - Find me on our Community Slack"
        },
        "id": "comment"
      },
      {
        "filter": "true",
        "conf": {
          "add": [
            {
              "name": "_raw",
              "value": "_raw.replace(/[{}\\t]/gm,'').replace(/[\\n\\r]+/gm,',')"
            },
            {
              "name": "_raw",
              "value": "C.Text.parseWinEvent(_raw,['0x0','0','-'])"
            }
          ]
        },
        "id": "eval",
        "disabled": false,
        "description": "Remove tabs & curly braces; replace newlines & returns with commas. Parse the Windows XML event, removing null or unnecessary fields & whitespace."
      },
      {
        "filter": "true",
        "conf": {
          "fields": [
            "_raw"
          ],
          "prefix": "",
          "depth": 5,
          "delimiter": "_"
        },
        "id": "flatten",
        "disabled": false,
        "description": "Flatten the object into key value fields"
      },
      {
        "filter": "true",
        "conf": {
          "baseFields": [],
          "renameExpr": "name.replace(/_raw_Event_\\w+_/,'')",
          "rename": []
        },
        "id": "rename",
        "disabled": false,
        "description": "Rename the top level fields and remove the log suffix"
      },
      {
        "filter": "true",
        "conf": {
          "type": "kvp",
          "fields": [
            "!_*",
            "!cribl_*",
            "!index",
            "!host",
            "!source",
            "!sourcetype",
            "*"
          ],
          "dstField": "_raw",
          "cleanFields": false
        },
        "id": "serialize",
        "disabled": false,
        "description": "Serialize top level events back into _raw in the desired type."
      },
      {
        "filter": "true",
        "conf": {
          "keep": [
            "_time",
            "_raw"
          ],
          "remove": [
            "*"
          ]
        },
        "id": "eval",
        "disabled": false,
        "description": "Drop unnecessary fields."
      }
    ]
  }
}
```

Finally, here's a sample of Windows XML events that you can upload to Cribl Stream's [Data Preview](data-preview) pane to try this out:

```xml {title="Sample data"}
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{54849625-5478-4994-A5BA-3E3B0328C30D}'/><EventID>4625</EventID><Version>0</Version><Level>0</Level><Task>12544</Task><Opcode>0</Opcode><Keywords>0x8010000000000000</Keywords><TimeCreated SystemTime='2020-11-12T17:27:33.608596000Z'/><EventRecordID>1598720</EventRecordID><Correlation/><Execution ProcessID='740' ThreadID='948'/><Channel>Security</Channel><Computer>EC2AMAZ-CPMK6J5.cribl.poc</Computer><Security/></System><EventData><Data Name='SubjectUserSid'>S-1-0-0</Data><Data Name='SubjectUserName'>-</Data><Data Name='SubjectDomainName'>-</Data><Data Name='SubjectLogonId'>0x0</Data><Data Name='TargetUserSid'>S-1-0-0</Data><Data Name='TargetUserName'>ADMINISTRATOR</Data><Data Name='TargetDomainName'></Data><Data Name='Status'>0xc000006d</Data><Data Name='FailureReason'>%%2313</Data><Data Name='SubStatus'>0xc000006a</Data><Data Name='LogonType'>3</Data><Data Name='LogonProcessName'>NtLmSsp </Data><Data Name='AuthenticationPackageName'>NTLM</Data><Data Name='WorkstationName'>-</Data><Data Name='TransmittedServices'>-</Data><Data Name='LmPackageName'>-</Data><Data Name='KeyLength'>0</Data><Data Name='ProcessId'>0x0</Data><Data Name='ProcessName'>-</Data><Data Name='IpAddress'>64.203.230.138</Data><Data Name='IpPort'>0</Data></EventData></Event>
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{54849625-5478-4994-A5BA-3E3B0328C30D}'/><EventID>4672</EventID><Version>0</Version><Level>0</Level><Task>12548</Task><Opcode>0</Opcode><Keywords>0x8020000000000000</Keywords><TimeCreated SystemTime='2020-11-12T17:27:33.622556400Z'/><EventRecordID>1598721</EventRecordID><Correlation/><Execution ProcessID='740' ThreadID='2508'/><Channel>Security</Channel><Computer>EC2AMAZ-CPMK6J5.cribl.poc</Computer><Security/></System><EventData><Data Name='SubjectUserSid'>S-1-5-18</Data><Data Name='SubjectUserName'>EC2AMAZ-CPMK6J5$</Data><Data Name='SubjectDomainName'>CRIBL</Data><Data Name='SubjectLogonId'>0xa58ba5</Data><Data Name='PrivilegeList'>SeSecurityPrivilege
			SeBackupPrivilege
			SeRestorePrivilege
			SeTakeOwnershipPrivilege
			SeDebugPrivilege
			SeSystemEnvironmentPrivilege
			SeLoadDriverPrivilege
			SeImpersonatePrivilege
			SeDelegateSessionUserImpersonatePrivilege
			SeEnableDelegationPrivilege</Data></EventData></Event>
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{54849625-5478-4994-A5BA-3E3B0328C30D}'/><EventID>4624</EventID><Version>2</Version><Level>0</Level><Task>12544</Task><Opcode>0</Opcode><Keywords>0x8020000000000000</Keywords><TimeCreated SystemTime='2020-11-12T17:27:33.622676800Z'/><EventRecordID>1598722</EventRecordID><Correlation/><Execution ProcessID='740' ThreadID='2508'/><Channel>Security</Channel><Computer>EC2AMAZ-CPMK6J5.cribl.poc</Computer><Security/></System><EventData><Data Name='SubjectUserSid'>S-1-0-0</Data><Data Name='SubjectUserName'>-</Data><Data Name='SubjectDomainName'>-</Data><Data Name='SubjectLogonId'>0x0</Data><Data Name='TargetUserSid'>S-1-5-18</Data><Data Name='TargetUserName'>EC2AMAZ-CPMK6J5$</Data><Data Name='TargetDomainName'>CRIBL.POC</Data><Data Name='TargetLogonId'>0xa58ba5</Data><Data Name='LogonType'>3</Data><Data Name='LogonProcessName'>Kerberos</Data><Data Name='AuthenticationPackageName'>Kerberos</Data><Data Name='WorkstationName'>-</Data><Data Name='LogonGuid'>{A9F7AB8C-F5DA-B736-AF84-F1A02DFA3FA4}</Data><Data Name='TransmittedServices'>-</Data><Data Name='LmPackageName'>-</Data><Data Name='KeyLength'>0</Data><Data Name='ProcessId'>0x0</Data><Data Name='ProcessName'>-</Data><Data Name='IpAddress'>::1</Data><Data Name='IpPort'>51932</Data><Data Name='ImpersonationLevel'>%%1833</Data><Data Name='RestrictedAdminMode'>-</Data><Data Name='TargetOutboundUserName'>-</Data><Data Name='TargetOutboundDomainName'>-</Data><Data Name='VirtualAccount'>%%1843</Data><Data Name='TargetLinkedLogonId'>0x0</Data><Data Name='ElevatedToken'>%%1842</Data></EventData></Event>
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{54849625-5478-4994-A5BA-3E3B0328C30D}'/><EventID>4634</EventID><Version>0</Version><Level>0</Level><Task>12545</Task><Opcode>0</Opcode><Keywords>0x8020000000000000</Keywords><TimeCreated SystemTime='2020-11-12T17:27:33.624089200Z'/><EventRecordID>1598723</EventRecordID><Correlation/><Execution ProcessID='740' ThreadID='780'/><Channel>Security</Channel><Computer>EC2AMAZ-CPMK6J5.cribl.poc</Computer><Security/></System><EventData><Data Name='TargetUserSid'>S-1-5-18</Data><Data Name='TargetUserName'>EC2AMAZ-CPMK6J5$</Data><Data Name='TargetDomainName'>CRIBL</Data><Data Name='TargetLogonId'>0xa58ba5</Data><Data Name='LogonType'>3</Data></EventData></Event>
<Event xmlns='http://schemas.microsoft.com/win/2004/08/events/event'><System><Provider Name='Microsoft-Windows-Security-Auditing' Guid='{54849625-5478-4994-A5BA-3E3B0328C30D}'/><EventID>4625</EventID><Version>0</Version><Level>0</Level><Task>12544</Task><Opcode>0</Opcode><Keywords>0x8010000000000000</Keywords><TimeCreated SystemTime='2020-11-12T17:27:36.185955600Z'/><EventRecordID>1598724</EventRecordID><Correlation/><Execution ProcessID='740' ThreadID='948'/><Channel>Security</Channel><Computer>EC2AMAZ-CPMK6J5.cribl.poc</Computer><Security/></System><EventData><Data Name='SubjectUserSid'>S-1-0-0</Data><Data Name='SubjectUserName'>-</Data><Data Name='SubjectDomainName'>-</Data><Data Name='SubjectLogonId'>0x0</Data><Data Name='TargetUserSid'>S-1-0-0</Data><Data Name='TargetUserName'>SCAN</Data><Data Name='TargetDomainName'></Data><Data Name='Status'>0xc000006d</Data><Data Name='FailureReason'>%%2313</Data><Data Name='SubStatus'>0xc0000064</Data><Data Name='LogonType'>3</Data><Data Name='LogonProcessName'>NtLmSsp </Data><Data Name='AuthenticationPackageName'>NTLM</Data><Data Name='WorkstationName'>-</Data><Data Name='TransmittedServices'>-</Data><Data Name='LmPackageName'>-</Data><Data Name='KeyLength'>0</Data><Data Name='ProcessId'>0x0</Data><Data Name='ProcessName'>-</Data><Data Name='IpAddress'>163.172.105.54</Data><Data Name='IpPort'>0</Data></EventData></Event>
```
