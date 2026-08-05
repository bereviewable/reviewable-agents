# Reviewable agents

Public distribution for the Reviewable agent plugin: the explicit `make-it-reviewable` action and the Reviewable stdio MCP, installed together.

This repository contains only the plugin bundle and its marketplace manifests. Reviewable's application source is not here.

## Install

### Claude Code

```sh
claude plugin marketplace add bereviewable/reviewable-agents
claude plugin install reviewable-agent-plugin@reviewable
```

Then run `/reload-plugins`.

### Codex

```sh
codex plugin marketplace add bereviewable/reviewable-agents
codex plugin add reviewable-agent-plugin@reviewable
```

Then reload plugins.

### Any other MCP host

The plugin is a convenience, not a requirement. Add a local stdio MCP server named `reviewable` that runs:

```sh
npx -y reviewable-artifacts-mcp
```

That alone is a complete installation. To also get the named action, copy `reviewable-agent-plugin/skills/make-it-reviewable/` into your host's Agent Skills mechanism.

## What installation does

It registers a local MCP server and one skill. It adds no service, stores no credential, and publishes nothing on its own.

The first time your agent needs to create a review, it opens Reviewable browser authorization and you approve a scoped, revocable grant. You never paste a workspace key into chat or configuration. You receive a private owner URL; sharing stays your explicit decision inside Reviewable.

Full setup instructions: [bereviewable.com/agents](https://bereviewable.com/agents).

## Versioning

The plugin version tracks the published `reviewable-artifacts-mcp` npm release. Releases are tagged.

## License

MIT
