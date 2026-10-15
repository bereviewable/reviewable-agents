# Install Reviewable in Cline

Add the Reviewable stdio server and name it `reviewable`. The command is `npx -y reviewable-artifacts-mcp`. The configuration takes no workspace key.

## Requirements

Use Node.js 22.13 or newer. That is the runtime the published `reviewable-artifacts-mcp` package requires. `npx` must be on `PATH`.

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

The first time your agent needs to create a review, it opens Reviewable browser authorization. Approve the scoped, revocable grant in the browser. Do not paste a workspace key into chat or into this configuration. You receive a private owner URL. Sharing that review stays your explicit decision inside Reviewable.

Adding the server publishes nothing on its own.
