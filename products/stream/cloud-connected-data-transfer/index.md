# Send Data from On-Prem to Cribl.Cloud


You can use Connected Environments to send data from an on-prem deployment to a Cribl.Cloud deployment. For example, if you previously had an on-prem deployment of Cribl and you are moving forward with a Cloud-first or Cloud only solution, you can use connected environments to onboard your data from a on-prem Worker Nodes to their counterpart Cribl.Cloud Worker Nodes.

You can transfer data from on-prem Worker Nodes to the Cribl.Cloud Worker Nodes without incurring any data transfer costs.

## Overview of the Data Migration Workflow

The workflow for migrating data from on-prem Worker Nodes to Cribl.Cloud Worker Nodes has these phases:

1. [Create a Connected Environment](#new-connected-env). This process generates the information you need to set up the data flow between the on-prem Worker Nodes and the Cribl.Cloud Worker Nodes.

1. [On-Prem Worker Nodes: Configure HTTP or TCP Destination](#on-prem-destination). Add an auth token on the on-prem Worker Nodes to configure the first half of the connection between the on-prem Destination and the Cribl.Cloud Source.

1. [Cribl.Cloud Worker Nodes: Configure HTTP or TCP Source](#cloud-source). Add a matching auth token on the Cribl.Cloud Worker Nodes to complete the connection.

1. [Validate the Data Flow](#validate-data-flow). Confirm that the data is properly flowing from the on-prem Worker Nodes to the Cribl.Cloud Worker Nodes.

### Create a Connected Environment {#new-connected-env}

To begin the process of migrating data from on-prem to Cribl.Cloud, you need to first set up a Connected Environment on the on-Prem Leader. This process generates the information you need to set up the data flow between on-prem Worker Nodes and the Cribl.Cloud Worker Nodes.

To set up a Connected Environment, follow the instructions on [How to Connect On-Prem Leaders to Cribl.Cloud](cloud-connected-env-how-to). These instructions also explain how to:

- Optionally set up environment variables at the time you create the Connected Environment.
- Use a command-line script to set up a Connected Environment if needed.

Ensure that you also follow the instructions to [Enable the On-Prem Leader Connection to Cribl.Cloud](cloud-connected-env-how-to#enable-connection).

### On-Prem Worker Nodes: Configure HTTP or TCP Destination {#on-prem-destination}

On the on-prem deployment, select a Worker Node and configure a new or existing [Cribl HTTP](destinations-cribl-http) or [Cribl TCP Destination](destinations-cribl-tcp). These Destinations enable internal data transfer between Cribl nodes to prevent data transfer billing costs.

To configure an HTTP or TCP Destination:

1. Follow the general configuration instructions on the [Cribl HTTP](destinations-cribl-http) or [Cribl TCP Destination](destinations-cribl-tcp) documentation.

1. In the **Auth Token** section of the configuration modal, select **Auth Token** to set up a shared secret between this Destination and the Cribl.Cloud Source:

   - **Token secret (text secret):** Select the name of the secret that you have created previously or create a new secret using the **Create** button. See [Create and Manage Secrets](securing-secrets) for specific instructions about creating secrets.
   - **Enable token:** Toggle the token on to enable it.
   - **Description:** Optionally, enter a description summarizing the purpose of the secret.

   > If you don't see the **Auth Token** section of the Destination, it means you have not set up a Connected Environment for your on-prem Leader.
   >
   {.box .warning}

1. Commit and deploy the changes.

You can add multiple tokens if you need to rotate auth tokens.

### Cribl.Cloud Worker Nodes: Configure HTTP or TCP Source {#cloud-source}

On the Cribl.Cloud deployment, select a Worker Node and configure a new or existing [Cribl HTTP](sources-cribl-http) or [Cribl TCP Source](sources-cribl-tcp). These Sources enable internal data transfer between Cribl nodes to prevent data transfer billing costs.

To configure an HTTP or TCP Source, follow the same steps listed in the previous section about [Configuring HTTP or TCP Destination for the on-prem Worker Nodes](#on-prem-destination).

When you select or create the secret from the **Token secret (text secret)** menu, this secret must match the value of the secret used on the on-prem Cribl HTTP or Cribl TCP Destination.

Commit and deploy after making these configuration changes.

### Validate the Data Flow {#validate-data-flow}

As a final step, confirm that the data is properly flowing from the on-prem Worker Nodes to the Cribl.Cloud Worker Nodes. A few methods for confirming data flow include:

- Capture sample data on your Cribl HTTP or TCP Source on your Cribl.Cloud Worker Nodes and verify it looks correct. See [Add Sample Data](data-preview#add) for specific instructions.
- In the configuration modal for your Cribl HTTP or TCP Source on your Cribl.Cloud Worker Nodes, select the **Charts** tab to view data events coming in to verify you see activity. You can also compare this to the outgoing data on the Cribl HTTP or TCP Destination on your on-prem Worker Nodes.
