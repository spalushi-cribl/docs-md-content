# Manage Collector State in Database and REST API Collectors


## Manage State in Database Collector {#manage-state-db-collector}

For the Database Collector, **state** identifies the last row that the Collector has read from the database, as expressed by that row's value in the [tracking column](collectors-database#tracking-column). You can view, edit, refresh, or delete that state – and that is what we mean by managing state. Testing or debugging a Collector is the main reason to do this.

## Manage State in REST Collector {#manage-state-rest-collector}

The REST Collector tracks **state** according to [configuration of the expressions](collectors-rest#tracking-state-collector-runs) **State update** and **State merge**. With state tracking enabled, these expressions default to track state by the greatest event `_time` value.

You can enable state tracking in the REST Collector in **State Tracking** when you schedule or perform a full run.

## Manage State

The **Manage State** button is enabled whenever your Collector state is not empty, and is available from two locations: 

* In the **Database** list view of your Database Collectors, at the end of each row. Select **Manage**, **Data**, **Manage Sources**, **Collectors**, **Database** to reach this UI.
* In the configuration modal for each Collector.

To view state, select **Manage State**. This opens the **Manage State** JSON editor, since state is recorded as a JSON object. For example, if you have chosen the `id` column in your database as your tracking column, and your Collector has just read 5 rows, the state object will look like this:

```json
{
  "id": {
     "value": 5
  }
}
```

Suppose a particular job did not read the data you expected it to. Viewing state is the most basic troubleshooting step in this kind of situation.

If you know that state was updated because the Collector has run, but the state shown in the editor does not reflect the change you expected, select **Refresh State**.

If you want to start again with no state, select **Delete State**. To learn how and why to use this operation, see [Deleting Collector State](#delete-state), below. 

### Edit Collector State {#edit-state}

In the **Manage State** JSON editor, you can change state by editing the JSON, and then select **Save State** to persist the change.

To continue our example, if your Collector has read 1000 rows but you want it to repeat the last 500 rows on the next scheduled run, you would change the state from `1000` to `500`.

### Delete Collector State {#delete-state}

The **Delete State** and **Delete Collector** buttons are independent of each other. Deleting the Collector does **not** delete its saved state information. Cribl Stream decouples these two operations to provide benefits that we'll explore in this section.

To begin with, note that Collector state is stored on disk, not in Git. Each Collector state is stored separately.

Secondly, the uses of state-deleting differ depending on whether your Database Collector is on a single-instance deployment of Cribl Stream, or a distributed deployment.

#### Delete Collector State in a Single-Instance Deployment

When you first configure a Database Collector, you might need to experiment with SQL queries until you arrive at one that returns the desired results. In doing this, you might need the queries you're testing to always start from the same place in the database. If you're using a tracking column, this won't work, because each query will move you further along in the database. The solution is to delete state between iterations of the query you're building.

#### Delete Collector State in a Distributed Deployment

Just like in single-instance deployments, in distributed deployments it can be useful to delete Collector state when you're testing SQL queries for a Collector with state tracking enabled.

Beyond that, with distributed deployments it's important to know that Cribl Stream decouples the deleting of Collectors themselves from the deleting of their saved state, and to see the benefits of that decoupling.

Suppose you have been running a Collector with state tracking enabled, and it's working its way through a database. At some point you decide you're not happy with that Collector and you select the **Delete Collector** button. What happens next? There are at least three possibilities:

* If you're sure you want to delete the Collector, you should Commit and Deploy. Once you do that, the Leader pushes the change out to the Worker Nodes, and the Collector is gone (although you can recover it by rolling back to a previous commit in Git).

* If you realize that you made a mistake, and want to keep the Collector after all, then you should **not** Commit and Deploy. Instead, select the **Undo All Changes** button in the **Git Changes** modal – this un-deletes the Collector. And, since the Collector state remains on disk, the Collector will pick up where it left off.

* If you deleted the Collector because you want to create another Collector with the same ID and the same tracking column, but a different configuration, you should Commit and Deploy, then create your new Collector. Because the old Collector state remains on disk, the new Collector with the same ID picks up the saved state. When you begin using the new Collector, it will pick up where the old Collector left off. On the other hand, if you prefer the new Collector to start from zero, just select the **Delete State** button before you start using it.

In short, Cribl Stream decoupling of Collector deletion from state deletion gives you control, such that you can avert messy situations where your Collector cannot find the state it needs.