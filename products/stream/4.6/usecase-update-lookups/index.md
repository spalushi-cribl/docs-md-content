# Updating Lookups in Multiple Packs


[Lookups](lookups-library) provide a flexible way to enrich events en route to their Destinations. You can use Lookups in multiple [Packs](packs) to share complex configurations and workflows across multiple Worker Groups, or across organizations.

Here, we will be walking you through a particularly challenging setup: What happens if you have a very large Lookup that requires frequent updates? To make it more interesting, what happens if the same Lookup is being used in multiple Packs? This topic explains how to update a Lookup file in multiple Packs. To learn how to set up a large Lookup file that needs frequent updates, see [Deploy an Updated Lookup File to Worker Nodes](/api/create-update-lookups/#deploy-update-worker-nodes).

## Updating Lookups in Multiple Packs

Manually updating multiple Packs with an ever-evolving Lookup table is a non-starter, so let's examine a better alternative.

### Create the Lookup File in the Worker Group 

The first step is to create a Lookup file in the Worker Group. For ease of sharing and use, make sure the Lookup file is not located in the Pack. 

Our example Lookup file is `host_destination_test.csv`.

### Validate that the Lookup File Is on the Worker Node

After you **Commit** and **Deploy** to propagate the Lookup table to the Workers, validate that you can view the Lookup. 

In our example, the path is: `$CRIBL_HOME/data/lookups/host_destination_test.csv`.

### Point the Pack(s) to the Lookup File

In your Pack(s), add a [Lookup Function](lookup-function) and point it to the Worker Node Lookup file location. In our example, the path is: `$CRIBL_HOME/data/lookups/host_destination_test.csv`.

> Do not point the Pack to the Leader Lookup File location, such as:  
> `$CRIBL_HOME/groups/<your group>/data/lookups/host_destination_test.csv`.  
> Also, do not point it directly to the Pack Lookup file, such as `host_destination_test.csv`.
>
{.box .warning}

![Lookup Function's configuration](st-lookup-usecase1.cff78599e4.png)
