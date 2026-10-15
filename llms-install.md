# Install Reviewable in Cline

Add the Reviewable stdio server and name it `reviewable`. The command is `npx -y reviewable-artifacts-mcp`. The configuration takes no workspace key.

## Requirements

Use Node.js 20 or newer. That is the runtime the published `reviewable-artifacts-mcp` package requires. `npx` must be on `PATH`.

## Add the server

Merge the `reviewable` entry into the existing `mcpServers` object. Leave every other server in place.

```json
{
  "mcpServers": {
    "reviewable": {
      "command": "npx",
      "args": ["-y", "reviewable-artifacts-mcp"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

In the Cline IDE extension, open **MCP Servers**. Choose **Configure**. Choose **Configure MCP Servers**. Cline opens the MCP settings JSON.

For Cline CLI, write the same object to `~/.cline/data/settings/cline_mcp_settings.json`, or run:

```sh
cline mcp add reviewable --yes -- npx -y reviewable-artifacts-mcp
```

## Authorize

When access is needed, the agent calls `start_web_authorization` and prints an approval URL and a short verification code. Open the URL. If this browser has no workspace key, create one and save the one-time recovery copy. Type the code your agent printed (the page never shows it), choose a scope, and approve. Then the agent calls `check_authorization`. Do not paste a workspace key into chat or into this configuration. You receive a private owner URL. Sharing that review stays your explicit decision inside Reviewable.

Adding the server publishes nothing on its own.
