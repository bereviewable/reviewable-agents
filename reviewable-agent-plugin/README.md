# Reviewable agent plugin

This static bundle installs the existing Reviewable stdio MCP and the explicit `make-it-reviewable` skill together. It adds no service, credentials, or automatic publishing.

The bundle is distributed from Reviewable's public plugin channel. Setup instructions for every harness live at [bereviewable.com/agents](https://bereviewable.com/agents).

## Codex

Add the Reviewable plugin from the Reviewable channel, then reload plugins. The bundle registers `npx -y reviewable-artifacts-mcp` and exposes `make-it-reviewable`.

## Claude Code

Install the Reviewable plugin from the Reviewable channel, then run `/reload-plugins`. Claude discovers `.mcp.json` and `skills/make-it-reviewable/` from the plugin bundle.

## Compatible MCP hosts

The plugin is a convenience, not a requirement. Add the `reviewable` stdio server with `npx -y reviewable-artifacts-mcp` and the same authorized tools are available; asking in plain language does what the named action does. To also get the named action, copy the `skills/make-it-reviewable/` folder into that host's Agent Skills mechanism.

## First publish

Ask the agent to make a named HTML artifact reviewable. The agent asks the owner to approve `workspace_create` in a browser only when it first needs to create a private review. The owner URL is private; share reviews from Reviewable after opening it.
