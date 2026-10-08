---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  pull-requests: read

engine: copilot
model: gpt-4.1

tools:
  edit:
  bash: [curl]

network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com

safe-outputs:
  create-pull-request:
    max: 1
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md` before drafting any changes.

This workflow runs on the GitHub Copilot CLI, where `web_fetch` is not available. Use the shell tool to run one `curl` request for each URL below and read the returned page content:

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Run each request as `curl --fail --location --silent --show-error --max-time 30 URL`. Attempt each of the three requests once before deciding what to do; do not treat missing or unread source content as evidence that there is nothing to add. The workflow allowlists `curl` shell calls, and the network policy restricts outbound requests to the listed GitHub domains. Do not call `web_fetch`, use other shell commands, install tools, or access any other host. If any request fails, report the failed URL with the `noop` safe output rather than guessing or inventing details.

For each successfully fetched source, identify specific announcements or updates and verify them against the current `site/content/github-info.md`. A candidate qualifies when its source supports a concrete, practical takeaway for developers and that specific takeaway is not already covered by the page. Sharing a broad theme with existing content (such as Copilot or Actions) does not by itself make a specific new update a duplicate. Do not invent details or include an item unless the source supports it.

If one or more candidates qualify, add concise, factual summaries to `site/content/github-info.md`, preserve its existing structure and other content, and create exactly one pull request for Mona to review. Include a link to the specific source for every new item. Give the pull request a concise title and explain the updates in the description, including the publisher name and direct URL for each source used. Do not write changes directly to the default branch. Do not choose `noop` when at least one candidate qualifies. Choose `noop` only if a source request failed or all three sources were read and none of their specific updates qualifies. Always finish by emitting exactly one safe output: `create_pull_request` when any candidate qualifies, otherwise `noop` with the failed URL(s) or a brief explanation of why no candidate qualifies. Do not silently finish without a safe output.