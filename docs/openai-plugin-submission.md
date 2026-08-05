# Official plugin directory submission kit

Use this material when a human submits the Reviewable plugin to OpenAI's public Plugins Directory. This work prepares discovery only; the repository marketplace distribution and plugin version stay unchanged.

## Listing copy

| Field | Value |
| --- | --- |
| Plugin name | Reviewable |
| Short description | Publish self-contained HTML artifacts for human review. |
| Long description | Reviewable adds an explicit `make-it-reviewable` skill and a local MCP server for one selected, self-contained HTML artifact. It publishes only after an explicit request, checks that the artifact can render without external dependencies, and uses browser authorization when needed. |
| Category | Productivity |
| Website | `https://bereviewable.com` |
| Privacy policy | `https://bereviewable.com/privacy` |
| Terms | `https://bereviewable.com/terms` |
| Logo | `reviewable-agent-plugin/assets/logo.svg` |

### Starter prompts

- Make the HTML artifact I selected reviewable.
- Publish this self-contained HTML artifact to Reviewable for human review.
- I want to make this named HTML artifact reviewable.

The descriptions and prompts above are limited to the behavior stated in `reviewable-agent-plugin/skills/make-it-reviewable/SKILL.md` and the repository READMEs.

## Codex manifest audit

The published release tracked by this bundle is `reviewable-artifacts-mcp` **0.3.1** (checked from npm on 2026-08-05). The same version remains in:

- `reviewable-agent-plugin/.codex-plugin/plugin.json`
- `reviewable-agent-plugin/.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`

The OpenAI packaging guide makes `.codex-plugin/plugin.json` the required manifest and names `skills`, `mcpServers`, `interface`, publisher metadata, and presentation assets as the relevant package fields. It specifies plugin-root-relative `./` paths and recommends storing visual assets under `./assets/`. See [Package your plugin](https://developers.openai.com/plugins/build/plugins#plugin-structure) and its [manifest fields](https://developers.openai.com/plugins/build/plugins#manifest-fields).

| Manifest field | Action in this PR | Documentation basis |
| --- | --- | --- |
| `repository` | Added with the public source repository. | The packaging guide lists `repository` as publisher and discovery metadata. |
| `interface.capabilities` | Changed from `Interactive`/`Write` to `Read`/`Write`, which matches the skill's read-only connection checks and its explicit review creation action. | `capabilities` is the documented install-surface capability field; the official complete manifest shows `Read` and `Write`. |
| `interface.defaultPrompt` | Replaced the single vague prompt with two explicit, realistic starter prompts. | The guide names `defaultPrompt` as the starter-prompt field. |
| `interface.brandColor` | Added `#184A45`. | The guide names `brandColor` as install-surface presentation metadata. |
| `interface.composerIcon` | Added `./assets/composer-icon.svg`. | The guide names `composerIcon` and requires relative plugin-root paths. |
| `interface.logo` | Added `./assets/logo.svg`. | The guide names `logo` and recommends `./assets/`. |
| `interface.screenshots` | Added `./assets/reviewable-artifact.svg`. | The guide names `screenshots` and recommends `./assets/`. |

`mcpServers: "./.mcp.json"` is retained: the packaging guide explicitly permits an `.mcp.json` bundle, including a wrapped `mcpServers` map. `skills: "./skills/"` is retained because the bundle contains the named skill. No `apps` field was added: it is only for a registered MCP connection mapping in `.app.json`, which this static bundle does not contain.

## Assets added

All three assets are original SVGs created for Reviewable; they contain no third-party imagery or marks.

- `reviewable-agent-plugin/assets/composer-icon.svg`
- `reviewable-agent-plugin/assets/logo.svg`
- `reviewable-agent-plugin/assets/reviewable-artifact.svg`

The last file is a product illustration for the screenshot slot. It deliberately contains no real artifact, account data, or private review link.

## Human portal checklist

Choose the submission type before opening a draft. The current plugin bundle contains a local stdio MCP command (`npx`); it does not contain a remotely accessible MCP server URL.

### Route A: Skills only

Choose **Skills only** if the public listing is meant to publish the reusable `make-it-reviewable` workflow itself. Upload the final skill bundle from `reviewable-agent-plugin/skills/make-it-reviewable/`. Before submitting, verify in the portal that this route presents the expected installation behavior for a skill that depends on the existing local Reviewable MCP; the official guide does not state that an uploaded skills-only bundle also installs this repository's `.mcp.json` configuration.

### Route B: With MCP

Choose **With MCP** only after a separately built and deployed remote MCP server is available at a public production URL. The portal scans that server, so the local `npx` command cannot be used as its URL. This route additionally requires domain verification, accurate tool annotations, authentication details, and reviewer-ready demo access when authentication is required.

### Common portal work

1. Use an OpenAI Platform organization where the submitter has **Apps Management: Write** (organization owners already have it).
2. In that same organization, verify the publisher identity. Choose **business verification** for Reviewable as the public publisher, or individual verification only if publishing under an individual's own name. The selected identity must match the public name, website, support contact, privacy policy, and terms.
3. Paste the listing copy above, upload the included logo, select **Productivity**, and provide a public support URL that matches the verified Reviewable publisher. This repository does not invent a support URL.
4. Add at least five positive and three negative test cases, choose only countries or regions where the product, support process, and legal terms are ready, write release notes, complete the policy attestations, and submit for review.

The authoritative process and checklist are [Submit plugins](https://developers.openai.com/plugins/deploy/submission): see [access and identity](https://developers.openai.com/plugins/deploy/submission#before-you-submit), [required materials](https://developers.openai.com/plugins/deploy/submission#prepare-required-materials), [MCP setup and domain verification](https://developers.openai.com/plugins/deploy/submission#MCP), and [test cases and final checklist](https://developers.openai.com/plugins/deploy/submission#testing).

### Test-case outline for the portal

Use only reviewer-accessible, non-sensitive artifacts and accounts. These scenarios are grounded in the shipped skill:

| Type | Scenario | Expected behavior |
| --- | --- | --- |
| Positive | Explicitly select one readable, self-contained `.html` file. | Check connection state, obtain browser authorization only if needed, then create one review. |
| Positive | Explicitly select one readable, self-contained `.htm` file. | Treat it as an eligible HTML artifact and follow the same explicit publish flow. |
| Positive | Re-run a transiently failed publish attempt. | Retry only with the same idempotency key. |
| Positive | Select a valid artifact that contains preserved `data-review-id` anchors. | Keep the anchors intact before the publish action. |
| Positive | Publish after a scoped browser authorization has been approved. | Continue with the creation action without requesting a workspace key in chat. |
| Negative | Ask to publish without an explicit request. | Do not publish. |
| Negative | Select a PDF, image, presentation, archive, loose JSON file, or external URL. | Decline and ask for a self-contained HTML file; do not convert it. |
| Negative | Select HTML that depends on external scripts, stylesheets, fonts, embeds, images, or render-time fetches. | Stop before any create action and explain that the artifact must be self-contained. |

## Required private-copy synchronization

The canonical private copy lives at `integrations/reviewable-agent-plugin/`. In the same change there, add the exact three files listed in **Assets added**, mirror the `.codex-plugin/plugin.json` metadata edits, and update `tests/agentPluginDistribution.test.mjs` so its `expectedFiles` list includes:

- `assets/composer-icon.svg`
- `assets/logo.svg`
- `assets/reviewable-artifact.svg`

Keeping the private copy and that test list in lockstep is required; this public repository must not contain application code, test data, credentials, or private review links.
