# Enterprise Cloud


With a Cribl.Cloud Enterprise plan, you have the same options and flexibility that an Enterprise license provides for a customer-managed (on-prem) distributed deployment – and more:

- Configuring and managing multiple [Worker Groups](/stream/deploy-distributed#worker-groups) and [Fleets](/edge/fleet-management).
- [Notifications](notifications) within Cribl apps, to PagerDuty, and/or to other services via webhook.
- Fine-grained, role-based control of [Member Permissions](cloud-managing#member-roles) on your Cribl.Cloud Organization and on individual [product resources](members).

- Single sign-on/[SSO authentication](cloud-sso) from external identity providers.
- The [hybrid deployment](#hybrid) option, described just below.

With an Enterprise Plan, the Leader resides in Cribl.Cloud, and controls a flexible mix of Cribl-managed and/or customer-managed Worker Groups. Cribl manages the Leader's high availability on your behalf.

> For other Enterprise features – and for comparisons between Cribl.Cloud plans and on-prem licenses – see Cribl's [Pricing](https://cribl.io/pricing/) page.
>
{.box .info}

## Hybrid Deployment {#hybrid}

The diagrams below show the comparative flexibility of a hybrid Cribl.Cloud deployment. The Leader (control plane) resides in Cribl.Cloud, while the Workers that process the data can be in any combination of the following environments: 

- In Cribl.Cloud, managed by Cribl. 
- In public or private cloud instances that you manage.
- On-prem in your data centers.

![Enterprise hybrid deployment, with control plane and Cribl-managed Workers in Cribl.Cloud](se-cloud-hybrid-arch-edge.cf56e2621f.png)
{scale="70%" border="true"}

![Enterprise hybrid deployment, with only control plane in Cribl.Cloud](hybrid-cloud-headless.00640a45ba.png)

As the footprint of your operations grows or changes, this flexibility makes it easy to reconfigure Cribl Stream in tandem. You can rapidly expand Cribl Stream observability into new cloud regions – and replace monitored hardware data centers with cloud instances – all while maintaining one centralized point of control. 

You can also add Workers or Edge Nodes, and reassign them to different Worker Groups, by easily auto-generating [Stream](/stream/deploy-workers#ui) or [Edge](/edge/managing-edge-nodes#bootstrap) command-line scripts within Cribl Stream's UI.

### Hybrid Requirements {#hybrid-req}

A hybrid deployment imposes these configuration requirements:
- Hybrid Workers (meaning, Workers that you deploy on-prem, or in cloud instances that you yourself manage) must be assigned to a different Worker Group than the Cribl-managed `default` Group – which can contain its own Worker Nodes.
- All Worker Nodes' hosts must allow outbound communication to the Cribl.Cloud Leader's port 4200 at `https://main-<Organization-name>.cribl.cloud:4200`, to enable configuration and workload management by the Leader.
- On all Worker Nodes' hosts, firewalls must allow outbound communication on port 443 to the Leader, and on port 443 to `https://cdn.cribl.io`.
- All Worker Nodes require connectivity to `https://cdn.cribl.io/telemetry/`. For details on testing this connectivity, on the metadata transmitted to Cribl, and on how we use that data, see [Telemetry Data](licensing#telemetry-data).
- If this traffic must go through a proxy, see [System Proxy Configuration](proxy-config) for configuration details.
- To verify your Leader's Region and public URL, open the [Access Details](cloud-portal#overview) modal. 

Note that you are responsible for data encryption and other security measures on Worker Node instances that you manage.

### Adding (Bootstrapping) Workers

To add Workers to your hybrid Cribl.Cloud deployment, Cribl recommends that you use the script outlined in [Bootstrap Workers from Leader](/stream/deploy-workers#add-bootstrap). Hosts for the new Workers must open the same ports (4200 and 443) listed in [Hybrid Requirements](#hybrid-req).

You have three options for generating the script, outlined in these subsections of the Bootstrap topic linked above:
- Auto-generate it from the [Leader's UI](/stream/deploy-workers#ui).
- Make a `GET` [API request](/stream/deploy-workers#api-spec) to the Leader.
- [curl](/stream/deploy-workers#curl) the same API request.

> In Cribl Edge, you access all these bootstrap options via the [Manage Edge Nodes](/edge/managing-edge-nodes#workers-tab) page's **Add/Update Edge Node** control.
>
{.box .success}

### Hybrid Cribl HTTP/TCP Configuration {#hybrid-pairs}

If you use the Cribl HTTP [Destination](destinations-cribl-http) and [Source](sources-cribl-http) pair, or the Cribl TCP [Destination](destinations-cribl-tcp) and [Source](sources-cribl-tcp) pair, to relay data between Worker Nodes connected to the same Leader, configuring hybrid Workers demands particular care.

The Worker Nodes that host each pair's Destination and Source must specify exactly the same Leader Address. Otherwise, token verification will fail – breaking the connection, and preventing data flow.
In hybrid Cribl.Cloud deployments, the Leader's Address format is `main‑<your‑Org‑ID>.cribl.cloud`. When configuring a hybrid Worker, use that format in the **Address** field.

To configure hybrid Workers:
1. Log directly into their UI, then select **Settings** > **Global Settings** > **Distributed Settings**.
  Make sure the **Mode** is set to **Managed Worker** or **Managed Edge** (which might require a restart). 
2. Then select the **Leader Settings** left tab, and ensure a consistent entry in the **Address** field.
