---
name: make-it-reviewable
description: Publish one explicitly selected, self-contained HTML artifact to Reviewable through its MCP and return the private owner URL. Use only when the user asks to make, upload, or publish an artifact to Reviewable for human review.
---

# Make it reviewable

Publish only on an explicit user request. Do not publish after generating an artifact unless the user separately asks.

## Resolve one artifact

1. Prefer the HTML path the user named.
2. Otherwise use exactly one clearly task-local HTML artifact created in this task.
3. If there is no candidate, more than one plausible candidate, an unreadable path, or an external URL, ask the user to choose or provide the file. Do not substitute a handoff, transcript, or another workspace artifact.

Accept only a readable `.html` or `.htm` file. Structured Slides are valid only when already wrapped in Reviewable's supported HTML envelope. Reject PDFs, presentations, Word documents, images, archives, loose JSON, and URLs. Ask the user to export or generate a self-contained HTML file instead; do not convert it yourself.

## Preflight before any MCP write

Confirm that the HTML is self-contained:

- Keep fetched render assets inline, as data URLs, or fragment references.
- Allow an `<img src>` that points to an unavailable local or remote image. Reviewable replaces it with an inert missing-image placeholder and does not fetch it. Reject `srcset`.
- Allow inline startup scripts in ordinary HTML. Reject external script sources, external stylesheets, iframes, embeds, fonts, CSS `@import`, and render-time `url(...)` fetches.
- For Structured Slides, allow only the supported JSON envelope script. Reject active scripts.
- Preserve `data-review-id` anchors when the artifact uses Reviewable review anchors.

If any check fails, tell the user what must change and stop. Do not call `create_review`.

## Connect and publish

1. Call `get_connection_state`.
2. If `workspace_create` is unavailable, call `start_web_authorization`, give the owner its approval URL and verification code, then call `check_authorization` after approval. Do not ask for or expose a workspace key.
3. Call `create_review` with the HTML, original filename, and one idempotency key for this publish attempt.
4. On a transient failure, retry only with the same idempotency key. Do not retry a validation or authorization denial as a write.

Return the `owner_review_url` only. Say that the owner can review it and share with colleagues from Reviewable. Never create, request, infer, or reveal a reviewer/share URL.
