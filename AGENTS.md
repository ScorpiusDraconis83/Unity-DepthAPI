# Agent Instructions — Unity Depth API

Two parallel Unity sample projects (BiRP and URP) demonstrating the Meta Depth API for dynamic occlusion of virtual content by real-world geometry on Quest 3-class hardware.

## Source-of-truth files (read these first, do not duplicate their contents in this file)

For setup, build steps, SDK versions, and project layout, read:

- `README.md` — official setup, occlusion shader integration guide, upgrade notes, troubleshooting
- `DepthAPI-BiRP/ProjectSettings/ProjectVersion.txt` and `DepthAPI-URP/ProjectSettings/ProjectVersion.txt` — Unity editor version
- `DepthAPI-BiRP/Packages/manifest.json` and `DepthAPI-URP/Packages/manifest.json` — Unity package versions
- `Packages/com.meta.xr.depthapi/` and `Packages/com.meta.xr.depthapi.urp/` — the shipped helper packages (also installable by git URL)
- `.gitattributes` — Git LFS (required)
- `LICENSE` — license terms

## Quest / Horizon-specific notes

- Quest 3 / 3S only — Depth API does not run on Quest 2 or Quest Pro.
- Two separate Unity projects in the repo root (BiRP, URP). Open one at a time; do not open the repo root.
- Core Depth API moved into Meta XR Core SDK in v67; the `com.meta.xr.depthapi*` packages here now contain only support shaders. `EnvironmentDepthTextureProvider` + `EnvironmentDepthOcclusionController` were merged into `EnvironmentDepthManager`. Custom-shader include paths changed accordingly — do not write code against the deprecated classes or old paths.
- Requires Vulkan + Multiview + `com.oculus.permission.USE_SCENE`. The Project Setup Tool (PST) is the expected way to enforce this — if Depth API "doesn't work," check these first.
- For raycasting against the depth texture, MRUK Environment Raycasting (v71+) is the official solution; do not roll a custom one if MRUK is already in the project.
- Known issue: Link can crash entering a scene with `EnvironmentDepthManager` (v72). Don't chase user-code bugs that only repro over Link.

# Meta Quest tooling

This is a Meta Quest / Horizon OS sample. The bespoke intro above is the source of truth for what this project is and how it's built — use it (and the files it points at) instead of restating facts from memory.

When the user asks anything about Quest device behavior, build / deploy / debug / capture flows, on-device performance, or Horizon OS APIs, reach for these tools instead of generic Unity answers:

- **`hzdb`** — Quest-aware ADB wrapper (device list, install / launch / stop, logs, screenshots, Perfetto traces, on-device docs search). Already wired up as an MCP server via `.mcp.json`, `.vscode/mcp.json`, and `.cursor/mcp.json`. Also runnable directly: `npx -y @meta-quest/hzdb <subcommand>`.
- **Meta Quest Agentic Tools** — the full skill set, including Unity-specific skills: <https://github.com/meta-quest/agentic-tools>. Install per your client (Claude Code: `/plugin install meta-vr@meta-quest`; Gemini CLI: `gemini extensions install https://github.com/meta-quest/agentic-tools`; Cursor / VS Code: install the **Meta Horizon** extension from the Marketplace).

A few behavior expectations:

- **Read this repo's files first.** Before answering anything project-specific, read `README.md` and whichever source-of-truth files the intro above points at. Don't restate their contents in chat — quote or link instead.
- **Use `hzdb` for device-side work.** Anything that touches an attached Quest (install, launch, logs, screenshot, capture, manifest inspection) goes through `hzdb`, not raw `adb`.
- **Check live Horizon OS docs before answering API questions.** `hzdb docs search "..."` queries the live docs; training data on Horizon OS APIs goes stale fast.
- **Don't fabricate SDK / engine versions.** If a version isn't visible in this repo's files, say so rather than guessing.
