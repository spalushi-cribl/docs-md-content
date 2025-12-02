# Projects (Beta)


Projects (also called Stream Projects) diversify who can use Cribl Stream, and how:
- Projects create isolated spaces for teams and users to share and manage their data. Project users get a streamlined visual UI. 
- Cribl admins can filter users' access to specific data and Destinations – scoping the access to match what's relevant to those users' work.

This self-service approach to Cribl Stream data can benefit its immediate users, by offering them accelerated access to relevant data, with minimal configuration requirements. It can also benefit their peers. 

Project users get isolated workspaces, and they can customize these workspaces to meet their needs, with no impact on other Cribl Stream users across their organization. Data can be forked into (for example) full-fidelity versus redacted views, to match different groups' authorization and needs. 

## Requirements {#requirements}

Enabling Projects requires either an Enterprise [license](licensing) and a distributed on-prem deployment, or a Cribl.Cloud Enterprise plan. 

## Components

This feature relies on the following building blocks:

- [Data Projects](#projects-about)
- [Subscriptions](#subscriptions)
- [Roles and Policies](#roles)

### Subscriptions {#subscriptions}

Each Subscription filters a subset of a Worker Group's incoming data to expose to Project users, along with processing options for that data.

### Data Projects {#projects-about}


The **Data Projects** tab is where Cribl admins associate Subscriptions with specific Destinations. Admins and Project users can then use the drag-and-drop visual UI to connect each selected Subscription to any of these defined Destinations.

### Roles and Policies {#roles}

Cribl admins control users’ access to Subscriptions and Projects by assigning those users Cribl Stream [Roles](roles). 

Users who have the `project_user` Role and/or the `ProjectSourceSubscribe` Policy – and no higher permissions – view data filtered through the Projects UI. They also have read access to Cribl Stream [Data Monitoring](monitoring#data-monitoring) and [Notifications](notifications).

### How the Components Fit Together {#sequence}

The components' names and relationship are easiest to remember in terms of the publish/subscribe model long used by Cribl Stream integrations like [Kafka](sources-kafka) or [Google Cloud Pub/Sub](sources-google_pubsub):
- A **Subscription** specifies a subset of available data to retrieve, and how to import the data.
- A **Project** is a microcosm of a whole Cribl Stream deployment – gathering together Subscriptions' incoming data, routing/processing, and Destinations.

![How Subscriptions and Projects filter incoming data](projects-isometric.f89340ba0a.png)
{scale="90%" border="true"}

## Getting Started with Projects {#start}

You interact with Projects in either of two ways, depending on your Cribl Stream Role.

### As a Project User {#user}

When you log into Cribl Stream and navigate to a Group, you'll find your Subscriptions and Projects already set up for you in a convenient visual UI. 

You can customize your Projects by connecting Subscriptions to Destinations. You can also insert and configure Pipelines and Packs (within the context of your Worker Group). This all works the same way as in [QuickConnect](quickconnect).

### As a Cribl Admin {#admin}

To access Projects configuration options:

1. Navigate to a Worker Group. (Projects and their components are configured per Group, taking advantage of the Group-level [RBAC](roles#how-product-rbac-works) built into Cribl Stream.

2. Select **Manage** > **Projects**. This opens a submenu of **Subscriptions** and **Data Projects**.

3. See the following pages for details on configuring each of these components for Project users.

![Accessing the Projects submenu](subscriptions-submenu-empty.4abe3fe433.png)
{border="true"}

For guidelines on setting up individuals or groups as as Project users, see [Roles and Policies](#roles) above.
