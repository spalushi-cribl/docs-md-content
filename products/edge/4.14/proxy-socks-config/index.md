# Configure SOCKS Proxy

This document outlines how to configure Cribl Edge to use a SOCKS proxy server for outbound communication.

By using a SOCKS proxy, you can enhance security, bypass network restrictions, and route specific traffic through a controlled intermediary server. 

> The SOCKS proxy is exclusively for Edge Nodes to communicate with a Leader Node. The Leader Node doesn't use the proxy for its outbound communications.
{.box .info}

## Architectural Overview: SOCKS Proxy in Cribl Deployments

A SOCKS proxy acts as an intermediary for outbound network connections from a client application (in this case, your Edge Node). It allows you to centralize and control the egress points for your Cribl infrastructure. The specific architectural flow depends on whether your Cribl deployment is entirely on-prem or on Cribl.Cloud.

### Management vs. Data Plane Communications

If you are using a proxy for Cribl software cluster communications, it must be a SOCKS proxy (Layer 4), not an HTTP proxy. This is because cluster communications use raw TCP connectivity on port 4200, which HTTP proxies cannot handle.

#### Communication Types Overview

Cribl products require distinct proxy types for different communication streams. Management communications between Leaders and Edge Nodes use a proprietary protocol over persistent TCP connections (port 4200), necessitating a **SOCKS (Layer 4\) proxy** for relaying. 

Configuration bundle downloads, however, use standard HTTP/HTTPS protocols. If you're using a proxy for these downloads, it needs to be an **HTTP proxy** (configured via `HTTP_PROXY`/`HTTPS_PROXY` environment variables) with access to the Leader Node's (ports 443/4200) or S3 endpoints. This differs from the SOCKS proxy needed for management communications. For details, see [Manage Config Bundles](/stream/config-bundle-management).

Given these different proxy requirements, you'll often find your deployment needs both SOCKS and HTTP proxies. It really depends on your network environment and whether you choose to use proxies for specific communication types.

The table below summarizes the communication type and proxy support for each. 

| Communication Type | Protocol Requirements | Proxy Support | Port | Purpose |
| ----- | ----- | ----- | ----- | ----- |
| **Management Plane** | TCP (Layer 4\) | ✅ SOCKS only </br>❌ HTTP proxies NOT supported | `4200` | Leader ↔ Edge Node control, configuration, and status |
| **Config Bundle Downloads** | HTTP/HTTPS (Layer 7) | ❌ SOCKS proxies NOT supported</br> ✅ HTTP proxies only (`HTTP_PROXY`/`HTTPS_PROXY`) | `443`/`4200` | Edge Node downloading configuration bundles from Leader or S3 |
| **Data Plane** | HTTP/HTTPS (Layer 7) | ✅ HTTP proxies (`HTTP_PROXY`/`HTTPS_PROXY`) | Various | Data ingestion, forwarding to destinations (S3, Splunk, etc.) |

### On-prem Deployment Architecture

In an on-prem deployment, all Cribl components – the Leader Node and the Edge Node – are deployed and managed within your organization's network. If you choose to use a SOCKS proxy, it will exclusively control the outbound communication from your Edge Nodes to their Leader Node. 

### Cribl.Cloud/Hybrid Deployment Architecture

In a Cribl.Cloud deployment, the Leader Node is hosted and managed by Cribl within the Cribl.Cloud infrastructure. Your Edge Nodes, however, are typically deployed within your own on-prem network or private cloud. A SOCKS proxy can be used to route the outbound communication from your customer-managed Worker Nodes to the Cribl-managed Leader in the cloud.

## Configure SOCKS Proxy 

SOCKS proxy settings are applied directly to the specific Cribl Nodes (Leader, Worker, or Edge) that need to use the proxy for their outbound communication.

There are two ways to define proxy variables in Cribl Edge.

- **Environment Variables**: Ideal for most scenarios, especially when automating deployments or managing a large number of Edge Nodes.
- **UI Configuration**: Suitable for smaller deployments or when manual configuration is preferred. However, be aware of the additional steps and potential complexities involved.

### Configure via Environment Variables {#env}

 This is the most straightforward and flexible method, ideal for automated deployments, containerized environments (Docker, Kubernetes), or managing a large number of Nodes.

#### For Worker/Edge Nodes Connecting to a Leader

Use the `CRIBL_DIST_WORKER_PROXY` on each Edge Node to configure its connection to the Leader Node. 

```
CRIBL_DIST_WORKER_PROXY="socks5://<username>:<password>@<proxy_host>:<proxy_port>"
```

The default protocol is SOCKS5, which uses the format: `socks5://<username>:<password>@<host>:<port>`. Specify the SOCKS proxy host, port, and optional authentication credentials in the variable.

 * `<username>:<password>`: Optional authentication credentials for your SOCKS proxy.  
 * `<proxy_host>`: The hostname or IP address of your SOCKS proxy server.  
 * `<proxy_port>`: The port on which your SOCKS proxy server listens.  

You can alternatively specify SOCKS4 as the protocol with the format: `socks4://<proxyhost>:<port>.` To authenticate on a SOCKS4 proxy with username and password, use this format: `socks4://<username>:<password>@<proxyhost>:<port>`. The `<proxyhost>` can be a `hostname`, `IPv4`, or `IPv6`.

##### For Leader Node Connections (On-Prem Specific)

