# ONES MCP for GitHub Copilot

Connect GitHub Copilot to ONES to manage projects and work with knowledge in your authorized workspace. This remote MCP server and this repository are maintained by ONES.

## Capabilities

- Find and manage projects, issues, sprints, and comments.
- Work with test cases and work-hour records.
- Create and retrieve Wiki knowledge and read supported attachments.

Available tools and actions depend on your account permissions and enabled ONES capabilities.

## Requirements

- VS Code with GitHub Copilot and MCP support enabled.
- An ONES account with access to a workspace served by `https://us.ones.com/mcp`.
- Permission to use MCP servers under your organization's policies.

This endpoint serves the US deployment. Other regional or private deployments may require a different MCP endpoint. Check your workspace's MCP settings before connecting.

## Connect in VS Code

1. Open the Command Palette and run **MCP: Add Server**.
2. Select **HTTP** and enter `https://us.ones.com/mcp`.
3. Name the server `ones`. For a new workspace connection, choose `.mcp.json`; for access across projects, choose the global configuration option offered by your client.
4. Start the server and confirm trust when prompted. Complete the browser-based OAuth authorization: sign in to ONES and review the requested access.
5. Open Copilot Chat in an agent mode that supports tools. Use **Configure Tools** to confirm that the ONES tools are available.

Alternatively, create `.mcp.json` in your own project's root using the following configuration. If the file already exists, merge the `ones` entry into its `mcpServers` object:

```json
{
  "mcpServers": {
    "ones": {
      "type": "http",
      "url": "https://us.ones.com/mcp"
    }
  }
}
```

Copyable examples: [portable-mcp.json](portable-mcp.json) uses the `.mcp.json` format above; [vscode-mcp.json](vscode-mcp.json) uses the `servers` format for existing `.vscode/mcp.json` setups. Configure the server in one location to avoid duplicate entries. Cloning this repository or running a local ONES server is not required.

ONES uses OAuth authorization. Do not put passwords, access tokens, or client secrets in configuration files committed to Git. Access remains subject to the authorized ONES user's permissions.

## Verify the connection

After saving the configuration, run **MCP: List Servers**, select `ones`, and start the server if needed. Complete the browser authorization with the ONES account you intend to use, then return to VS Code. In Copilot Chat, open **Configure Tools** and check that ONES tools are available and enabled.

Send this first request:

> Use ONES to list the projects I can access. Do not change any data.

A successful check returns actual accessible project results, or an explicit empty result if the account has no accessible projects. A generic answer about ONES does not verify a tool call: expand Copilot's tool activity and confirm that an ONES tool ran successfully. If authorization or a tool call fails, use the troubleshooting steps below.

## Use ONES in Copilot Chat

Choose a chat mode that supports tools and enable the relevant ONES tools. State the project, issue, or Wiki page you want to work with. When names are ambiguous, ask Copilot to show matching records before proceeding. Replace angle-bracket placeholders below with your own values.

### Find and summarize work

> Use ONES to find the project named `<project name>`. If multiple projects match, let me choose. Then list the issues assigned to me in that project, including their titles and statuses. Do not modify anything.

Use the returned issue identifiers for follow-up requests:

> Retrieve ONES issue `<issue ID>` and summarize its description, status, and comments.

### Retrieve workspace knowledge

> Search my authorized ONES Wiki content for `<release process>`. Show the matching page titles, retrieve the relevant page, and summarize the release steps. Include the source page link if one is returned.

Results depend on the Wiki content the authorized account can access.

### Create an issue

> In ONES project `<project name>`, prepare an issue titled `<title>` with this description: `<description>`. Ask me for any required fields that are missing, and show the target project, issue type, and proposed fields before creating it.

Review the proposed values, then ask Copilot to create the issue. If VS Code requests tool approval, review the operation before approving it. Check the returned issue identifier and open the issue in ONES to verify the saved result. Use a test project when evaluating write operations.

### Update an existing issue

> Retrieve ONES issue `<issue ID>`. Show its current status and the available target statuses, then ask me which status to use before updating it.

Check the resulting record in ONES after the update. Available operations and required fields vary with workspace configuration and account permissions.

## Disconnect

Use **MCP: List Servers** to stop the `ones` server. To remove the connection configuration, delete only its entry from the configuration file you selected. Stopping or removing the client configuration does not itself revoke an existing OAuth grant; manage that authorization separately in your ONES account settings.

## Troubleshooting

- **Authorization fails:** confirm that your ONES account and workspace belong to the configured deployment, then reconnect and complete OAuth again.
- **No tools appear:** check the server status with **MCP: List Servers**, review its output, and check the tool selection in Copilot Chat.
- **An operation is denied:** confirm your ONES permissions and the workspace's enabled capabilities with an administrator.
- **MCP is restricted:** ask your organization's administrator whether this server is allowed.

For reproducible integration problems, [open an issue](https://github.com/ONES-com/ones-github-mcp/issues) with the client version and a sanitized error message. Never include tokens, credentials, or private workspace data.

## Registry metadata

The server's Official MCP Registry identifier is `com.ones/ones`. [server.json](server.json) contains the metadata prepared for publication, including this public repository URL. A file in this repository is not evidence that the corresponding version has been published or approved by GitHub; check the [live Registry record](https://registry.modelcontextprotocol.io/v0.1/servers/com.ones%2Fones/versions/latest) for the published version.

This repository provides public documentation and connection metadata for the hosted ONES MCP service. It does not contain the hosted service's backend source code.

## Links

- [ONES](https://ones.com)
- [ONES MCP overview](https://ones.com/features/mcp)
- [VS Code MCP setup documentation](https://code.visualstudio.com/docs/agent-customization/mcp-servers)
- [GitHub MCP Registry publishing guide](https://github.blog/ai-and-ml/generative-ai/how-to-find-install-and-manage-mcp-servers-with-the-github-mcp-registry/)
