---
name: update-github-info
description: Refresh Mona's GitHub Info page with practical, sourced updates from GitHub.
intent: Keep Mona's GitHub Info page concise, practical, and current with verified official GitHub Blog and Changelog updates.
on:
  schedule: daily
  workflow_dispatch:
  skip-if-match: 'is:pr is:open in:title "[GitHub Info]"'
permissions:
  contents: read
  pull-requests: read
strict: true
engine:
  id: copilot
  copilot-sdk: true
tools:
  edit: true
  web-fetch: {}
  bash: false
  cli-proxy: false
network:
  allowed:
    - github.blog
    - github.com
safe-outputs:
  create-pull-request:
    base-branch: main
    reviewers: [mona]
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and the current `site/content/github-info.md`. Fetch
`https://github.blog/latest/` and `https://github.blog/changelog/` on every run.

Select only recent, verified updates that help developers learn GitHub faster.
Keep summaries short and practical, link each update to its original Blog or
Changelog source, and preserve the page's existing useful content and style.
Edit only `site/content/github-info.md`; do not infer facts that the sources do
not support.

If neither source contains a suitable new update, or the evidence is uncertain,
make no changes and call `noop` with a brief reason. Otherwise, propose the
page update through `create-pull-request` with a concise `[GitHub Info]` title
and summary, requesting review from Mona. Never write directly to `main`.
