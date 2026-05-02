# Zendesk Auto Translate — Macros

This repository stores the shared macro library used by the [Zendesk Auto Translate Chrome extension](https://github.com/PsycoStea/zendesk-auto-translate-chrome-extension) for the Mac Group Global support team.

## Layout

```
macros/
  <slug>.json          — one file per macro (metadata + body)
  <slug>/              — folder per macro holding its attachments
    <filename>.pdf     — one binary file per attachment
README.md
```

Each macro JSON looks like:

```json
{
  "name": "order-cancelled",
  "body": "<p>Hi {{ticket.requester.first_name}}</p><p>Thanks for the email...</p>",
  "updated": 1730500000000,
  "attachments": [
    { "name": "return-form.pdf", "size": 12345, "type": "application/pdf" }
  ]
}
```

- `name` — display name shown in the `//` autocomplete dropdown.
- `body` — HTML body. Supports the formatting the in-app macros editor produces (bold, italic, underline, lists, links, blank lines).
- `updated` — Unix milliseconds. Used for last-write-wins merging on sync.
- `attachments` — list of PDFs that get auto-attached to the reply when the macro is inserted. Each entry references a file at `macros/<slug>/<filename>`.

## How sync works

The extension manages this repo through the GitHub Contents API.

- **Pull** is anonymous — anyone can click "Pull" in the macros page to grab the latest macros and their attachments. No GitHub account or token required.
- **Push** uses a personal access token entered into the extension's macros page. Only the team lead needs one. Push uploads changed JSON files and PDFs, and removes anything that's been deleted locally.
- There is no automatic background sync — the user clicks "Pull" or "Push" explicitly.

## Editing here directly

Editing macro JSON via the GitHub UI is supported and intended — you can collaborate on copy without all needing the extension installed. Just keep the JSON shape valid and bump `updated` to the current Unix-ms timestamp so the change wins the merge. Adding/removing PDFs by hand requires committing the binary file under `macros/<slug>/` and updating the JSON's `attachments` array to match.
