# Projects


Projects (also called Stream Projects) diversify who can use Cribl Stream, and how:
- Projects create isolated spaces for teams and users to share and manage their data. Project editors get a streamlined visual UI. 
- Cribl admins can filter users' access to specific data and Destinations – scoping the access to match what's relevant to those users' work.

This self-service approach to Cribl Stream data can benefit its immediate users, by offering them accelerated access to relevant data, with minimal configuration requirements. It can also benefit their peers. 

Project editors get isolated data flows, and they can customize them to meet their needs, with no impact on other Cribl Stream users across their organization. Data can be forked into (for example) full-fidelity versus redacted views, to match different groups' authorization and needs.

> You can also isolate data and permissions at a higher level by using [Workspaces](workspaces) (available in Cribl.Cloud).
> Workspaces offer a separate VPC (virtual private cloud) that acts as a separate environment
> that contains not only Cribl Stream, but all Cribl products.
> See [Workspaces](workspaces) for more details.
{.box .success}

> ##### Breaking Change
>
> Any Projects you created in v.4.1.x are deleted when you upgrade to v.4.2. You must re-create your Projects after you upgrade. You should also delete and re-create any Subscriptions that carry over during the upgrade. 
> 
> In Cribl Stream 4.2 and later, Projects/Subscriptions rely entirely on the fine-grained [Members/Permissions](/stream/members) access control first introduced in Stream 4.2. See [Adding Users to Projects](/stream/sharing-projects) for details.
>
{.box .warning}

## Use-Case Scenario {#scenario}

Imagine that a Cribl administrator wants to filter data from a shared set of firewall Sources, splitting the data between her organization's Security and Ops teams. A requirement is that each team's data should be isolated, with neither team having access to the other team’s data. The Security team uses Splunk, while the Operations team uses Elasticsearch. 

Projects enable this filtering and isolation. Each team gets secure access to its own sets of data, and each team's data transformations and configuration changes don't affect the other team’s data or configs. Additionally, data capture and samples are isolated to each project.


## Requirements {#requirements}

Enabling Projects requires either an Enterprise [license](licensing) and a distributed on-prem deployment, or a Cribl.Cloud Enterprise [plan](cloud-enterprise).

Projects' Subscriptions can ingest data only from Cribl Stream Sources configured as **Send to Routes**, not from [QuickConnect](quickconnect) Sources. (This is despite Projects' QuickConnect-like Project view, introduced [below](#admin).)

## Components

This feature relies on the following building blocks:

- [Data Projects](#projects-about)
- [Subscriptions](#subscriptions)
- [Roles and Permissions](#roles)

### Data Projects {#projects-about}

The **Data Projects** tab is where Cribl admins can add and configure Projects, share access to those Projects, and associate Subscriptions with specific Destinations. 

Admins see an expanded version of the same drag-and-drop visual UI that Project editors can use to connect configured Subscription with configured Destinations. 

Each project maintains its own independent processing Pipelines, data capture and sample data.

### Subscriptions {#subscriptions}

Each Subscription filters a subset of a Worker Group's incoming data to expose to Project editors, along with processing options for that data.

To ensure Project metrics appear in [Subscription charts](monitoring#monitoring-page), remove `project` from the **Disable field metrics** list in both Worker and Leader [group settings](monitoring#criblmetrics-override). If `project` is present in this list, metrics for Projects will not be collected or displayed.

### Members and Permissions {#permissions}

Projects work with the [Members and Permissions](members) authorization system. Cribl Stream admins – users who hold the Stream- or Worker Group-level  `Admin` or `Editor` Permission, or the legacy `stream_admin` Role – can create and configure Projects and Subscriptions. They can access resources outside individual Projects to build the Projects.

As a Stream Admin, you also control users’ access to Projects and Subscriptions. For details, see [Adding Users to Projects](/stream/sharing-projects).

### How the Components Fit Together {#sequence}

The components' names and relationship are easiest to remember in terms of the publish/subscribe model long used by Cribl Stream integrations like [Kafka](sources-kafka) or [Google Cloud Pub/Sub](sources-google_pubsub):
- A **Project** is a microcosm of a whole Cribl Stream deployment – gathering together Subscriptions' incoming data, routing/processing, Destinations, and authorized users.
- A **Subscription** specifies a subset of available data to retrieve, and how to import the data.

![How Subscriptions and Projects filter incoming data](projects-isometric.ec18a4651b.png)
{border="true"}

## Getting Started with Projects {#start}

You interact with Projects in either of two ways, depending on your Cribl Stream Permissions.

### As a Project Editor {#user}

When you log into Cribl Stream and navigate to a Group, you'll find your Projects and Subscriptions already set up for you in a convenient visual UI. 

You can adjust your Projects' data flow by connecting Subscriptions to Destinations. You can insert and configure Pipelines, and can insert Packs, that are part of the Project. You can also commit your configuration changes to Git. This all works the same way as in [QuickConnect](quickconnect).

### As a Cribl Admin {#admin}

To access Projects configuration options:

1. Navigate to a Worker Group. (Projects and their components are configured per Group, providing a first layer of isolation).
2. Select **Manage** > **Projects**. This exposes two lower tabs: **Data Projects** (the default, also called the Projects view) and **Subscriptions**.
3. See the following pages for details on configuring each of these components for Project editors.

![Accessing the Project view](data-projects-tab.e08515ecc4.png)
{border="true"}