In on-prem deployments, if your Leader Node needs to route its own outbound connections (e.g., to external services like CDNs for software updates, licensing servers, or telemetry endpoints) through a web proxy, configure this using standard HTTP proxy settings. Note that Leader Nodes do not initiate connections to Edge Nodes – those connections are always initiated by the Edge Nodes to the Leader. SOCKS proxy configuration is not supported for Leader Node outbound connections to cloud services.

**Where to set environment variables:**

* **Systemd Service:** For Linux deployments, add `Environment="<VARIABLE_NAME\>=..."` under the `[Service]` section in your systemd override file (such as, `/etc/systemd/system/cribl.service.d/override.conf)`.  
* **Docker/Kubernetes:** Set as environment variables in your Dockerfile, `docker run` command, or Kubernetes Deployment/StatefulSet manifests.  
* **Directly on Host:** For manual installations, you can set it in a shell script that starts Cribl, or in a profile file (for example, `.bashrc`, `.profile`) for the user running Cribl.
  
> After setting the environment variable, restart the Cribl service on the Node for the changes to take effect.
{.box .warning}

> Config bundle downloads cannot be sent via a SOCKS proxy, but instead use
> an HTTP proxy that allows requests to the Leader or an [S3 bucket](/stream/config-bundle-management#configure-s3-storage).
{.box .info}

### Configure via the UI {#ui}

Configuring a SOCKS proxy for Cribl Edge using the UI is more complex than using environment variables due to the multi-step process involved. It requires enabling direct access to the Leader, setting up the proxy on the Edge Nodes, and then configuring the SOCKS proxy settings. Additionally, using the UI might not be suitable for all deployment scenarios, especially those with large numbers of Edge Nodes or complex network configurations.

Configuring a SOCKS proxy for Cribl Edge using the UI is suitable for individual Edge Nodes or smaller deployments where direct access to each Edge Node's UI is feasible.

To configure a SOCKS proxy for Cribl Edge, follow these steps.

1. **Enable direct access**: Allow Edge Nodes to access the Leader Node directly.
   - Cribl Edge: [Set up Leaders and Edge Nodes](/edge/setting-up-leader-and-edge-nodes/).
   - Cribl Stream: [Set up Leaders and Worker Nodes](/stream/setting-up-leader-and-worker-nodes/). 
   - Remember to enable UI Access (and teleport) to [Workers](/stream/setting-up-leader-and-worker-nodes#teleporting) or [Edge Nodes](edge/setting-up-leader-and-edge-nodes#edge-access) as applicable. 
2. **Set up Edge Nodes and SOCKS proxy**: Configure the Edge Nodes and ensure the SOCKS proxy server is running and accessible.
3. **Configure proxy details**: You can access the Edge Node via teleporting. To configure without teleporting, log in to each Edge Node directly using a local user account. 
4. Select **Edge Node Settings** from the Edge Node submenu. Navigate to **Distributed Settings** > **Leader Proxy Settings** and configure the following fields:
   - **Username**: Enter the username for the SOCKS Proxy authentication.
   - **Password**: Enter the password for the SOCKS Proxy authentication.
   - **Proxy host**: Enter the proxy server host. Can be a `hostname`, `IPv4`, or `IPv6`.
   - **Proxy port**: The port on which your SOCKS proxy server listens.
   - **Protocol version**: Version of the SOCKS protocol. Defaults to `5` for `socks5`.
5. **Save the settings**: The Edge Nodes will automatically reconnect to the Leader node using the configured SOCKS proxy.
6. **Disable direct access**: Verify the SOCKS proxy configuration is verified. To ensure all traffic between Edge Nodes and Leader is routed through the SOCKS proxy, disable the direct network connections between them. This includes removing any firewall rules or network configurations that allow direct communication.

### Best Practices & Troubleshooting

For successful SOCKS proxy implementation, consider the following.

* **Firewall Rules**: Set up your firewall rules properly for a seamless deployment. 
  * **Edge Node to SOCKS Proxy:** Allow outbound TCP from Cribl Edge Node to your SOCKS proxy on its configured `proxy_port`.  
  * **SOCKS Proxy to Leader (TCP 4200)**: Your SOCKS proxy must have outbound access to the Leader Node on TCP port 4200. For on-Prem deployments, ensure your Leader Node accepts inbound connections from the SOCKS proxy on TCP 4200. For Cribl.Cloud deployments, allow outbound access from your SOCKS proxy to the Cribl.Cloud Leader's IPs for the Network Load Balancers (found in **Workspace** > **Access** > **Leader NLB IPs**). For details, see [View Workspace Detail](workspaces#view-access-details).
* **DNS Resolution**: The SOCKS proxy server must resolve the Leader Node's hostname (on-prem Leader or `your-workspace.cribl.cloud`).  
* **Proxy Placement (On-Prem)**: Strategically place the SOCKS proxy to optimize network paths and minimize latency.  
* **Egress Control (Cribl.Cloud)**: The SOCKS proxy provides a vital, controlled egress point for Cribl Nodes' communication with Cribl.Cloud, aligning with corporate security policies.  
* **Authentication**: Securely manage and ensure correct credentials if your SOCKS proxy requires authentication.  
* **Other Proxy Types**: SOCKS proxies (`CRIBL_DIST_WORKER_PROXY`) are for communication between the Leader and Edge Node. Other outbound traffic (for example, to external Sources/Destinations) may require standard HTTP/HTTPS proxies (`HTTP_PROXY`/`HTTPS_PROXY`). For details, see [Configure System Proxy](proxy-config).


