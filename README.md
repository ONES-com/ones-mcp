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
3. Name the server `ones` and choose the desired configuration scope.
4. Start the server and confirm trust when prompted. Complete the browser-based OAuth authorization: sign in to ONES and review the requested access.
5. Open Copilot Chat in an agent mode that supports tools. Use **Configure Tools** to confirm that the ONES tools are available.

Alternatively, merge the following entry into your workspace's `.vscode/mcp.json` (preserve any existing servers):

```json
{
  "servers": {
    "ones": {
      "type": "http",
      "url": "https://us.ones.com/mcp"
    }
  }
}
```

The same configuration is provided in [vscode-mcp.json](vscode-mcp.json). For a portable workspace configuration, use [.mcp.json format](portable-mcp.json) at the root of your own project. Cloning this documentation repository is not required to connect.

ONES uses OAuth authorization. Do not put passwords, access tokens, or client secrets in configuration files committed to Git. Access remains subject to the authorized ONES user's permissions.

## Example prompts

- "List the ONES projects I can access."
- "Show the issues assigned to me in the project I select."
- "Search ONES Wiki for our release process and summarize the results."
- "Create an issue in the project I select, using the title and description I provide."

Start with a read-only request to verify the connection. Before approving a write action, review the target workspace, project, and proposed changes. Use a test workspace when evaluating write operations.

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
