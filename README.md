# Zendesk Auto Translate — Macros

This repository stores the shared macro library used by the [Zendesk Auto Translate Chrome extension](https://github.com/PsycoStea/zendesk-auto-translate-chrome-extension) for the Mac Group Global support team.

## Layout

```
macros/
  <slug>.json        — one file per macro
README.md
```

Each macro file looks like:

```json
{
  "name": "order-cancelled",
  "body": "<p>Hi {{ticket.requester.first_name}}</p><p>Thanks for the email...</p>",
  "updated": 1730500000000,
  "attachments": []
}
```

- `name` — display name shown in the `//` autocomplete dropdown.
- `body` — HTML body. Supports the same formatting the in-app macros editor produces (bold, italic, underline, lists, links, blank lines).
- `updated` — Unix milliseconds. Used for last-write-wins merging on sync.
- `attachments` — reserved for the future PDF-attachment feature; currently always `[]`.

## How sync works

The extension manages this repo through the GitHub Contents API using a personal access token entered into the extension's macros management page. Pull merges remote macros into local storage by `updated` timestamp; push uploads local changes to the matching remote files. There is no automatic background sync — the user clicks "Pull" or "Push" explicitly.

## Editing here directly

Editing files in this repo via the GitHub UI is supported and intended — agents can collaborate on copy without all needing the extension installed. Just keep the JSON shape valid and bump `updated` to the current Unix-ms timestamp so the change wins the merge.
