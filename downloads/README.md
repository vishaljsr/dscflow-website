# Agent installer (served at `/downloads/`)

Place **`DSCFlowAgentSetup.exe`** here so the landing-page Download button serves the real Windows installer.

## Sync from local-agent build

```bash
# From repo: build installer (local-agent), then:
cd web
npm run sync:installer
```

This copies:

`local-agent/dist/installer/DSCFlowAgentSetup.exe` → `web/public/downloads/DSCFlowAgentSetup.exe`

and writes `DSCFlowAgentSetup.exe.sha256`.

`predev` / `prebuild` run the same sync (non-strict). Use `npm run sync:installer:strict` in CI/release when the binary must be present.

## Why a 2 KB “.exe” is wrong

If this file is missing, Vite preview / SPA nginx `try_files` returns **`index.html`** (~2 KB, `text/html`) for `/downloads/DSCFlowAgentSetup.exe`. The browser still saves it as an application, and Windows reports *The file or directory is corrupted and unreadable.*

A real installer is on the order of **~30 MB**.

## Git

The `.exe` is gitignored (large binary). Commit only this README / `.gitkeep`. Deploy pipelines must run `sync:installer` (or upload to CDN and set `VITE_AGENT_DOWNLOAD_URL`).
