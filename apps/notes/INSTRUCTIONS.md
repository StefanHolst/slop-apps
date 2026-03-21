# Notes app integration

This app is primarily a text editor with Files app integration.

## Local app storage

Keys used:
- `draft_v1` — JSON draft object:
```json
{
  "folder": "root",
  "name": "note.txt",
  "mimeType": "text/plain",
  "content": "hello",
  "updatedAt": 1710000000000
}
```
- `active_file_v1` — the last file opened/saved from the Files app, or `null`

## Files app dependency

This app sends messages to the Files app id:
- `mbq7qhd4mqro`

Messages used:
- `{ action: 'saveFile', file }`
- `{ action: 'getFile', folder, name }`
- `{ action: 'deleteFile', id }` or `{ action: 'deleteFile', folder, name }`

## Notes
- Content is plain text.
- MIME types currently supported in the UI: `text/plain`, `text/markdown`.
- Drafts are autosaved locally even before saving to Files.
