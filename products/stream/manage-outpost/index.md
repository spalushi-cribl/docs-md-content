# Manage Cribl Outpost

 

Control, diagnose, and upgrade your Cribl Outposts.

---

> ##### Preview Feature {#preview}
>
> This Preview feature is still being developed. We do not recommend using it in a production environment,
> because the feature might not be fully tested or optimized for performance, and related documentation could be incomplete.
>
> Please continue to submit feedback through normal Cribl [support channels](/support/),
> but assistance might be limited while the feature remains in Preview.
> 
{.box .info}

## View Outposts {#view}

You can view all Outposts configured in your deployment on the **Outposts** page.

1. On the top bar, select **Products**, and then select **Stream**.
1. In the sidebar, select **Outposts**.

You will see a list of all Outposts, including disconnected ones.
Select any row in the table to see detailed information about the Outpost.

![Cribl Edge UI with a list of two Outposts](outposts-list-4.14.png)
{border="true" scale="80%" caption="List of Outposts"}

### View Outpost Logs

To view Outpost logs, consult the **Logs** tab in the Outpost screen.
For more information about Outpost logs, see [Internal Logs](internal-logs#outpost).

### Create Outpost Diagnostic Bundle

In the outpost screen you also create a diagnostic bundle to help to share with Cribl Support as help in diagnosing any issues.
To do so, on the **Diagnostic** tab, select **Create Diagnostic Bundle**.
To learn more about diagnostic bundles, see [Diagnose Issues](diagnosing).

> You can also create diagnostic bundles by using the [`diag`](cli-diag) command.
{.box .success}

## Restart an Outpost {#restart}

> Required Permission: **Admin/Owner**
{.box .info}

To restart an Outpost:

1. On the top bar, select **Products**, and then select **Stream**.
1. In the sidebar, select **Outposts**.
1. Select the check box in the row of the Outpost or Outposts you want to restart and select **Restart**.
1. Confirm with **Restart**.

## Upgrade an Outpost {#upgrade}

> Required Permission: **Admin/Owner**
{.box .info}

You can upgrade any Outpost to the current Leader version.
To upgrade an Outpost:

1. On the top bar, select **Products**, and then select **Stream**.
1. In the sidebar, select **Outposts**.
1. Select the check box in the row of the Outpost you want to upgrade and select **Upgrade**.
1. You will see the Leader version that the Outpost will upgrade to. Confirm with **Upgrade**.

The recommended order of upgrade is:

1. Upgrade the Leader Node.
1. Upgrade Outposts.
1. Upgrade individual Nodes.

For more information, refer to [Upgrading Overview](upgrading).
