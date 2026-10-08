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

Run each request as `curl --fail --location --silent --show-error --max-time 30 URL`. The workflow allowlists only `curl` shell calls, and the network policy restricts outbound requests to the listed GitHub domains. Do not call `web_fetch`, use other shell commands, install tools, or access any other host. If a request fails, stop and report the URL with the `noop` safe output rather than guessing or inventing details.

Identify recent, practical GitHub updates that are useful to developers and fit Mona's editorial angle. Update `site/content/github-info.md` with concise, factual summaries, avoid duplicating existing themes, and link to the specific source for every new item. Do not invent details or include an item unless the source supports it. Preserve the existing document's structure and other content.

When there is a meaningful update, propose the change by creating one pull request for Mona to review. Give the pull request a concise title and explain the update in the description, including the publisher name and direct URL for each source used. Do not write changes directly to the default branch. If there is nothing new and useful to add, do not create a pull request.