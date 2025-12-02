# Fleet Upgrading Use Case


An example upgrading use case, starring fictional solar energy company Greatgreen Goats.

---

## Greatgreen Goats: A Fleet Upgrading Use Case

To help you conceptualize automatic Fleet upgrades, this use case walks you
through how the fictitious company Greatgreen Goats upgrades Nodes in their two
Fleets.

Greatgreen Goats is a small but rapidly growing company focused on
installing and maintaining solar energy systems at goat farms around the country.

They use Cribl Edge to manage data on their edge nodes, divided into two
distinct Fleets:

- **Fleet 1: Linux Operations.** This Fleet contains 20 Nodes, all running Edge
  v.4.4.4. When the IT admin upgrades the Leader of Fleet 1 to 4.5, and they set the
  target upgrade version in the drop-down to **None**. Therefore, Nodes in
  Fleet 1 are never upgraded. They continue to remain at their current version,
  4.4.4.
- **Fleet 2: Windows Monitoring.** This Fleet contains 15 Nodes. In this Fleet,
  the IT admin selects the current Leader version (in this case, 4.5) in the
  **Target upgrade version** drop-down. Now, when Nodes connect to the Leader,
  they pull down version 4.5 and upgrade to that version. 
  
Fast forward four weeks: Cribl Edge has released a new version: 4.5.2. The
Leader is now at version 4.5.2.  

Since Fleet 1 does not have a target version selected, Nodes in Fleet 1 remain
at their current version, 4.4.4. If the IT admin decides to change the
**Target upgrade version** to match the Leader in Fleet 1, the Nodes in Fleet 1
will be upgraded to 4.5.2. 

Nodes in Fleet 2 will remain on version 4.5, which was the Leader's previous
version, until the admin selects the next **Target upgrade version** (4.5.2, the
Leader's new version). Then, Nodes will request the new version the next time
they connect to the Leader and automatically upgrade.

This approach gives the IT admin at Greatgreen the flexibility to upgrade Nodes in
Fleets independently from the Leader. Greatgreen can now scale and upgrade their
Fleets with more reliability, since Nodes are pulling the release down from the
Leader (instead of the Leader creating an overwhelming number of upgrade jobs).