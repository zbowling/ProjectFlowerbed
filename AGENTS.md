# Agent Instructions — Project Flowerbed

WebXR immersive gardening showcase built on Three.js (Meta fork). ECS architecture via `ecsy`, runs in the Quest Browser.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup and instructions
- `package.json` — Node / Yarn dependencies and scripts (note: depends on a Meta fork of Three.js, not upstream)
- `webpack.config.js` — dev server / build configuration
- `.gitattributes` — text/binary handling
- `LICENSE` — license terms

## Quest / Horizon-specific notes

- WebXR requires HTTPS. The dev server is HTTPS by default; do not switch it to plain HTTP for "simpler" testing — Quest Browser will refuse to enter immersive mode.
- To test on a headset over USB, run `adb reverse tcp:8081 tcp:8081` and load `https://localhost:8081/` in the Quest Browser.
- Source assets under `content/` must be re-run through the matching `yarn run compress:*` script before changes show up in `src/assets/` (which is what the runtime actually loads). Some assets exist only in `src/assets/` with no `content/` source.
- The `server/` directory is a disabled AWS Lambda prototype, not live infrastructure.

## Meta Quest tooling

This repository is part of the Meta Quest / Horizon OS ecosystem (a sample, library, template, or related project — the bespoke intro above describes which). Use that intro and the source-of-truth files it references for project-specific decisions; don't restate or invent facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic WebXR answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including WebXR-specific skills: [github.com/meta-quest/agentic-tools](https://github.com/meta-quest/agentic-tools). Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
