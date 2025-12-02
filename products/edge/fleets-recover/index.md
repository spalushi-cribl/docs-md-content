# Recover a Deleted Fleet


Learn how to recover deleted Fleets in Cribl Edge.

---

If you delete a Fleet, you can no longer access its **Version Control** menu.
As a result, you also lose the option to directly recover the Fleet.

However, you can still recover a deleted Fleet using the command line.
To revert the deletion commit:

1. Stop the Cribl server.
1. Using a command line interface, look at the version control history for the Leader Node
   and identify the commit which deleted the Fleet.
1. Revert to the last commit before the Fleet was deleted.
1. Restart the Cribl server.
