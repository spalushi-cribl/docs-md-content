# Work with the Cribl MCP Server


Learn how to configure and use the Cribl [MCP](https://modelcontextprotocol.io/docs/getting-started/intro) server, an AI-native way to interact with your Cribl environment.

---

## Overview {#overview}

The Cribl MCP server can help you gain insight into your Cribl environment by investigating your Sources, Destinations, Worker Groups, and system health through an [MCP client](https://modelcontextprotocol.io/docs/learn/client-concepts) of your choice. The Cribl MCP server is self-hosted and can run locally or in the cloud, depending on the needs of your organization.

## Prerequisites {#prerequisites}

- Cribl environment ([cloud, self-hosted](/stream/cloud-vs-self-hosted), or [hybrid](/stream/cloud-enterprise/#hybrid))
- MCP client (such as Cursor, Claude Desktop, GitHub Copilot, Windsurf, or LibreChat)
- Docker container (if you want to host locally)
- AWS account (if you want to host in AWS)
- Amazon ECS or Amazon ECS Anywhere (if you want to host in AWS)
- Access to port 3030

## Set Up Your Target Environment and MCP Clients {#setup}

For demonstration purposes, this guide will use a Cribl MCP server hosted locally through Docker, with Cursor as the MCP client.

Start by downloading the Cribl MCP server image. Depending on where you want to host your server, you can get the container image from Docker Hub or the AWS Marketplace. 
- [Docker Hub](https://hub.docker.com/r/cribl/cribl-mcp-server/tags)
- [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-dddpw5botcxri) 

Run the image in a container on your platform of choice. The Cribl MCP server requires certain environment variables to be set for the container, as described in the following table.

| Variable Name | Required | Description | Information Source |
| --- | --- | --- | --- | 
| CRIBL_BASE_URL | Yes | The base URL for your target Cribl environment. | [Cribl](/cribl-as-code/api/#base-urls) |
| CRIBL_CLIENT_ID | Yes | The first component of the Cribl API credential. Must be created by an admin or owner. | [Cribl API Auth](/cribl-as-code/api-auth) | 
| CRIBL_CLIENT_SECRET | Yes | The first component of the Cribl API credential. Must be created by an admin or owner. | [Cribl API Auth](/cribl-as-code/api-auth) | 
| MCP_API_KEY | No | A self-created API key to restrict access to your MCP server. Recommended if hosting in AWS or shared environments. | User created and managed | 
| aws_account_id | Yes (for AWS) | Your target AWS account ID if hosting in AWS | [AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/console-account-id.html) | 
| aws_region | Yes (for AWS) | Your target AWS region if hosting in AWS. | [AWS ECS](https://docs.aws.amazon.com/general/latest/gr/ecs-service.html) | 

When you have the information needed to populate the necessary environment variables, run your Cribl MCP server in your chosen container using one of the following terminal commands:

### Docker (local)

```
docker run -p 3030:3030 \
  -e CRIBL_BASE_URL=<https://your-org.cribl.cloud> \
  -e CRIBL_CLIENT_ID=<your-client-id> \
  -e CRIBL_CLIENT_SECRET=<your-client-secret> \
  -e MCP_API_KEY=<your-secure-api-key> \
  cribl/cribl-mcp-server
```

### AWS ECS

```
docker run -p 3030:3030 <aws_account_id>.dkr.ecs.<aws_region>.amazonaws.com/cribl/cribl-mcp-server
-e CRIBL_BASE_URL=<https://your-org.cribl.cloud> 
-e CRIBL_CLIENT_ID=<your-client-id>
-e CRIBL_CLIENT_SECRET=<your-client-secret>
-e MCP_API_KEY=<your-secure-api-key> 
cribl-mcp
```

You should see the following responses from your container hosting platform, confirming that your Cribl MCP server is successfully running:

```
[2025-11-04T17:00:45.965Z] INFO  Configuration validated successfully {"baseUrl":"<https://your-org.cribl.cloud>"}
[2025-11-04T17:00:45.966Z] INFO  Registering MCP tools...
[2025-11-04T17:00:45.966Z] INFO  MCP tools registered successfully
[2025-11-04T17:00:45.969Z] INFO  MCP Stateless Streamable HTTP Server listening on port 3030
```

## Configure Your MCP Client {#configure}

The Cribl MCP server is treated as a local MCP server, even if it is running in an ECS instance. For your chosen MCP client, add the following Cribl MCP server details to the client’s mcp.json file:

```
{
  "mcpServers": { 
    "cribl-mcp-remote": { 
      "command": "npx", 
      "args": [ "-y", "mcp-remote", "http://localhost:3030/mcp", "--transport", "http" ] 
      } 
    }
}
```

After successful configuration, your client should surface four available tools for the Cribl MCP Server: `cribl_getSystemMetrics`, `cribl_listWorkerGroups`, `cribl_getSources`, and `cribl_getDestinations`. You can access each of these tools through your MCP client to gain insights on and interact with your Cribl environment. 

## Use Case Examples {#use-cases}

The Cribl MCP server provides an AI-native approach to interacting with your Cribl environment. When evaluating how to best use your Cribl MCP, consider the following use cases as a reference.

| Use Case | Sample Prompt | Value | 
| --- | --- | --- | 
| Cribl environment health | “What’s the current health of my Cribl environment? Highlight any areas of concern or action items I need to consider.” | Coalesces health information across your Cribl environment into a digestible summary with clear action items. 
| Cribl Source and Destination overview | “What Sources and Destinations do I have in my environment? Are there any I should pay specific attention to?” | Provides a comprehensive overview of active and unused Sources and Destinations across your environment. | 
| Cribl Worker Group overview | “What’s the processing status of my Worker Groups?” | Provides cross-worker group visibility into key items like throughput, health, and configuration settings, with improvement recommendations. | 