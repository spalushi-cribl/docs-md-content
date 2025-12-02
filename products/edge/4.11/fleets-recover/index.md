# Recover a Deleted Fleet


Learn how to recover deleted Fleets in Cribl Edge

---

If you delete a Fleet, you can no longer access its **Version Control** menu.
As a result, you also lose the option to directly recover the Fleet. 

You can, however, still recover a deleted Fleet, using one of the following two methods:

- [Create a new Fleet with the same name](#newfleet).
- [Revert commit via command line](#reverting-via-cli).

### Creating a New Fleet with the Same Name {#newfleet}

When you create a new Fleet with the same name as a deleted one,
it will restore that Fleet with all of its configuration.

1. Select **Fleets** from the sidebar, then **Add Fleet**.
1. Set **Fleet name** to be exactly the same as the deleted Fleet. Fill in the remaining information and **Save**.
1. If you previously committed the deletion of the original Fleet, open the **Version Control** menu.
1. Select a previous version of the deleted Fleet and confirm with **Done**.

### Reverting Commit via Command Line {#revert-via-cli}

You can restore a deleted Fleet in version control using the command line. 
To revert the deletion commit:

1. Stop the Cribl server.
1. Using a command line interface, look at the version control history for the Leader Node
   and identify the commit which deleted the Fleet.
1. Revert to the last commit before the Fleet was deleted.
1. Restart the Cribl server.