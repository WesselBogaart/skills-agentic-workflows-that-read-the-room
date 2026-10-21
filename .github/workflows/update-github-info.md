---
name: update-github-info
description: Daily refresh of Mona's GitHub Info page from the GitHub Blog, Changelog, and Awesome Copilot workflows, proposed as a pull request.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
tools:
  edit:
  web-fetch:
  github:
    toolsets: [repos]
network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    title-prefix: "[github-info] "
    labels: [automation]
    if-no-changes: ignore
    close-older-pull-requests: true
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Refresh `site/content/github-info.md` with recent GitHub news and propose the change as a pull request for Mona to review. Never push to `main`.

## Steps

1. Read `notes/mona-notes.md` with the GitHub repository file tools (not the terminal or CLI) and follow its editorial guidance.
2. Read the current `site/content/github-info.md` the same way.
3. Fetch https://github.blog/latest/ with web-fetch.
4. Fetch https://github.blog/changelog/ with web-fetch.
5. Fetch https://awesome-copilot.github.com/workflows/ with web-fetch.
6. Edit `site/content/github-info.md`:
   - Keep the existing heading structure and tone.
   - Add or refresh a short, practical summary of recent stories worth watching.
    - Name the source (GitHub Blog, GitHub Changelog, or Awesome Copilot workflows) for every item, with a link to the original page.
   - Keep entries concise and useful for developers learning GitHub.
7. Open a pull request with the change using the `create-pull-request` safe output. Explain in the body what changed and which sources were used.

## Constraints

- Only modify `site/content/github-info.md`.
- Use only content from the three fetched pages; do not invent news or links.
- Call `noop` with a short reason if the fetched pages yield nothing new relative to the current file, or if a fetch fails.

## Usage

Runs daily, or on demand with `gh aw run update-github-info`. Mona reviews and merges the resulting pull request.
