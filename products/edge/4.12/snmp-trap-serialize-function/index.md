# SNMP Trap Serialize


Use the SNMP Trap Serialize Function to convert compliant events into SNMP traps for sending out through [SNMP Trap Destinations](destinations-snmp-traps) that were modified within a Pipeline from an [SNMP Trap Source](sources-snmp-traps). This ensures compatibility with existing SNMP infrastructure and supports integration with network management tools that use SNMP for monitoring and alerting.

## Limitations

* The SNMP Trap Source the events came from must have **Include varbind types** enabled for this function to work. The SNMP Trap Serialize Function needs the varbind types to correctly serialize the events.

* For v2c events, `sysUpTime` and `snmpTrapOID` appear both at the top level and within varbinds. During serialization, the Function uses the values in varbinds as they reflect the event's generation time. If the values differ, varbinds take precedence. Modifying the top-level values will not affect serialization.

## Usage

**Filter**: Filter expression (JavaScript) that selects data to feed through the Function. Defaults to `true`, meaning it evaluates all events.

**Description**: Simple description of this Function. Defaults to empty.

**Final**: Toggle on to stop feeding data to the downstream Functions. Default is toggled off.

**Enforce required fields**: When toggled on (default), v2c and v3 traps must meet the following requirements:
* At least two varbinds must be present.
* The first varbind must have the `sysUpTime` OID (`1.3.6.1.2.1.1.3.0`) and type `67` (`TimeTicks`).
* The second varbind must have the `snmpTrapOid` OID (`1.3.6.1.6.3.1.1.4.1.0`) and type `6` (`OID`).

These requirements are mandated by [RFC 3416 Section 4.2.6](https://datatracker.ietf.org/doc/html/rfc3416#section-4.2.6), [RFC 1066 Section 5.1](https://datatracker.ietf.org/doc/html/rfc1066#section-5.1), and [RFC 1905 Section 3.1.2](https://datatracker.ietf.org/doc/html/rfc1905#section-3.1.2). While traps can be serialized without these fields, downstream receivers may drop them. With this option enabled, non-compliant traps are dropped before being sent downstream.

**Drop failed events**: When toggled on (default), events that fail to serialize into valid traps are dropped entirely, preventing them from passing through the Pipeline. This is useful when sending to an SNMP Trap Destination, as invalid traps will not be sent.

When toggled off, the event will pass through without `__snmpRaw` and will include a `snmpSerializeErrors` field for troubleshooting.

Regardless of whether **Drop failed events** is toggled on or off, if an event fails to serialize, the `__snmpRaw` field is deleted, preventing the event from leaving the SNMP Trap Destination.

Recommended setup flow:

1. Get a sample of the traps to modify and configure the SNMP Trap Serialize Function.
1. Enable **Drop failed events** and run a [Data Preview](data-preview).
1. Modify the SNMP Trap Serialize Function and preceding Eval Function until no `snmpSerializeErrors` remain.
1. Disable **Drop failed events** when confident in the Pipeline setup and ready to flow actual data.

**SNMPV3 User**: SNMPv3 provides enhanced security by enabling user authentication and data encryption. These settings are crucial for ensuring that only authorized users can send SNMP traps and that the data remains secure during transmission.

* **Username**: The username for SNMP v3 authentication. Ensure the username matches the one configured on the SNMP manager.

* **Authentication protocol**: The protocol to use for SNMP v3 authentication. Options include `None`, `MD5`, `SHA1`, `SHA224`, `SHA256`, `SHA384`, and `SHA512`.

  * **V3 authentication key**: The key used for SNMPv3 authentication. This field appears when an **Authentication protocol** is selected.
  * **Privacy protocol**: The protocol to use for SNMPv3 encryption. This field appears when an **Authentication protocol** is selected, as it is not possible to have an encrypted trap without authentication. Options include:
    * `None`
    * `AES128`
    * `AES256b (Blumenthal)`
    * `AES256r (Reeder)`
  * **V3 privacy key**: The key used for SNMPv3 encryption. This field appears when a **Privacy protocol** is selected.

## Maintain Original Source IP in SNMP Traps

This setup maintains the original source IP in SNMP traps by adding it to the varbinds array, ensuring accurate data transmission even when traps traverse network devices that alter IP headers.

Before the SNMP Trap Function in your Pipeline, use the [Eval Function](eval-function) to append a new object to the varbinds array. The object includes an OID, type, and the value from the host field, effectively capturing the original source IP. For example:
   * **Name**: `varbinds`
   * **Value expression**: `[...varbinds, {oid: '.1.3.6.1.6.3.18.1.3', type: 64, value: __e.host}]`

Next, for the SNMP Trap Serialize Function we recommend enabling both **Enforce required fields** and **Drop failed events**. This will ensure that the SNMP trap is serialized correctly, enforcing necessary fields and dropping events that fail serialization.

## Change Authorization and/or Privacy Level

To change the authorization or privacy level of an SNMP v3 trap, you must modify two settings:

* **SNMPV3 User**: Update the user configuration within the SNMP Trap Serialize Function.
* **msgFlags**: Set the appropriate value at the top level of the event in an [Eval Function](eval-function) before the serialization.

The `msgFlags` value should be a string representation of the desired number:

* `00`: No authorization, no privacy
* `01`: Authorization, no privacy
* `03`: Authorization and privacy

## Change the SNMPv3 User

To change the SNMPv3 user, you need to update the `msgUserName` field within the SNMP Trap object.

Before the SNMP Trap Function in your Pipeline, use the [Eval Function](eval-function) to update `msgSecurityParams.msgUserName` to the new intended username. For example, **Eval expression**: `msgSecurityParams.msgUserName = 'newUser';`
