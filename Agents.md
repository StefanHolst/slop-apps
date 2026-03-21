# SlopOS App Repository

This repo contains apps for [SlopOS](https://github.com/slop-os/slop-os.github.io), a browser-based desktop environment. Each subdirectory under `apps/` is a self-contained app that can be installed via the SlopOS GitHub integration.

## Repository Structure

```
apps/
  my-app/
    index.html      # Required — app entry point
    meta.json       # Required — app name and icon
    style.css       # Optional
    app.js          # Optional
    icon.svg        # Optional — referenced from meta.json
    INSTRUCTIONS.md # Optional — describes storage keys and messaging protocol
```

Each app is a flat directory (no nesting). SlopOS discovers apps by listing directories under the configured path (default: `apps/`).

## meta.json

```json
{
  "name": "Short Name",
  "icon": "<svg viewBox=\"0 0 56 56\" ...>...</svg>"
}
```

- `name`: 1-2 words, displayed under the desktop icon and in the titlebar.
- `icon`: Inline SVG string (56x56 viewBox, dark background `#1a2a4a`, single colored accent, simple geometric shapes, no text). Alternatively, set `"icon": "icon.svg"` to reference a file in the same directory.

## App Conventions

### Style

- Dark theme: `#0a0e1a` background, `#0f1923` surface, `#e0e8f0` text.
- `rgba()` borders (e.g., `rgba(255,255,255,0.06)`).
- Monospace font: `'SF Mono', 'Fira Code', 'Consolas', monospace`.
- Border radius: `6px`–`8px`.
- Read an existing app (e.g., `ucal`) for exact patterns before creating a new one.

### Structure

- Simple apps: single `index.html` with inline `<style>` and `<script>`.
- Complex apps: split into `index.html`, `style.css`, `app.js`, etc. Use relative paths (`./style.css`, `./app.js`).
- Every app must have an `index.html` — this is the entry point loaded in the iframe.

### Environment

Apps run inside a sandboxed iframe. They do **not** have direct access to `localStorage` or the parent DOM. Use the provided APIs instead.

### APIs

All APIs are async and injected into the iframe by SlopOS.

**Storage** (sandboxed per app):
```js
await appStorage.getItem(key)
await appStorage.setItem(key, value)
await appStorage.removeItem(key)
await appStorage.clear()
await appStorage.keys()
```

**Cross-app storage** (user prompted to approve):
```js
const other = appStorage.foreign(appId);
await other.getItem(key)
await other.setItem(key, value)
```

**Inter-app messaging** (user prompted to approve):
```js
// Send a message to another app
const response = await appMessaging.send(targetAppId, { action, ...data });

// Receive messages
appMessaging.onMessage((fromAppId, message, reply) => {
  reply({ ... });
});
```

**Visibility** (pause animations/timers when backgrounded):
```js
window.addEventListener('message', e => {
  if (e.data?.type === 'slopos-visibility') {
    // e.data.state: 'active' | 'inactive' | 'minimized' | 'backgrounded'
  }
});
```

### INSTRUCTIONS.md

If your app exposes data via cross-app storage or accepts inter-app messages, include an `INSTRUCTIONS.md` describing:
- Storage keys and their value formats
- Message protocol (expected actions, request/response shapes)

Other apps will read this file before integrating.

## When Modifying Apps

- Always read the existing files before making changes.
- Write complete file contents — partial updates are not supported.
- Match the existing style conventions exactly. Do not guess — read a reference app first.
