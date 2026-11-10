# Azahar Web — mirror

Browser Nintendo 3DS emulator, mirrored here so x3e serves it from its own origin
instead of hotlinking `shubin123.github.io/azahar_web`.

## Upstream

- Wrapper / web frontend: <https://github.com/Shubin123/azahar_web> (the `web/` folder, commit `main`)
- Emulator core: [Azahar](https://github.com/azahar-emu/azahar), a Citra fork — GPL-2.0-or-later.
  The wasm builds here come from `Shubin123/azahar_emscripten`, which is a private sibling repo,
  so the corresponding source for this build is not public. If you redistribute this mirror,
  keep this file with it and point people at upstream.

## What was copied

Everything from upstream `web/` except `server.cjs` and `lan_saves.cjs` (Node-only local dev
server, useless on GitHub Pages). Files are unmodified; all paths inside are relative, so the
folder works at any URL.

## Why it must open as a top-level page

The engine uses `SharedArrayBuffer`, which browsers only expose to a cross-origin isolated
document (`Cross-Origin-Opener-Policy: same-origin` + `Cross-Origin-Embedder-Policy: require-corp`).
GitHub Pages cannot send headers, so `azahar_ui.js` registers `coi-serviceworker.js` at scope
`./` and reloads once with those headers added. Isolation is inherited from every ancestor
document, so this never works inside x3e's game overlay iframe — hence the new-tab tile.

The first load reloads once while the worker takes control. That is expected.

## ROMs

No ROMs are included. Load a decrypted `.3ds` from your device, or use the page's built-in
"Shared Game Library" (it queries Internet Archive; nothing is hosted here).
