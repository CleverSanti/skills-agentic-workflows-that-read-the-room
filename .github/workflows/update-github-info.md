---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  pull-requests: read

engine: copilot

tools:
  edit:
  web-fetch:

network:
  allowed:
    - github.blog
    - github.com

safe-outputs:
  create-pull-request:
    max: 1
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md` before drafting any changes.

Use the web-fetch tool to read both of these official sources:

- https://github.blog/latest/
- https://github.blog/changelog/

Identify recent, practical GitHub updates that are useful to developers and fit Mona's editorial angle. Update `site/content/github-info.md` with concise, factual summaries, avoid duplicating existing themes, and link to the specific source for every new item. Do not invent details or include an item unless the source supports it. Preserve the existing document's structure and other content.

When there is a meaningful update, propose the change by creating one pull request for Mona to review. Give the pull request a concise title and explain the update and its GitHub Blog or Changelog sources in the description. Do not write changes directly to the default branch. If there is nothing new and useful to add, do not create a pull request.