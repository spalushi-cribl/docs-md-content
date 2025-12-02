# Managing QRadar Licenses


If you're using a SIEM (security information and event management) system like QRadar that's subject to an events-per-second (EPS) licensing model, managing license costs can be an issue. Cribl Stream enables you to proactively manage your license by slowing EPS growth. 

Cribl Stream does so by offering fine-grained control over what events to send to the QRadar Event Processor, without breaking the expected [LEEF](https://www.ibm.com/docs/en/dsm?topic=leef-overview) (Log Event Extended Format) data format. You can report on event codes, and make decisions about what to drop, based on the data in the events. Some of what we discuss here also applies to SIEMs generally, not just QRadar.

## Confronting QRadar Constraints 

QRadar imposes constraints on data through several of its components:
* QRadar Event Collectors can send data only to QRadar. Elastic and other systems of retention or analysis are not an option.
* QRadar Event Collectors can send data only in LEEF format. The pipeline cannot provide additional streams of data in different formats such as JSON or syslog.
* Incoming data must adhere to a predefined schema, based on IBM's LEEF format. You can't reformat events, nor reduce the size of an event, without breaking the correlation rules within the QRadar Event Processor.
* When incoming data exceeds the EPS license limits, QRadar will drop or stop ingesting data. Meanwhile, since the license is based on EPS event velocity, license costs increase linearly: the higher the EPS count, the higher the cost. 

If your event velocity approaches the license limit, your options are unappealing: 
* You can try to exclude some data, but the correlation rules and constraints on data formatting restrict your ability to be selective. You can drop only broad categories of data, such as an entire Windows Event ID, leaving you at a risk of missing something important. 
* You can hope that you never actually exceed the EPS limit, but if you guess wrong, QRadar dropping or not ingesting data can be disruptive and compromise your enterprise security posture. 

Cribl Stream enables you to surgically minimize these risks, and get the most out of your existing licenses. 

## Dropping Events Selectively

A common tactic to avoid exceeding an EPS license limit is to start dropping an entire class of events. This is a crude approach that carries its own risks. With Cribl Stream, you can analyze the data within the event, and decide what to keep based on your priorities.

### Analyzing a Windows Event

Here's an example of how to analyze and selectively drop [Windows Security Event ID 4674](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4674). This event documents operations attempted on a privileged object. While it's commonly discarded when admins are feeling pressed to drop events, 4674 is actually a critical ID that should be kept some, or most, of the time.  

Event ID 4674 includes the logon types shown in the table below.

Logon Type| Logon Title| Description
--- | ------ | ---
2 | `Interactive`| Activity on the system's keyboard and screen.
3 | `Network`| Connection to a shared folder on this computer from elsewhere on the network.
4|  `Batch` | Processes might be executing on behalf of a user without their direct intervention.
5|  `Service` | Service startup by the Service Control Manager.
7|  `Unlock` | An unattended workstation with password-protected screen saver.
8|  `NetworkCleartext`| Logon with credentials sent in cleartext. Most often indicates a logon to IIS with “basic authentication”. 
9|  `RemoteInteractive`| Logon with RunAs, or mapping a network drive with alternate credentials. 
10| `CachedInteractive`| Logon with cached domain credentials, such as when logging on to a laptop when away from the network.

Each logon type is assigned a different security value. 

Let's suppose that your QRadar deployment is in an enterprise where inbound file sharing is disabled. In that case, you don't need logon type 3 events. So let's see how to use Cribl Stream to keep all ID 4674 events except logon type 3 events, which we'll' drop. 

### Dropping the Excess Baggage

> Use sample data to validate the reduction methods described below. Only when you are sure they won't interfere with your system's usability should you apply these methods to your production SIEM. 
>
{.box .warning}

1. Save your Windows Events to Cribl Stream using [Live Capture](data-preview#live).
2. [Create a Pipeline](getting-started-guide#pipeline) and name it `Windows_Event_Reduction`. 
3. [Add the Regex Extract Function](usecase-regex-filtering) to your Pipeline to create fields for `EventCode` and `Logon Type`. These fields will be the foundation for a Filter that drops only Windows events of ID 4624, logon type 3.

    ![Regex Function on Pipeline](st-usecase-qradar-regex.6ad73b1fbf.png)
    {border="true"}

4. [Add the Drop Function](stream/drop-function) to your Pipeline using the Filter below. 

    ```js

    index=='endpoint' && source=='WinEventLog:Security' && __eventCode=='4624' && __logon_Type=='3'

    ```

    ![Drop Function](st-usecase-qradar-filter.4f25bfc4f5.png)
    {border="true"}

5. Apply the Pipeline to your Windows Sources. Cribl Stream will then start dropping the unneeded events.

## Other Reduction Examples 

> Apply the options in this section only after careful analysis of your own environment and priorities. Even if you choose to exclude these events from QRadar, you might want to send them to an object store for later auditing and/or replay. 
>
{.box .warning}

### WinEvents

- Event ID 4624: Look for Subtypes (Logon Type) that can be dropped.
- Event ID's 4634 and 4674: An account was logged off. Most security use cases don't require Logoff events. 
- Event ID 4673: A privileged service was called. This event rarely contains actionable intel. It's a large-volume data source that can be dropped, in exchange for other, more-valuable event IDs.
- Event ID 4674: An operation was attempted on a privileged object. Similar to Event ID 4673, this event is largely considered "noise," providing little value. Consider dropping. 
- Event ID 4689: A process was exited. This source, when paired with Event ID 4688, can tell you "how long a process was running." Typically, this is unimportant information, and can be omitted from your event collection. 

### Palo Alto

The following filters can be helpful to reduce the noise: 

- All external-to-internal traffic AND `action=deny`: `Ingress traffic && action=deny`

- `PAN Traffic logs && application=ping|ibm-bigfix|outlook-web-online|google-base|traceroute|ms-teams|ic mp|outlook-web-online|ms-office365-base|crowdstrike`

### Cisco ASA


- All external-to-internal traffic AND `action=deny`: `Ingress traffic && action=deny`
- Teardown UDP Connection && Event ID 302016
- Teardown TCP connection && Event ID 302014
- Teardown ICMP connection && Event ID 302021
- Teardown Translation && Event ID 305010|305012